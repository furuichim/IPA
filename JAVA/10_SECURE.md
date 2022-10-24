# SECUREコーディング

## 入力値検査とデータの無害化(IDS) 

### SQLインジェクションを防ぐ

Webページなどでユーザが入力した値をそのままSQL文で使用しないようにする。
`PreparedStatement`で`?`で入力パラメータを埋め込むようにする。
`?`は利用できるのは、カラムの値だけなので、それ以外の文字列が指定されると、実行時エラーになる。
また、入力値をそのまま、PreparedStatementの入力に指定するのではなく、入力値の妥当性チェックを行う様にする。

```java
public void changePassword(String userId, String password) {
    final String connectionString = "jdbc:postgresql://localhost:5432/test_db";
    final String sql = "update user set " +
                        "user_password=? " +
                        "where user_id=?";

    try (Connection conn = DriverManager.getConnection(connectionString, "postgres", "postgres");
        PreparedStatement statement = conn.prepareStatement(sql)) {

        // 入力値は事前にチェックする
        if (userId.length() > 10 ) {
            // エラー処理
        }
        
        // 入力値は、PreparedStatementの入力値として埋め込む
        statement.setString(1, userId);
        statement.setString(2, password);
        
        int count = statement.executeUpdate();
        if (count == 0) {
            System.out.println("更新失敗");
        }
    } catch (SQLException e) {
        System.out.println(e);
    }
}
```


### 文字列は検査する前に標準化する

クロスサイドスクリプティング(XSS)を防ぐために、`<script>`タグ等が入力値に含まれていないことをチェックする。
しかし、入力値をそのまま正規表現でチェックしても、チェックがすり抜ける場合がある。
例えば、`\uFE64`などのUnicodeが埋め込まれると、環境依存文字となるため、正規表現のパターンでチェックできなくなる。

そこで、不正なタグが含まれていないかなど検査する前に正規化を行う様にする。

```java
public boolean check(String msg) {
    // msgには "\uFE64" + "script" + "\uFE65"が格納されている。

    // まず正規化する
    msg= Normalizer.normalize(msg, Form.NFKC);

    // 不正な文字が含まれていないか検査する
    Pattern pattern = Pattern.compile("[<>]");
    Matcher matcher = pattern.matcher(msg);
    if (matcher.find()) {
        // 不正な文字が見つかった場合の処理
        return false;
    }

    return true;
}
```


### パス名は検査する前に正規化する

ユーザがパスを入力できるプログラムの場合、`..`を使った親ディレクトリアクセスが行われると、意図しないディレクトリをアクセスされる可能性がある。
また、アクセスするパスが最終的にシンボリックリンクになる場合も、意図しないファイルやディレクトリがアクセスされる可能性があります。
このため、パスをまず正規化する。

以下は不適切な例です。

```java
public static void checkFile(String[] args) {
    File file = new File(System.getProperty("user.home") +
                         System.getProperty("file.separator") +
                         args[0]);
    
    // 絶対パスを取得する。
    String absPath = file.getAbsolutePath();
    System.out.println(absPath);
}
```

`getAbsolutePath`メソッドは、正規化していない文字列が返却されます。
例えば、`user.home`が`/usr/home/guest`で、`args[0]`が`../root`の場合、`/usr/home/guest/../root`が返却されます。

そこで、`getAbsolutePath`メソッドの代わりに、`getCanonicalPath`メソッドを使用します。
このメソッドの場合、上記のケースでは、`/usr/home/root`の様に正規化されたパスが返却されます。
また、最終的なパスがシンボリックリンクの場合は、リンボリックリンク先に変換されます。

```java
public static void checkFile(String[] args) {
    try {
        File file = new File(System.getProperty("user.home") +
                             System.getProperty("file.separator") +
                             args[0]);

        // 絶対パスを取得する。
        String absPath = file.getCanonicalPath();

        System.out.println(absPath);
    } catch (IOException e) {
    }
}
```

## 入出力(FIO) 

### ファイル関連エラーを検知し、処理する

ファイル操作を行うメソッドで、正常な処理が行われなかった際に、例外を投げて通知することもあれば、戻り値で通知することもあります。
このため、戻り値を正しく扱う必要があります。

以下は、不適切なコードです。

```java
try {
    File file = new File(args[0]);
    file.delete();
} catch (SecurityException e) {
    e.printStackTrace();
}
```

このコードでは、指定されたパスのファイルが存在に関係なく、正常終了します。
そこで、`File.delete`メソッドを使う場合は、必ず返却値を評価する様にします。

```java
try {
    File file = new File(args[0]);
    boolean isDeleted = file.delete();
    if (isDeleted) {
        System.out.println("Failed in deleting the specified file");
    } else {
        // 削除が成功した場合の処理
    }
} catch (SecurityException e) {
    e.printStackTrace();
}
```

また、`java.nio.file.Files`クラスを使う以下の方法も採用することができます。

```java
try {
    File file = new File(args[0]);
    Path path = file.toPath();
    Files.delete(path);
} catch (SecurityException e) {
    e.printStackTrace();
} catch (IOException e) {
    e.printStackTrace();
}
```


### 不要になったリソースは解放する

明示的にクローズしないとガーベージコレクションの対象にならないリソースは必ず明示的に開放すこと。
特に`InputStream`、`OutputSream`、`FileReader`、`FileWriter`、`Connection`、`Statement`、`ResultSet`は明示的にクローズする必要があります。
※`ResultSet`は`Statement`がクローズされると自動的にクローズされます。


以下は不適切なコードです。
`FileInputStream`は明示的にクローズする必要があります。

```java
try {
    FileInputStream st = new FileInputStream(new File(path));
    int data = 0;
    while ((data = st.read()) != -1) {
        System.out.println(data + " ");
    }
} catch (FileNotFoundException e) {
    e.printStackTrace();
} catch (IOException e) {
    e.printStackTrace();
}
```

以下は明示的に`close`を行う適切なコードです。

```java
FileInputStream st = null;
try {
    st = new FileInputStream(new File(path));
    int data = 0;
    while ((data = st.read()) != -1) {
        System.out.println(data + " ");
    }
} catch (FileNotFoundException e) {
    e.printStackTrace();
} catch (IOException e) {
    e.printStackTrace();
} finally {
    if (st != null) {
        try {
            st.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

上記コードは、try-with-resources文を使った方がよりシンプルに記述できます。

```java
try (FileInputStream st = new FileInputStream(new File(path))) {
    int data = 0;
    while ((data = st.read()) != -1) {
        System.out.println(data + " ");
    }
} catch (FileNotFoundException e) {
    e.printStackTrace();
} catch (IOException e) {
    e.printStackTrace();
}
```

## シリアライズ(SER)

### 開発中のクラスにおいてシリアライズの互換性を維持する

あるクラスのオブジェクトがシリアライズされる時とデシリアライズされる時に同じクラスファイルが使われることが保証される必要があります。
同じクラスのソースコードであっても、ビルドする度にJVMによって任意の`serialVersionUID`が自動的割り当てられます。シリアライズするマシンとデシリアライズするマシンで、同じソースを別々にビルドした場合、異なる`serialVersionUID`が割り当てられるために、デシリアライズ時に実行時例外が発生します。

このため、プログラムで明示的に`serialVersionUID`を割り当てることが推奨されています。
Eclipseでは、自動でランダムな値を割り当てることができる。

```java
public class User implements Serializable {
    // 明示的にserialVersionUIDを割り当てる
    private static final long serialVersionUID = -1047556999647681795L;
    String name;
    int age;
    User() {}
    User(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

## プラットフォームのセキュリティ(SEC)

### センシティブな情報を特権ブロックから信頼境界を越えて漏えいさせない

javaでは、ライブラリ毎にセキュリティ設定を行うことができる。
例えば、以下の設定の場合、ライブラリAのクラスからライブラリBのクラスのメソッドを呼び出すと、より権限の引く方に合わせて実行されるため、セキュリティ例外が発生します。

- ライブラリA - 読み込み権
- ライブラリB - 書き込みと読み込み権

そこで、呼び出し元の権限にかかわらず、呼び出し先ライブラリの権限で処理が実行されるようにすることができます。
これには、[`java.security.AccessController`](https://docs.oracle.com/javase/jp/8/docs/api/java/security/AccessController.html)クラスの`doPrivileged`メソッドを使用します。

- 戻り値が無く、例外がスローされない場合

    ```java
    private static void dpPrivileged1(String key, final String value) {
        AccessController.doPrivileged((PrivilegedAction<Void>)()->{
            System.setProperty(key, value);
            return null;
        });
    }
    ```

- 戻り値を返却する場合

    ```java
    private static String dpPrivileged2(String key) {
        String value = AccessController.doPrivileged((PrivilegedAction<String>)()->{
            String val = System.getProperty(key);
            return  val;
        });

        return value;
    }
    ```

- 例外をスローする場合

    ```java
    private static String dpPrivileged3(String key) throws IOException {
        try {
            String value = AccessController.doPrivileged((PrivilegedExceptionAction<String>)()->{
                // 例外をスローする可能性のある何らかの処理。
            });

            return value;
        } catch (PrivilegedActionException e) {
            throw (IOException)e.getException();
        }
    }
    ```


**特権処理を行うメソッドは `private` にするべきである。**


## 雑則(MSC)

### 繰り返し処理中の基となるコレクションを変更しない

イテレータを使用してコレクションを直接操作する際に、元となるコレクションを変更すると、即座に`ConcurrentModificationException`がスローされます。

以下のコードは、イテレータの操作中に元となるコレクションを変更する事で、`ConcurrentModificationException`例外がスローされる不適切なコードの例です。

```java
try {
    List<String> list = new ArrayList<>();
    list.add("a");
    list.add("b");
    list.add("c");
    list.add("d");

    Iterator<String> iter = list.iterator();
    while(iter.hasNext()) {
        String s = iter.next();
        if (s.equals("b")) {
            list.remove(s);
        }
    }
} catch (ConcurrentModificationException e) {
    e.printStackTrace();
}
```

`Iterator.remove`を呼び出すことで、`ConcurrentModificationException`例外をスローさせないようにできます。

```java
try {
    List<String> list = new ArrayList<>();
    list.add("a");
    list.add("b");
    list.add("c");
    list.add("d");

    Iterator<String> iter = list.iterator();
    while(iter.hasNext()) {
        String s = iter.next();
        if (s.equals("b")) {
            // イテレータから削除する
            iter.remove();
        }
    }

    System.out.println(list);       // -> [a, c, d]
} catch (ConcurrentModificationException e) {
    e.printStackTrace();
}
```

### シングルトンオブジェクトのインスタンスを複数作らない

シングルトンクラスは以下のルールで実装します。

- コンストラクタを`private`にする
- マルチスレッド環境下で利用されることを考慮し、並列に初期化処理が行われない様にする
- Serializableインタフェースを実装しない
- クラスのクローンを作らないようにする
- 独自のクラスローダでロードされたクラスがガーベージコレクションで回収されないようにする


```java
public class TestSingleton {
    private static TestSingleton instance = new TestSingleton();
    
    // privateのコンストラクタ
    private TestSingleton() {
        // 初期化処理
    }

    public static synchronized TestSingleton getInstance() {
        return instance;
    }
}
```
