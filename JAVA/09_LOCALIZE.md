# ローカライズ

## 概要
- ローカライズとは特定の地域にアプリケーションを対応させること
- 特定の地域とは、国や言語、文化などによって一括りに考えることができる地理上の範囲で、ロケールで定義される。
- ローカライズではメニューやメッセージなどを様々な言語で表示できるようにして、数値と日付・通貨なども、特定の地域で使われている形式で表示する。

## Localeクラス

### Localeの情報
[`java.util.Locale`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/Locale.html)クラスは以下の情報を持つ

- `language` - 2桁の英子文字で表される言語コード(ISO639)
    `ja`, `en`, `fr` 等
- `country` - 2桁の英大文字で表される国コード(ISO3166)
    `JP`, `US`, `GB`, `CN` 等
- `variant` - 任意の値

### コンストラクタ

```java
// 言語コードだけをとるコンストラクタ
Locale(String language);

// 言語コードと国コードをとるコンストラクタ
Locale(String language, String country);

// 言語コードと国コードと任意の値をとるコンストラクタ
Locale(String language, String country, String variant);

```

ロケールの作成例
ロケールのコンストラクタでは、言語コードと国コードの妥当性はチェックされないことに注意！

```java
// 日本語、日本
Locale locale1 = new Locale("ja", "JP");

// フランス語、カナダ
Locale locale1 = new Locale("fr", "CA");

// 英語、カナダ
Locale locale1 = new Locale("en", "CA");
```

- システムデフォルトのロケール

`Locale.getDefault()`メソッドでシステムデフォルトのロケールを取得することができる。

```java
// システムデフォルトのロケールを取得する。
Locale locale = Locale.getDefault();

// 言語コードを出力
System.out.println(locale.getLanguage());

// 国コードを出力
System.out.println(locale.getCountry());

// 任意の値を出力
System.out.println(locale.getVariant());
```

### ロケール定数

Localeクラスで定義済みの定数を使うことで、ロケールを取得することができる。

- `Locale.ENGLISH`  - 英語を表すロケール定数(国コードは空文字になる)
- `Locale.CHINESE`  - 中国語を表すロケール定数(国コードは空文字になる)
- `Locale.JAPANESE` - 日本語を表すロケール定数(国コードは空文字になる)
- `Locale.CHINA`    - 中国の国を表すロケール定数
- `Locale.CANADA`   - カナダの国を表すロケール定数(国コードはCA、言語コードは**en**)
- `Locale.JAPAN`    - 日本の国を表すロケール定数(国コードはJP、言語コードはjp)
- `Locale.UK`       - イギリスの国を表すロケール定数
- `Locale.US`       - アメリカの国を表すロケール定数


### ビルダー

`Builder`パターンに基づいた[`Locale.Builder`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/Locale.Builder.html)クラスを使うことで、ロケールを取得することができる。

ビルダーを使ったロケール作成の例

```java
// ビルダーを作成する
Locale.Builder builder = new Locale.Builder();

// ビルダーに言語コードを設定する
builder.setLanguage("ja");

// ビルダーに国コードを設定する
builder.setCountry("JP");

// ビルダーでロケールを作成する
Locale locale = builder.build();
```

上記のコードは以下のように記述することもできます。

```java
Locale locale = new Locale.Builder()
    .setLanguage("ja")
    .setCountry("JP")
    .build();
```

`Builder`を使う場合、存在しない言語コードや国コードを指定した時点で、`IllformedLocaleException`例外がスローされるので、バグを防ぐことができる。

## Properiesクラス

プロパティとは、キー＆バリューの形式で表されるデータのこと。
データはテキストファイルまたはXMLファイルで記述することができる。
ファイルはプロパティファイルと呼ぶ。

### テキスト形式のプロパティファイル

ファイル拡張子は任意であるが、慣習的に`properties`が使われる。
**#** か **!** で始まる行は、コメントとして扱われる。

```java
# properties for the database
host=192.168.0.3
name=testdb
user=root
password=admin
```

### XML形式のプロパティファイル

```java
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
    <comment>Properties for the database</comment>
    <entry key="host">192.168.0.3</entry>
    <entry key="name">testdb</entry>
    <entry key="user">root</entry>
    <entry key="password">admin</entry>
</properties>
```

### プロパティファイルのロード

FileInputStream もしくは FileReader クラスを使用する。
[`java.util.Properties`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/Properties.html)の`load`メソッドでプロパティをロードする。

- `public void load(InputStream inStream)`
    入力バイト・ストリームからキーと要素が対になったプロパティ・リストを読み込みます。

    ```java
    try (InputStream in = new FileInputStream("example.properties")) {
        // プロパティオブジェクトを作成する
        Properties props = new Properties();

        // プロパティファイルを読み込む
        props.load(in);

        // プロパティで定義されている内容を取得する。
        System.out.println(props.getProperty("A"));
    } catch (IOException e) {
        e.printStackTrace();
    }
    ```

### プロパティの値の取得
[`java.util.Properties`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/Properties.html)インスタンスに対して、以下のメソッドを呼び出す。

- `String getProperty(String key)`
    指定されたキーを持つプロパティを、プロパティリストから探します。
    プロパティが見つからなかった場合は、`null`が返却される。

- `String getProperty(String key, String defaultValue)`
    指定されたキーを持つプロパティを、プロパティリストから探します。
    プロパティが見つからなかった場合は、引数で指定したデフォルト値が返却される。


### プロパティ値の列挙

[`java.util.Properties`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/Properties.html)クラスは、`Hashtable<Object, Object>`を継承しているため、`Hashtable`として扱うことができる。
このため、Mapインタフェースのデフォルトメソッドである`forEach`を使用することができる。

```java
// すべてのプロパティの値を列挙する
props.forEach((key, value) -> {
    System.out.println(key + ":" + value);
});
```

また、Propertiesクラスの`list`メソッドを使用することで、キーとバーの一覧を出力することができる。

```java
props.list(System.out);
```


## リソースバンドル

- 言語や表示形式（リソース）をロケールごとに１つにまとめたもの
- 実体は以下の２つのいずれか
    + クラスファイル
    + プロパティファイル
- プロパティファイルの場合、拡張子を`.properties`にする必要がある
- クラスパスが設定されているディレクトリ上に置く

### リソースバンドルの名前

```java
基底名_言語コード_国コード
MyResources_ja_JP
```

### リソースバンドルの作成

```java
// ロケールの指定なしでリソースバンドルを読み込む
// 拡張子は不要
ResourceBundle rb = ResourceBundle.getBundle("MyResources");

// 明示的にロケールを指定してリソースバンドルを読み込むこともできる
ResourceBundle rb = ResourceBundle.getBundle("MyResources", Locale.US);
```

指定したリソース・バンドルが見つからなかった場合は、`MissingResourceException`がスローされる。

[`java.util.ResourceBundle`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/ResourceBundle.html)はリソースバンドルを探して、読み込む機能があるが、以下の優先順位で検索を行う。

- クラスファイルがプロパティファイルよりも優先される。
- 言語コードと国コードが一致するリソースバンドルが優先される。
- 次に言語コードのみが一致するリソースバンドルが優先される。
- 最後に言語コードも国コードも指定されていないリソースバンドルが検索される。

つまり、検索順は、基底名が「MyResources」、ロケールが「ja_JP」の場合は以下の順になる。
1. MyResources_ja_JP.class
1. MyResources_ja_JP.properties
1. MyResources_ja.class
1. MyResources_ja.properties
1. MyResources.class
1. MyResources.properties


### クラスを使用したリソース・バンドル

`ResourceBundle` クラスのサブクラスである以下の２つのいずれかを継承したクラスを作成する
- [`java.util.ListResourceBundle`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/ListResourceBundle.html) クラス
    ソースコード内に記述したリソースからリソース・バンドルを作成
    **`Object[][]`** を返却する`getContents()`メソッドを実装する。

- [`java.util.PropertyResourceBundle`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/PropertyResourceBundle.html) クラス
    プロパティファイルからリソース・バンドルを作成


```java
public class SpanisResources extends ListResourceBundle {
    @Override
    protected Object[][] getContents() {
        return new Object[][] {
            { "Thank you.", "Gracias." },
            { "Hello.", "De nada." }
        };
    }
}

...
// クラスで定義しているリソースバンドルを取得する。
ResourceBundle rb = ResourceBundle.getBundle("q14.SpanisResources");

// "Thank you."に対するバリュー
System.out.println(rb.getString("Thank you.")); // Gracias.

// "Hello."に対するバリュー
System.out.println(rb.getString("Hello."));     // De nada.
```

※リソースバンドルがパッケージされている場合は、パッケージ名での修飾が必要となる。


## フォーマット


### 主なフォーマットクラス

- 数値及び通貨
    - [`java.text.NumberFormat`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/text/NumberFormat.html) - 抽象クラス
    - [`java.text.DecimalFormat`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/text/DecimalFormat.html)
    - [`java.text.DecimalFormatSymbols`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/text/DecimalFormatSymbols.html)

- 日付および時刻
    - [`java.time.format.DateTimeFormatter`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/time/format/DateTimeFormatter.html)
    - [`java.text.DateFormat`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/text/DateFormat.html) - 抽象クラス
    - [`java.text.SimpleDateFormat`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/text/SimpleDateFormat.html)

- テキストメッセージ
    - [`java.text.MessageFormat`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/text/MessageFormat.html)
    - [`java.text.ChoiceFormat`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/text/ChoiceFormat.html)


### NumberFormatクラス

NumberFormatクラスは抽象クラスのため、以下のスタティックメソッドでインスタンスを取得します。

- `public static final NumberFormat getNumberInstance()`
    現在のデフォルトのFORMATロケールに対する汎用数値フォーマットを返します。
    これは、getNumberInstance(Locale.getDefault(Locale.Category.FORMAT))の呼び出しと同等です。

    ```java
    NumberFormat fmt = NumberFormat.getInstance();
    ```

- `public static NumberFormat getNumberInstance(Locale inLocale)`
    指定されたロケールに対する汎用数値フォーマットを返します。

    ```java
    NumberFormat fmt = NumberFormat.getNumberInstance(new Locale("ja", "JP"));
    ```

- `public static NumberFormat getCurrencyInstance(Locale inLocale)`
    指定されたロケールに対する通貨フォーマットを返します。

    ```java
    NumberFormat fmt = NumberFormat.getCurrencyInstance(Locale.GERMANY);
    ```

- `String format(long number)`
    フォーマットの特殊化です。

    ```java
    // 日本のロケールで金額をフォーマットする
    NumberFormat fmt_jp = NumberFormat.getCurrencyInstance(Locale.JAPAN);
    System.out.println(fmt_jp.format(123456));      // ￥123,456

    // アメリカのロケールで金額をフォーマットする
    NumberFormat fmt_us = NumberFormat.getCurrencyInstance(Locale.US);
    System.out.println(fmt_us.format(123456));      // $123,456.00

    // イギリスのロケールで金額をフォーマットする
    NumberFormat fmt_uk = NumberFormat.getCurrencyInstance(Locale.UK);
    System.out.println(fmt_uk.format(123456));      // £123,456.00
    ```

- `public Number parse(String source)`
    指定された文字列の先頭からテキストを解析して数値を生成します。
    メソッドは指定された文字列のテキスト全体に使用されない場合もあります。
    数値の解析の詳細については、parse(String, ParsePosition)メソッドを参照してください。

    ```java
    try {
        // 日本のロケールで金額をパースする
        NumberFormat fmt_jp = NumberFormat.getCurrencyInstance(Locale.JAPAN);
        Number value1 = fmt_jp.parse("￥123,456");
        System.out.println(value1);         // 123456

        // アメリカのロケールで金額をパースする
        NumberFormat fmt_us = NumberFormat.getCurrencyInstance(Locale.US);
        Number value2 = fmt_us.parse("$123,456.00");
        System.out.println(value2);         // 123456
    } catch (ParseException e) {
        System.out.println(e);
    }
    ```


### DecimalFormatクラス

- `public DecimalFormat(String pattern)`
    デフォルトのFORMATロケールに対して、指定されたパターンと記号を使ってDecimalFormatを作成します。
    これは、国際化が主要な問題でない場合は、DecimalFormatを得るためには簡単な方法です。

    ```java
    DecimalFormat fmt1 = new DecimalFormat("###,###.###");
    System.out.println(fmt1.format(1234567));       // 1,234,567

    DecimalFormat fmt2 = new DecimalFormat("###,###.000");
    System.out.println(fmt2.format(1234567));       // 1,234,567.000

    DecimalFormat fmt3 = new DecimalFormat("###");
    System.out.println(fmt3.format(1234567));       // 1234567

    DecimalFormat fmt4 = new DecimalFormat("#");
    System.out.println(fmt4.format(1234567));       // 1234567

    DecimalFormat fmt5 = new DecimalFormat("$###,###.00");
    System.out.println(fmt5.format(1234567));       // $1,234,567.00

    // \u00a5は通貨単位を表す
    DecimalFormat fmt6 = new DecimalFormat("\u00a5###,###.00");
    System.out.println(fmt6.format(1234567));       // ¥1,234,567.00
    ```


### MessageFormatクラス

- `public MessageFormat(String pattern)`
    デフォルトのFORMATロケールと指定されたパターンのためのMessageFormatを構築します。
    コンストラクタはロケールを設定してからパターンを解析し、含まれているフォーマット要素についてサブフォーマットのリストを作成します。
    パターンとその解釈はクラスの概要で指定されています。

    ```java
    MessageFormat fmt = new MessageFormat("Goodbye {0}");
    System.out.println(fmt.format(new Object [] {"Jiro"})); 
    ```

- `public static String format(String pattern, Object... arguments)`
    指定されたパターンを使ってMessageFormatを作成し、それを使用して指定された引数をフォーマットします。
    これは次の記述と同等です。

    ```java
    System.out.println(MessageFormat.format("Hello {0}", new Object [] {"Taro"})); 
    ```

