<style>
strong {
    background: linear-gradient(transparent 0%, #6f6 0%) !important;
}

strong * {
    background: linear-gradient(transparent 0%, #6f6 0%) !important;
}

</style>
# MODULEシステム

## 利用可能なモジュール

以下のコマンドでシステムで利用可能なモジュール一覧を表示することができる。

```bash
java --list-modules
```

## モジュール記述子
`module-info.java`をビルドしたもの。
つまり、`module-info.class`のこと。

`module-info.java`の記述内容。

```java
module モジュール名 {
    exports xlib;
    requires java.base;
    requires foo;
}
```


- `exports`
    公開するパッケージを指定する。

- `requires`
    必要となるモジュールを指定する。
    `java.base`はすべてにモジュールで必要となるため、記述を省略する事ができる。

- `requires transitive`
    必要となるモジュールを指定する。
    この指定を行うことで、指定したモジュールを間接的にエクスポートしたことにできる。


## ビルド方法

`--module-path`オプションで依存するモジュールのパスを指定する。

`out\foo`モジュールに依存している`client`モジュールをコンパイルする場合、以下の様にコンパイルします。


```java
javac -d out\client --module-path out\foo src\clientmodule-info.java src\client\app\main.java
```

## 実行方法

`--module-path`または`-p`オプションでモジュールが格納されている場所を指定する。
`--module`または`-m`オプションで、モジュール名とエントリ・ポイントとなるクラスの完全修飾名を指定する。

```java
// java --module-path モジュールのあるディレクトリ --module モジュール名/パッケージ名.クラス名
java --module-path out\foo;out\client --module client/app.Main
```

jarファイルにモジュールが格納されている場合は、`--module-path`でjarファイルのあるディレクトリを指定する。
modulesディレクトリにjarファイルが配置されている場合は、以下のコマンドを実行する。

```java
java --module-path modules --module client/app.Main
```

## 無名モジュール

CLASSPATH上に存在して、モジュール名を持たない(module-info.class)モジュールを無名モジュールと呼びます。
無名モジュールは以下として挙動します。

- すべてのパッケージをexportsする。
- モジュールパス上のすべてのモジュールをrequiresする。
- 名前付きモジュールから無名モジュールを参照することはできない。

## 自動モジュール

モジュールパス上に存在して、モジュール名を持たない(module-info.class)モジュールを自動モジュールと呼びます。
自動モジュールは以下として挙動します。

- すべてのパッケージをexportsする。
- モジュールパス上のすべてのモジュールをrequiresする。
- 名前付きモジュールから自動モジュールの参照が可能。

自動モジュールはモジュール名がないため、以下のルールでモジュール名を決定する。

1. マニフェストファイル(MANIFIEST.MF)のAutomatic-Module-Name属性の値
1. マニフェストファイル(MANIFIEST.MF)で指定されていない場合はjarファイル名で解決
    `commons-beanutils-1.9.3.jar`の場合、`commons.beanutils`となる。


## モジュールの依存性の確認

- `--describe-module`オプション
    javaコマンドのオプションで、モジュール識別子の情報を表示します。

    ```java
    java --describe-module client
    ```

- `--show-module-resolution`オプション
    javaコマンドのオプションで、順次呼び出されるモジュール名を標準出力します。

    ```java
    java --show-module-resolution -p modules -m client/app.Main
    ```

- `jdeps`コマンド
    クラスファイルの依存関係をパッケージまたはクラスレベルで表示する。
 
    ```java
    jdeps --module-path modules modules\client.jar
    ```
    
    - `-summary` または `-s` オプション
        依存関係のサマリーのみを出力
    
    - `jdkinternals` オプション
        JDK内部のAPIでクラス・レベルの依存関係を検索
    
    - `dotoutpu` オプション
        `<archive-file-name>.dot`という名称のファイルとアーカイブ間の依存関係を一覧表示した`summay.dot`ファイルが生成される。


## カスタムランタム・イメージの作成

SE11からJREが単体で提供されなくなっている。
そこで`jlink`コマンドを使って、配布するアプリケーションにJREを同梱する。


```java
// module-info.javaとアプリのクラスをビルドする。
javac -d out src\client\app\Main.java src\client\module-info.java

// jarにクラスをアーカイブする。
// out 配下のclassファイルをmodules\client.jarにアーカイブする
jar -cvf modules\client.jar -C out .

// modulesフォルダ配下のclientモジュールをと追加して、必要なＪＲＥをまとめてclientapp1に出力する。
jlink --module-path modules --add-modules client --output clientapp1
```


## ServiceLoaderの使用

まず、サービスを提供するモジュールの`module-info.java`で`provides ～ with`を使って提供するサービスを記述します。

例えば、以下の例では。
- fooモジュールで、`xlib`パッケージを公開する
- xlib.MyInterインターフェースのサービスを公開する。
- xlib.MyInterインターフェースの実態としては、xlib.XTestとxlib.YTestとする。

```java
module foo {
    exports xlib;
    provides xlib.MyInter with xlib.XTest, xlib.YTest;
}
```

上記サービスを使用するclientモジュールの`module-info.java`は以下の様に記述する。
この例では、
- clientモジュールはfooモジュールを必要としている。
- clientモジュールはxlib.MyInterのサービスを利用する。

```java
module client {
    requires foo;
    uses xlib.MyInter;
}
```


そして、clientでは以下のコードでxlib.MyInterを利用する。

```java
package app;
import xlib.MyInter;
import java.util.Iterator;
import java.util.ServiceLoader;

public class Main {
    public static void main(String[] args) {
        // ServiceLoaderを使ってMyInterインタフェースを実装しているクラスを取得する
        ServiceLoader<MyInter> loader = ServiceLoader.load(MyInter.class);
        // 複数のクラスが取得できるため、iteratorで一つずつ取り出す。
        Iterator<MyInter> iter = loader.iterator();
        while (iter.hasNext()) {
            MyInter obj = iter.next();
            System.out.println(obj.sayHello());
        }
    }
}
```