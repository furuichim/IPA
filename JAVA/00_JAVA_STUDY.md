# Java言語勉強メモ

## 変数
### 変数の種類  
- `Instance variables` - non-static fields
- `Class variables` - static fields
- `Local variables` -  store temporary state inside a method
- `Parameters` - variables that provide extra information to a method

### 変数名
- 先頭は英字、$、_のみが使用可能  
- ただし、$、_は使用しないことが推奨されている
- 先頭以外は、英字、数字、$、_であるが、英字と数字だけのキャメルケースが推奨されている。
- 変数名は、意味不明な省略語を使用しないこと
- static finalだけは、大文字でスネイクケース(アンダーバーでの結合)が推奨されている

### 変数の初期値
**フィールドは明示的に初期化を行わない場合、変数には以下のデフォルト値が設定される**。  
- `byte` - `0`  
- `short` - `0`  
- `int` - `0`  
- `long` - `0L`  
- `float` - `0.0f`  
- `double` - `0.0d`  
- `char` - `\u0000`  
- `boolean` - `false`  
- `String or any object` - `null`  

しかし、**ローカル変数には明示的なデフォルト値は与えられない**。  
このため、**未初期化のローカル変数を参照するプログラムは、コンパイルエラー**になる。

その他コンパイルエラーになる例。

```java
int x = 0811;           // 8進数は0-7までの数字しか使えない。
byte a = 0b10000000;    // byte型は-128～127までしか指定できない。
int b = 100L;           // int型にlong型をキャストしないで代入できない。
float c = 9.3;          // 少数リテラルはdouble型であるので、キャストが必要である。
```

以下はコンパイルエラーにならない。

```java
byte a = (byte)0b10000000;  // 整数リテラルはint型であり、byteに収まらない場合も、キャストすればOK。
float c = (float)9.3;       // 少数リテラルはdouble型であるので、キャストが必要である。
```

### 整数リテラル
`long`型の値は、最後に`L`か`l`を付与する。  
`L`か`l`をつけない場合は`int`となる。  

```java
long x = 12345L;
int y = 1234;
byte a = 123;     // 数値リテラルはint型であるが、byteの範囲なので、暗黙的にキャストされる。
short b = 1234;   // 数値リテラルはint型であるが、shortの範囲なので、暗黙的にキャストされる。
```

※大文字の`L`を使うことが推奨されている(小文字の`l`は数字の`1`と判別することが難しいため)  

### 10進数、16進数、8進数
C言語と同じように、以下のプレフィックスをつける。  

```java
// The number 26, in decimal
int decVal = 26;
//  The number 26, in hexadecimal
int hexVal = 0x1a;
// The number 26, in binary
int binVal = 0b11010;
```

### 浮動小数点リテラル
- `float`型の場合は、最後に`F`または`f`を付与する。  
- `double`型の場合は、最後に`D`または`d`を付与する。  
- `f`や`d`を指定しないと、`double`になる。

```java
double d1 = 123.4;
// same value as d1, but in scientific notation
double d2 = 1.234e2;
float f1  = 123.4f;
```

***少数リテラルは暗黙的にキャストされないため、以下はコンパイルエラーになる***。

```java
float f2  = 123.4;      // 少数リテラルはdouble型のためfloat型にキャストされない。
```

### 整数リテラルでのアンダースコア
整数リテラルは`_`を入れて見やすくすることができる。  
ただし、整数リテラルの先頭と末尾には入れられない。  
また、記号の前後にも入れることができない。

```java
long creditCardNumber = 1234_5678_9012_3456L;
long socialSecurityNumber = 999_99_9999L;
float pi =  3.14_15F;
long hexBytes = 0xFF_EC_DE_5E;
long hexWords = 0xCAFE_BABE;
long maxLong = 0x7fff_ffff_ffff_ffffL;
byte nybbles = 0b0010_0101;
long bytes = 0b11010010_01101001_10010100_10010010;
```

NGな例

```java
int a = _123;
int b = 1234_;
long c = 12345_L;
float d = 1_.123f;
float e = 1._123f;
float f = 1.123_f;
int g = 0x_12345;
```

### 文字のリテラル
`char`型変数に対しては、文字リテラルが使用できる。  
文字リテラルは、`'`(シングルクォーテーション)で1文字を囲む。  
文字リテラルは`char`型であるので、整数型変数にもキャスト無しで使用できる。

```java
char a = 'a';
byte b = 'b';
short c = 'c';
int d = 'd';
long e = 'e';
```

また、`char`型変数は、Unicodeの文字コードで初期化することもできます。  

```java
char a = '\u30A2';      // \uXXXXの形式
char a = 0x30a2;        // XXXXの数字
```

ただし、数値で初期化する場合は、0～65535までの範囲となる。  
`char`型は符号無し16bit整数であることに注意が必要。  
負数を入れると、コンパイルエラーになる。  

### キャスト
大きな型から小さな型への代入は、明示的なキャストが必要。  
小さな型から大きな型への代入は、明示的なキャストは不要。

OKな例。

```java
byte a = 0xf;
short b = a;
int c = b;
long d = c;
float e = 0.1f;
double f = e;
```

NGな例。

```java
double f = 0.1;
float e = f;    // コンパイルエラー
long d = 1L;
int c = d;      // コンパイルエラー
short b = c;    // コンパイルエラー
byte a = b;     // コンパイルエラー
```

### 配列
- `Array`は、作成された時に、サイズが固定される。
- ブランケット`[]`を変数名の後ろにすることもできるが、推奨されない。

```java
int[] aa;
int aa[];
```

- しかし、以下はコンパイルエラーになる。  

```java
int[] a[];
int[][] b[];
```

- 配列の要素数は`int`型であり、`long`を指定するとコンパイルエラーになる。  

### 配列の初期化
- サイズを指定して初期化（int）  
***以下の例では、各要素が`0`で初期化される***。

```java
int [] a = new int[10];
```

- サイズを指定して初期化（String）  
***以下の例では、各要素が`null`で初期化される***。

```java
String [] b = new String[10];
```

- 実際に値をリテラルで指定した初期化  

```java
int[] b = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
```

あるいは

```java
int[] b = new int[]{1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
```

実際に値をリテラルで指定する場合は、 ***配列のサイズを指定して`new`してはいけない***。
以下は、コンパイルエラーになる。  

```java
int[] b = new int[10]{1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
```

以下は、同じ意味になります。

```java
int[] a = {};               // サイズ０の配列
int[] a = new int[0];
```

以下はコンパイルエラーになる。

```java
int[][] a = new int[] {};
```

以下はOK。  

```java
int[][] a = new int[][] {};
```

初期化子は、`new`と同時に指定する必要がある。
以下はコンパイルエラーになる。

```java
int[] a;
a = {2,3};
```

以下はOK

```java
int[] a;
a = new int[]{2,3};
```

- 多次元配列の初期化  
javaでの多次元配列は、各次元でサイズが異なっていてもよい。

```java
String[][] names = {
    {"Yamada", "Itou", "Ogawa"},
    {"Taro", "Shinji", "Yumeko", "Akira"}
}
```

```java
String[][] a = new String[2][3];
```

```java
String[][] a = new String[2][];
a[0] = new String[3];
a[1] = new String[5];
```


### 配列のコピー  
- `System.arraycopy`メソッドが用意されている。  

```java
char[] copyFrom = { 'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i'};
char[] copyTo = new char[7];
System.arraycopy(copyFrom, 2, copyTo, 0, 7);
System.out.println(new String(copyTo));
```

- `java.util.Arrays`クラスは便利な配列操作のメソッドを用意している。  

```java
char[] copyFrom = { 'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i'};
char[] copyTo = java.util.Arrays.copyOfRange(copyFrom, 2, 9);
System.out.println(new String(copyTo));
```

## オペレータ

### 優先順位

1. `postfix` - `expr++` `expr--`
1. `unary` - `++expr` `--expr` `+expr` `-expr` `~` `!`
1. `multiplicative` - `*` `/` `%`
1. `additive` - `+` `-`
1. `shift` - `<<` `>>` `>>>`
1. `relational` - `<` `>` `<=` `>=` `instanceof`
1. `equality` - `==` `!=`
1. `bitwise AND` - `&`
1. `bitwise exclusive OR` - `^`
1. `bitwise inclusive OR` - `|`
1. `logical AND` - `&&`
1. `logical OR` - `||`
1. `ternary` - `? :`
1. `assignment` - `=` `+=` `-=` `*=` `/=` `%=` `&=` `^=` `|=` `<<=` `>>=` `>>>=`

### instanceof  
継承しているクラスと実装しているインタフェースについても`instanceof`でチェックすることができる。  

```java
interface Human {};
public class Parent {}
public class Children extends Parent implements Human {}

public static void Test() {
    Parent obj1 = new Parent();
    Parent obj2 = new Children();
    System.out.println(obj1 instanceof Parent);     // true
    System.out.println(obj1 instanceof Children);   // false
    System.out.println(obj1 instanceof Human);      // false
    System.out.println(obj2 instanceof Parent);     // true
    System.out.println(obj2 instanceof Children);   // true
    System.out.println(obj2 instanceof Human);      // true
}
```

## Expression
`Expression`は変数とオペレータと関数呼び出しから構成される。  
以下のサンプルコードの緑色で強調した部分`Expression`である。

<pre>
int <strong>cadence = 0</strong>;
<strong>anArray[0] = 100</strong>;
System.out.println(<strong>"Element 1 at index 0: " + anArray[0]</strong>);
int <strong>result = 1 + 2</strong>; // result is now 3
if (<strong>value1 == value2</strong>) 
    System.out.println(<strong>"value1 == value2"</strong>);
</pre>

## Statement
### Expression Statment  
`Expression`にセミコロンをつけたもの

```java
// assignment statement
aValue = 8933.234;
// increment statement
aValue++;
// method invocation statement
System.out.println("Hello World!");
// object creation statement
Bicycle myBike = new Bicycle();
```

### declaration statements  
以下のような宣言のこと  

```java
// declaration statement
double aValue = 8933.234;
```

### control flow statements  

- if-then Statement  

    ```java
    void applyBrakes() {
        // the "if" clause: bicycle must be moving
        if (isMoving) { 
            // the "then" clause: decrease current speed
            currentSpeed--;
        }
    }
    ```

- if-then-else Statement  

    ```java
    void applyBrakes() {
        if (isMoving) {
            currentSpeed--;
        } else {
            System.err.println("The bicycle has already stopped!");
        } 
    }
    ```

- switch Statement  
    `switch Statement`では、`String`型を`case`に指定することができる。  
    しかし、 ***判定する`String`オブジェクトが`null`でないことを事前にチェックしておくこと***。  
    そうしないと、`NullPointerException`が発生する可能性がある。

    ```java
    public class SwitchDemo {
        public static void main(String[] args) {

            int month = 8;
            String monthString;
            switch (month) {
                case 1:  monthString = "January";
                        break;
                case 2:  monthString = "February";
                        break;
                case 3:  monthString = "March";
                        break;
                case 4:  monthString = "April";
                        break;
                case 5:  monthString = "May";
                        break;
                case 6:  monthString = "June";
                        break;
                case 7:  monthString = "July";
                        break;
                case 8:  monthString = "August";
                        break;
                case 9:  monthString = "September";
                        break;
                case 10: monthString = "October";
                        break;
                case 11: monthString = "November";
                        break;
                case 12: monthString = "December";
                        break;
                default: monthString = "Invalid month";
                        break;
            }
            System.out.println(monthString);
        }
    }
    ```

    `switch`ステートメントの条件式で使用できる型は以下になる。
    - `char`
    - `byte`
    - `short`
    - `int`
    - `String`
    - `enum`

    `enum`を条件式にした場合は、caseはordinal値でなくenum値を指定する必要がある。

    ***`long` `float` `double`は使用できないことを注意！***  
    また、`switch`の`case`には、nullでない、同じ型の、直値か ***定数(`final`宣言されている物)*** しか指定できない。  
    `switch`の`default`の位置は最後でなくても良い。
    `switch`の`case`で同じ値を指定するとコンパイルエラーになる。

- while Statement  

    ```java
    class WhileDemo {
        public static void main(String[] args){
            int count = 1;
            while (count < 11) {
                System.out.println("Count is: " + count);
                count++;
            }
        }
    }
    ```

- do-while Statement  

    ```java
    class DoWhileDemo {
        public static void main(String[] args){
            int count = 1;
            do {
                System.out.println("Count is: " + count);
                count++;
            } while (count < 11);
        }
    }
    ```

- for Statement  
    ***`C`や`C++`と異なり、`for`の初期化ステートメントで宣言した変数は、`for`の外で使えない***。

    ```java
    class ForDemo {
        public static void main(String[] args){
            for (int i = 1; i < 11; i ++) {
                System.out.println("Count is: " + i);
            }
        }
    }
    ```

    `for`の初期条件で単一の型しか宣言できない。  
    したがって、以下はコンパイルエラーになる。

    ```java
    for (int i = 0, long j = 0; i < 100; ++ i) {
    }
    ```

    `for`で条件文を省略すると、`true`を指定したのと同じになる。  

- enhanced for Statement  
    `Collections`と`arrays`に対するイテレーションをする時に使用できる。  

    ```java
    class EnhancedForDemo {
        public static void main(String[] args){
            int[] numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
            for (int item : numbers) {
                System.out.println("Count is: " + item);
            }
        }
    }
    ```

- unlabled break Statement  
    `for` `while` `do-while` `switch`の内部のステートメントを中断するために使用する。  

    ```java
    for (i = 0; i < arrayOfInts.length; i ++) {
        if (arrayOfInts[i] == searchfor) {
            foundIt = true;
            break;
        }
    }
    ```

- labeled break Statement  
    `for` `while` `do-while` `switch`の外側のステートメントを中断するために使用する。  

    ```java
    search:
        for (i = 0; i < arrayOfInts.length; i ++) {
            for (j = 0; j < arrayOfInts[i].length; j ++) {
                if (arrayOfInts[i][j] == searchfor) {
                    foundIt = true;
                    break search;
                }
            }
        }
    ```

- unlabled continue Statement  
    内側のループの繰り返し処理をスキップする。  

    ```java
    // ０から９までの奇数の和を求める
    // 1+3+5+7+9=25となる
    int amount = 0;
    for (int i=0; i < 10; ++ i) {
        if (i % 2 == 0) {
            continue;
        }
        amount += i;
    }
    System.out.println(amount);
    ```

- labled continue Statement  
    外側のループの繰り返し処理をスキップする。  

    ```java
    // この処理では、amount += iは全く通らないため、
    // amount=0である。
    int amount = 0;
    Test:
    for (int i = 0; i < 10; ++ i) {
        for (int j = 0; i < 10; ++ i) {
            if (j % 2 == 0) {
                continue Test;
            }
            amount += i;
        }
    }
    System.out.println(amount);
    ```

- return Statement  

    ```java
    int Test(int value) {
        if (value % 2 == 0) {
            return value % 2;
        }

        return 0;
    }
    ```


## Block
`{}`によってグループ化された0個以上のStatementグループ  
以下にBlockのイメージを示す。

```java
class BlockDemo {
     public static void main(String[] args) {
          boolean condition = true;
          if (condition) { // [begin] block 1
               System.out.println("Condition is true.");
          } // [end] block one
          else { // [begin] block 2
               System.out.println("Condition is false.");
          } // [end] block 2
     }
}
```

## Class  
### Access level modifiers  
- `public` - すべてのパッケージのすべてのクラスから参照可能  
- `protected` - 同じパッケージ内のクラスと、サブクラスから参照可能  
- `package-private` - 同じパッケージ内のクラスから参照可能  
- `private` - 同一クラス内からだけ参照可能  

各クラスから`access modifiers`毎のアクセス可否  

|From\To|public|protected|package private|private|
|----|----|----|----|----|
|同じクラス|〇|〇|〇|〇|
|同一パッケージのサブクラス|〇|〇|〇|×|
|同一パッケージのクラス|〇|〇|〇|×|
|他パッケージのサブクラス|〇|〇|×|×|
|他パッケージのクラス|〇|×|×|×|


----
パッケージ`gomi`に以下の`Parent1`クラスがあるとする。

```java
package gomi;   // 一つ目のパッケージ

public class Parent1 {
    private String a = "a";     // privateなフィールド
    String b = "b";             // package-privateなフィールド
    protected String c = "c";   // protectedなフィールド
    public String d = "d";      // publicなフィールド

    public Parent1() {
    }
}
```

パッケージ`gomi`の`Parent1`クラスを継承する、同一パッケージの`Child1`クラスからは`private`なフィールドだけが見えない。

```java
package gomi;   // 同一パッケージ

public class Child1 extends Parent1 {
    public void test() {
        //String a = this.a;  // これは見えない
        String b = this.b;
        String c = this.c;
        String d = this.d;
    }
}
```

パッケージ`gomi`の`Parent1`クラスは、同一パッケージの任意のクラスからも`private`なフィールドだけが見えない。

```java
package gomi;   // 同一パッケージ

public class App 
{
    public static void main( String[] args )
    {
        Parent1 p = new Parent1();
        //String a = p.a;  // これは見えない
        String b = p.b;
        String c = p.c;
        String d = p.d;
    }
}
```
----
別パッケージ`gomi2`に以下の`Parent2`クラスがあるとする。

```java
package gomi2;  // 二つ目のパッケージ

public class Parent2 {
    private String a = "a";     // ★privateなフィールド
    String b = "b";             // ★package-privateなフィールド
    protected String c = "c";   // protectedなフィールド
    public String d = "d";      // publicなフィールド

    public Parent2() {
    }
}
```

別パッケージ`gomi`の、`Parent2`クラスを継承する、`Child2`クラスからは`private`と`package-private`なフィールドが見えない。

```java
package gomi;   // 別パッケージ

public class Child2 extends gomi2.Parent2 {
    public void test() {
        //String a = this.a;    // ★これは見えない
        //String b = this.b;    // ★これは見えない
        String c = this.c;
        String d = this.d;
    }
}
```

別パッケージの`gomi2`の`Parent2`クラスは、任意のクラスから`public`なフィールド以外は見えない。

```java
package gomi;   // 別パッケージ

public class App 
{
    public static void main( String[] args )
    {
        Parent2 p = new Parent2();
        //String a = p.a;   // ★これは見えない
        //String b = p.b;   // ★これは見えない
        //String c = p.c;   // ★★これは見えない
        String d = p.d;
    }
}
```

### クラスフィールドの初期化
クラスフィールドの初期化は、直接値を設定する方法以外に以下の方法が使用できる。  

- `Static Initialization Blocks`  
    `static initailization block`は、`class`の`body`に複数個記述できる。  
    複数個記述されている場合は、先頭から順に呼び出されることになる。  

    ```java
    class A {
        static String [] aaaaa;
        static {
            // ここに初期化処理を記述する。
            aaaa = new String[] {"a", "b", "c", "d"};
        }
    }
    ```

- `private static method`  
    `private static method`を用意して、それを呼び出す方法。  

    ```java
    class A {
        public static varType myVar = initializeClassVariable();
        private static varType initializeClassVariable() {
            // ここに初期化処理を記述する。
        }
    }
    ```

### インスタンスフィールドの初期化
クラスフィールドの初期化は、直接値を設定する方法以外に以下の方法が使用できる。  

- `Initialization Blocks`  
    `Static Initialization Blocks`と似ているが、`static`をつけないブロックである。  
    ***`Initialization Blocks`は、それぞれのコンストラクタにコンパイラがコピーする***。  
    したがって、この方法はコンストラクタ内の共通処理を記述するのに使用することができる。  

    ```java
    class A {
        String aaaaa;
        {
            // ここに初期化処理を記述する。
            aaaa = "123456";
        }
    }
    ```

- `final method`  
    `final method`は、サブクラスでオーラ―ライドすることができない。  
    この方法は、サブクラスで初期化メソッドを再利用したい場合に便利である。

    ```java
    class A {
        private varType myVar = initializeInstanceVariable();
        protected final varType initializeInstanceVariable() {
            // ここに初期化処理を記述する。
        }
    }
    ```

### Inner Class
クラスの`body`の内部のクラスを定義する。  

- ***`inner class`からは、`Access level modifiers`に関わらず、その外側のクラスのフィールドやメソッドにアクセスできる***。  
- ***`inner class`を使うことで、間接的に外側のクラスの`private`なメンバーにアクセスすることができる。外側のクラスを継承するサブクラスは、この方法で`private`なメンバーにアクセスできる***
- ***`outer class`からは、`Access level modifiers`に関わらず、その内部のクラスのフィールドやメソッドにアクセスできる***。  
- `inner class`は、別のクラスから`Access level modifiers`に従って参照することができる。
- `inner class`は、staticなメソッドやフィールドを定義できない。



```java
public class Outer {
    private String x = "outer";

    class Inner {
        private String x = "inner";
        public String y = "test";
        private void test() {
            // 外側のクラスのxを参照する。
            // privateでも参照することができる。
            // この場合は、Outer.thisを使う
            System.out.println("Outer.this.x = " + Outer.this.x);
            // 内部のクラスのxを参照する。
            // この場合は、Inner.thisを使う
            System.out.println("Inner.this.x = " + Inner.this.x);
            // 外側のクラスのメソッドを呼び出す。
            // privateでも呼び出すことができる。
            Outer.this.outerMethod();
        }
    }

    public void test() {
        Inner inner = this.new Inner();
        // 内部クラスはprivateであっても参照できる。
        String XX = inner.x;
        // 内部クラスのprivateメソッドも呼び出すことができる
        inner.test();
    }
    
    private void outerMethod() {
        System.out.println("outerMethod!");
    }
}

public class App {
    public static void main() {
        Outer outer = new Outer();
        Outer.Inner inner = outer.new Inner();
        //String x = inner.x;   // ★privateは参照することができない。
        String y = inner.y;     // publicは参照することができる。
    }
}
```

なお、`staic`な`Inner class`の場合は、外側のクラスが作成されていなくても、以下の様に作成することができる。

```java
public class Outer {
    static class Inner {
        public final String test = "inner";
    }
}

public class Main {
    public static void main(String[] args) {
        Outer.inner x = new Outer.inner();
        System.out.println(x.test);
    }
}
```


### Local class ***ここは要チェック***
- `Local class`は`block`内で定義されるクラスである。  
- ***`final`か実質的に`final`(値が変更されない)なローカル変数やパラメータを`Local class`内部から参照することができる***。  
- ***また、`java8`以降では、この制限がなくなり、すべてのローカル変数やパラメータを`Local class`内部から参照することができるようになっている***。  
- ***`Local class`では`static`なメソッドを定義することはできない***。
- `Local class`は使用できるが、`Local interface`は`block`内部に定義することはできない。  
- ***`Local class`内部で定義できる`static`な定数は`primitive`型か`String`型だけである***。  
    コンパイル時に定数として評価できるものに限る。  
    `static final String [] m = {"a", "b", "c"};`のような`String[]`もダメ。

```java
package Furuichim.gomi;

public class LocalClassApp {
    // クラス変数
    public final String z = "z";
    public String y = "y";

    protected void test(String x) {
        String t = "t";

        // ローカルクラスの定義
        class LocalClass {
            
            // ローカルクラスでは、native型とString型以外の定数を持つことができない。---- (1)
            //static final String [] m = {"a", "b", "c"};
            //static java.util.Date date = new java.util.Date();

            // native型の定数は持つことができる。   ---- (2)
            static final int u = 1;

            // コンストラクタ
            public LocalClass() {
                // 引数を参照することができる。 ----(3)
                String a = x;

                // クラス変数を参照することができる。----(4)
                String b = z;

                // インスタンス変数を変更することもできる。----(5)
                y = "YYYY!";
                
                // ローカル変数は参照できる。--- (6)
                String d = t;

                // ローカル変数は変更できない。--- (7) ★重要
                //t = "TTTT";           // コンパイルエラー

                // メソッドの引数も変更できない。--- (8) ★重要
                // x = "Modified!;      // コンパイルエラー
            }

            // privateなメソッド
            private String getW() {
                return t;
            }

            // staticなメソッドは定義できない --- (9) ★重要
            //public static void test() {
            //}
        }

        // ローカルクラスのインスタンスを作成する。
        LocalClass local = new LocalClass();

        // ローカルクラスインスタンスのprivateなメソッドを呼び出すことができる。 --- (10)
        String w = local.getW();

        System.out.println(w);
    }
}
```

### Anonymous Classes
- `Local class`はクラスであるが、`Anonymous Classes`は`Expression`である。  
    ***このためコンストラクタを持つことはできない***。
- `Anonymous Classes`はその名の通り名前を持っていない。  
- クラスの宣言と初期化を同時に行うことができる。  
- `inteface`を実装するクラスの場合、`new`の後にその`interface`名を記述する。  
- `inteface`を実装するクラスの場合は、コンストラクタに引数を渡すことができない。  
- `class`を継承するクラスの場合は、`new`の後にその`class`名を記述する。  
- `class`を継承するクラスの場合は、コンストラクタに引数を渡すことができる。  
- `Anonymous Classes`は`Local class`と同様に、
    - 外側のメソッドの引数やローカル変数を参照することができる。
    - `static`なメソッドを定義できない。
    - `static`な定数は`primitive`型か`String`型だけである。  

```java
public class AnonymousClassApp {
    // インスタンス変数
    public int x = 0;

    // Anonymous Classが実装するインタフェースを定義する。 ---(1)
    public interface Hello {
        public void say();
    }

    // Anonymous Classが継承するスーパークラスを定義する。 ---(2)
    public class Shape {
        public int x;
        public int y;
        public Shape() {
            x = 0;
            y = 0;
        }
        public int getRegion() {
            return 0;
        }
    }
    
    
    // テスト用のメソッド
    protected void test(String b) {
        String a = "a";

        //--------------------------------------
        // Interfaceを実装するAnonymous Class
        //--------------------------------------
        // Anonymous Classを使ってインスタンスを作成する。
        Hello hello = new Hello() {
            public void say() {
                // ローカル変数を参照することができる。 ---(3)
                String aa = a;
                
                // ローカル変数を変更することはできない。---(4) ★重要
                //a = "A!";     // コンパイルエラー

                // 引数を参照することができる。---(5)
                String bb = b;
                
                // インスタンス変数は変更することができる。---(6)
                x = 100;

                // メソッドの引数は変更する事ができない。---(7) ★重要
                //b = "abcd";   // コンパイルエラー
            }
        };

        // Anonymous Classのインスタンスのメソッドを呼び出す。
        hello.say();

        //--------------------------------------
        // Classを継承するAnonymous Class
        //--------------------------------------
        // Anonymous Classを使ってインスタンスを作成する。
        Shape shape = new Shape() {
            @Override
            public int getRegion() {
                return 100;
            }
        };

        // Anonymous Classのインスタンスのメソッドを呼び出す。
        int region = shape.getRegion();
    }
    
}

```

### Lambda expressions
- `Anonymouse Classes`と似ているが、メソッドを一つしか持たないより簡潔な機能。  
- メソッドを1つしか持たなインタフェースは、`functional interface`と呼ばれる。  
    + １つの抽象メソッド以外に、`static`、`default`、`private`、`private static`メソッドを持つ事ができる。
    + `java.lang.Object`のメソッドは、抽象メソッドとして定義することができる。
    + `functional interface`には`@FunctionalInterface`アノテーションをつけることができる。
- `functional interface`をimplementsするクラスはコンパイルエラーになる。
    `functional interface`は`Lambda expressions`か匿名クラスでのみ実装することができる。
- `functional interface`を引数にとるメソッドには、`Lambda expressions`を使用することができる。  
- `java.util.function.*`で便利な`functional interface`が定義されている。
    - `Predicate<T>` - boolean test(T)
    - `BiPredicate<T,U>` - boolean test(T, U)
    - `Consumer<T>` - void accept(T)
    - `Supplier<T>` - T get()
    - `Function<T,R>` - R apply(T) 
- 1つ以上のパラメータを持つことができる。
- パラメータが１つの場合は、`()`を省略することができる。
- `Lambda expressions`の本体が1つの`expression`の場合は、`{}`も`return`も省略することができる。  
  この場合は、コンパイラが`return`を付与する。
- ***`Lambda expressions`では`this`で呼び出し元の`this`にアクセスできる***。  
- ***`Lambda expressions`を定義する関数スコープ内のローカル変数と同じ名前の引数を使うことはできない***。
- ***`Lambda expressions`を定義する関数スコープ内のローカル変数を参照できるが、変更することはできない***。
- ***`Lambda expressions`を定義する関数スコープ内のローカル変数を参照する場合、その外部で当該ローカル変数を変更することもできない***。

```java
public class LambdaApp {

    public int x = 100;
    
    // メソッドが一つだけのinterface = functional interface ---(1)
    @FunctionalInterface
    interface Hello {
        String say(String name);
    }

    // Lambda関数を呼び出すテスト関数。
    public void doTest() {
        String y = "Test";
        String [] names = {"Taro", "Hanako", "Mike", "Jane"};

        // Lambda関数を引数にする。---- (2)
        this.test(names, (String name) -> {
            x = 101;                // インスタンス変数は変更できる。 ---- (3)
            //y = y.toUpperCase();  // ローカル変数は変更できない。   ---- (4)
            String z = y;           // ローカル変数は参照できる。     ---- (5)
            return "Hello " + name; 
        });

        // ローカル変数と同じ名前の引数使えない。---- (7)
        //this.test(names, (String y) -> {
        //  return y; 
        //});

        //y = y.toUpperCase();      // Lambda関数内部でローカル変数を参照している場合は、
                                    // 外部でローカル変数を変更することもNG     ----(6)
                                    // これを行うと(5)でコンパイルエラーになる。
    }

    public void doTest2(int x) {
        String [] names = {"Taro", "Hanako", "Mike", "Jane"};

        // Lambda関数を引数にする。----(7)
        this.test(names, (String name) -> {
            // メソッドの引数は変更できない。---(8)
            //x += 3;   //コンパイルエラー
            return "x = " + x +", this.x = " + this.x + name; 
        });
    }

    // functional interfaceを引数に指定することで、Lambda関数が指定できるようになる。 ---(9)
    public void test(String [] names, Hello h) {
        for (String n : names) {
            System.out.println(h.say(n));
        }
    }
}
```

### `Local Class` `Anonymous Class` `Lambda expression`の比較

|特徴|Local Class|Anonymous Class|Lambda expression|
|----|----|----|----|
|ブロック外からの利用|できない|できない|できない|
|コンストラクタ|定義 ***できる***|定義 ***できない***|ない|
|staticなメソッド|定義できない|定義できない|ない|
|定数|primitiveかString型のみ可能|primitiveかString型のみ可能|ない|
|外部メソッドの引数の参照|できる|できる|できる|
|外部メソッドの引数の変更|***できない***|***できない***|***できない***|
|外側のローカル変数の参照|できる|できる|できる|
|外側のローカル変数の変更|***できない***|***できない***|***できない***|
|外側のインスタンス変数の変更|できる|できる|できる|

### aggregate operations  
`Collection`型のオブジェクトを`java.util.stream`に変換して、以下のようなメソッドをチェーンして行うことができる。  
各メソッドでは、`Lambda expressions`を引数にとる。  
- `filter`
- `map`
- `reduce`
- `forEach`

```java
roster
    .stream()
    .filter(
        p -> p.getGender() == Person.Sex.MALE
            && p.getAge() >= 18
            && p.getAge() <= 25)
    .map(p -> p.getEmailAddress())
    .forEach(email -> System.out.println(email));
```

### Method References  
`C`や`C++`言語と同様に、メソッドの参照を使用することができる。  

- `static method`の参照  
    `ClassName::MethodName`で指定する。  
    メソッド参照は、`functional interface`として使用できる。  

    ```java
    import java.util.Arrays;

    public class MethodReferenceApp {
        class TestMethods {
            public static int Compare1(String a, String b) {
                return a.compareTo(b);
            }
        }
        
        public void test1() {
            String[] names = {"a", "b", "c", "d", "e"};

            // static methodの参照を指定する。 --- (1)
            Arrays.sort(names, TestMethods::Compare1);
        }
    }
    ```

- `instance method`の参照  
    `instance::MethodName`で指定する。  

    ```java
    import java.util.Arrays;

    public class TestMethodsImpl {
        public static int Compare1(String a, String b) {
            return a.compareTo(b);
        }

        public int Compare2(String a, String b) {
            return a.compareTo(b);
        }
    }

    public class MethodReferenceApp {
        public void createObject(Supplier<Date> supplier)
        {
            Date d = supplier.get();
            System.out.println(d);
        }
        
        public void test1() {

            String[] names = {"a", "b", "c", "d", "e"};

            // static methodの参照を指定する。 --- (1)
            Arrays.sort(names, TestMethodsImpl::Compare1);

            // インスタンス methodの参照を指定する。 --- (2)
            TestMethodsImpl t = new TestMethodsImpl();
            Arrays.sort(names, t::Compare2);

            // コンストラクタの参照を指定する。---(3)
            createObject(Date::new);
        }
    }
    ```

- `Constructor`の参照  
    `ClassName::new`で指定する。  
    `ClassName::new`は`Supplier<T>`または`Function<?,T>`として扱うことができる。

    ```java
    public class MethodReferenceApp {
        public void createObject(Supplier<Date> supplier)
        {
            Date d = supplier.get();
            System.out.println(d);
        }
        
        public void test1() {
            // コンストラクタの参照を指定する。---(1)
            createObject(Date::new);

            // Function<Set<String>, List<String>>型のコンストラクタ
            Function<Set<String>, List<String>> creator = ArrayList::new;
            List<String> obj = creator.apply(new Set.of("a", "b"));
        }
    }
    ```

- オーバーロードの制限1  
    クラスメソッドでオーバーロードされたメソッドがある場合、FunctionalInterfaceの型に合わせて自動的にメソッドが選択される。

    ```java
    static public void main(String[] args) {
        // Function<String, String>の型に合うString::newが選択される。
        String v1 = test1(String::new);

        // Supplier<String>の型に合うString::newが選択される。
        String v2 = test2(String::new);
    }

    public String test1(Function<String, String> method) {
        return method.apply("123456");
    }

    public String test2(Supplier<String> method) {
        return method.get();
    }
    ```


- オーバーロードの制限2  
    クラスメソッドとインスタンスメソッドで同じ名前のメソッドが提供されている場合、メソッド参照を使うとコンパイルエラーになる。
    例えば、java.lang.Integerは以下のメソッドを提供している。

    ```java
    // インスタンスメソッド
    // このIntegerのハッシュ・コードを返します。
    int hashCode();

    // スタティックメソッド
    // Integer.hashCode()との互換性がある、int値のハッシュ・コードを返します。
    static int hashCode(int value)
    ```

    この場合以下はコンパイルエラーになる。
    どちらの`hashCode`メソッドか判断がつかないためコンパイルエラー

    ```java
    int hash = Integer::hashCode;
    ```



### Enum Types
Java言語の`Enum`は他の言語の`Enum`よりも、非常に強力です。  
`Enum`は`java.lang.Enum`を継承したクラスで、デフォルトで各種メソッドを持ち、独自のメソッドを追加することも可能です。  

```java
package Furuichim.gomi;

public class EnumTest {
    // 独自のメソッドを持つEnum型
    public enum Day {
        SUNDAY("日曜日", "Sun"),
        MONDAY("月曜日", "Mon"),
        TUESDAY("火曜日", "Tue"),
        WEDNESDAY("水曜日", "Wed"),
        THURSDAY("木曜日", "Thu"),
        FRIDAY("金曜日", "Fri"),
        SATURDAY("土曜日", "Sat");

        private String jpName;
        private String simpleName;

        Day(String jpName, String simpleName) {
            this.jpName = jpName;
            this.simpleName = simpleName;
        }

        public String getJpName() {
            return jpName;
        }

        public void setJpName(String jpName) {
            this.jpName = jpName;
        }

        public String getSimpleName() {
            return simpleName;
        }

        public void setSimpleName(String simpleName) {
            this.simpleName = simpleName;
        }
    };
    
    public static void testEnum() {
        for (Day d : Day.values()) {
            System.out.printf("Name = %s, jpName = %s, simpleName = %s\n", d.name(), d.getJpName(), d.getSimpleName());
        }
    }
}
```

- `Enum Types`では、コンストラクタは`private`しか指定できない。
- `Enum Types`は、暗黙的に`java.lang.Enum`クラスを継承するため、他のクラスを継承することはできない。
- しかし、`Enum Types`は、インタフェースを実装することはできる。
- `Enum Types`は、暗黙的に`final`が付加される。明示的に`final`をつけることはできない。
- `Enum Types`は、abstractメソッドを持つことができるが、各定数でabstractメソッドを定義する必要がある。
    ```java
    Enum Test {
        A {
            String test() { return "I'm A."; }
        },
        B {
            String test() { return "I'm B."; }
        },
        C {
            String test() { return "I'm C."; }
        };

        abstract String test();
    }
    ```
- `Enum Types`は`Comparable`インタフェースを実装しており、各定数は列記した順で管理される。
- `Enum Types`はクラス内で定義する事はできるが、メソッド内では定義できない。


`Enum Types`の重要なメソッド
- `name()` - 定数の名前(文字列)を取得する
- `toString()` - 定数の名前(文字列)を取得する
- `valueOf(文字列)` - 名前(文字列)を定数に変換する。
- `values()` - 定数の一覧を取得する。
- `ordinal()` - 定数の先頭からの位置を取得する。先頭は０である。


## Annotations  
### アノテーションとは
アノテーションはプログラムに与えられるメタデータです。  

- コンパイラに対する情報 - エラーを検出したり、警告を抑制するために、コンパイラにより使われる。  
- コンパイル時とデプロイ時の処理 - XMLなどのコードを生成するために、ソフトウェアツールはアノテーションの情報を処理する。  
- 実行時の処理 - 実行時に検査することができるアノテーションもある。

### 指定方法
`@`を先頭につける。  
複数のアノテーションをつけることもできる。  
また、同じアノテーションを複数個つけることもできる。  

```java
@Entiry

@Override
void mySuperMethod() {}

@Author(
   name = "Benjamin Franklin",
   date = "3/27/2003"
)
class MyClass() {}

@Author(name = "Jane Doe")
@Author(name = "John Smith")
class MyClass2() {}
```

### 独自のアノテーションの定義方法
`interface`の定義に似た、`@interface`で宣言する。  
メンバは、インタフェースでの抽象メソッドと同じで、暗黙的に`public abstract`になります。  
デフォルト値がある場合は、`default`を使って指定することができる。  
デフォルト値は、空文字は許可されるが、nullやnewを使った初期化はコンパイルエラーになる。  
デフォルト値の指定がある属性は、アノテーションの指定時に省略することができる。  
※逆にデフォルト値が無い場合は、必ず指定する必要がある。

`@interface`をつけたアノテーション型は、`java.lang,annotation.Annotation`インタフェースを暗黙的に実装したインタフェースとなります。
定義したアノテーションを`java doc`内に記載する場合は、`@Documented`でアノテートする。  

```java
import java.lang.annotation.*;
public class AnnotaionTest {

    // 独自のアノテーションを定義
    @Documented
    @interface ClassPreamble {
        String author();
        String date();
        int currentRevision() default 1;
        String lastModified() default "N/A";
        String lastModifiedBy() default "N/A";
        // Note use of array
        String[] reviewers();
    };

    @ClassPreamble(
        author = "Masahiro Furuichi",
        date = "2020/01/20",
        currentRevision = 5,
        lastModified = "2020/02/01",
        lastModifiedBy = "Taro Yamada",
        reviewers = {"Umeko", "Takako", "Kumiko"})
    public class Test {
    }
}
```

### Java言語によって使われる定義済みアノテーション

- `@Deprecated`  
    要素が廃止されており、使うべきでないことをマークする。  
    この要素が疲れると、コンパイラはワーニングを出す。  
    java docでは`@deprecated`を、コードには`@Deprecated`を使用する。  

    ```java
    /**
    * @deprecated
    * explanation of why it was deprecated
    */
    @Deprecated
    static void deprecatedMethod() { }
    ```

- `@Override`  
    メソッドがオーバーライドされていることをコンパイラに知らせる。  

    ```java
    // mark method as a superclass method
    // that has been overridden
    @Override 
    int overriddenMethod() { }
    ```

- `@SuppressWarnings`  
    コンパイラに一部の警告を抑止するように指示する。  
    Java言語には、`deprecation`と`unchecked`の警告のカテゴリーがある。  
    `unchecked`は`generics`の仕様が登場する以前のレガシーなコードとやり取りする時に発生する警告である。

    ``` java
    // use a deprecated method and tell 
    // compiler not to generate a warning
    @SuppressWarnings({"deprecation", "unchecked"})
    void useDeprecatedMethod() {
        // deprecation warning
        // - suppressed
        objectOne.deprecatedMethod();
    }
    ```

    以下のいずれの記述方法でもOKです。

    ```java
    @SuppressWarnings(values={"unchecked"})
    @SuppressWarnings(values={"deprecation", "unchecked"})
    @SuppressWarnings("unchecked")
    @SuppressWarnings({"unchecked"})
    @SuppressWarnings({"deprecation", "unchecked"})
    ```

- `@SafeVarargs`  
    メソッドやコンストラクタの本体が自身の可変パラメータに対して安全でない可能性のある操作を実行しないことを示す、プログラマアサーションです。メソッドまたはコンストラクタにこの注釈を適用すると、型情報保持可能でない可変引数 (vararg) 型に関する未検査警告が抑制されるほか、パラメータ化された配列のコールサイトでの作成に関する未検査警告も抑制されます。
    **`@SafeVarargs`をつけることができるメソッドは、`final`、`static`、`private`のいずれかです。**

    ```java
    //@SafeVarargs
    //public void test1(java.util.List<String>... list) {}  // コンパイルエラー

    @SafeVarargs
    private void test2(java.util.List<String>... list) {}

    @SafeVarargs
    public static void test3(java.util.List<String>... list) {}

    @SafeVarargs
    public final void test4(java.util.List<String>... list) {}
    ```

- `@FunctionalInterface`  
    インタフェース型の宣言を、Java言語仕様に定義されている関数型インタフェースとすることを目的としていることを示すために使われる情報目的の注釈型です。

### 他のアノテーションに適用されるアノテーション
`meta-annotations`と呼ばれる。  

- `@Retention`  
    注釈付きの型を持つ注釈を保持する期間を示します。  
    注釈型宣言にRetention注釈がない場合、デフォルトの保持ポリシーは`RetentionPolicy.CLASS`になります。

    - `RetentionPolicy.SOURCE` - ソースレベルで保持されて、コンパイラに無視される。  
    - `RetentionPolicy.CLASS` - コンパイル時にコンパイラによって保持されて、実行時にはJVMによって無視される。  
    - `RetentionPolicy.RUNTIME` - JVMによって保持されて、実行環境で使用される。

- `@Documented`  
    javadoc および同様のツールによってデフォルトでドキュメント化されることを示します。  
    この型は、クライアントによる注釈付き要素の使用に影響する注釈を持つ型の宣言に注釈を付けるために使用します。  

- `@Target`  
    Javaのどの要素に対してアノテーションを適用できるかを指定する。  

    - `ElementType.ANNOTATION_TYPE` - アノテーションに対して適用できる。  
    - `ElementType.CONSTRUCTOR` - コンストラクタに対してい適用できる。  
    - `ElementType.FIELD` - フィールドに対して適用できる。  
    - `ElementType.LOCAL_VARIABLE` - ローカル変数に対して適用できる。  
    - `ElementType.METHOD` - メソッドに対して適用できる。  
    - `ElementType.PACKAGE` - パッケージ宣言に対して適用できる。  
    - `ElementType.PARAMETER` - メソッドのパラメータに対して適用できる。  
    - `ElementType.TYPE` - クラス、インタフェース及びenum宣言に対して適用できる。  

- `@Inherited`  
    アノテーション型がスーパークラスから継承されていることを示す。  
    `@Inherited`が指定されたアノテーションは、クラスに対するアノテーションのみ、サブクラスに継承されます。  
    ユーザがアノテーション型をクエリーする時、クラスがこのアノテーションを持っていない場合、スーパークラスのアノテーション型が検索される。  

- `@Repeatable`  
    このアノテーションでマークされたアノテーションは、複数回指定することができる。  

    ```java
    @Alert(role="Manager")
    @Alert(role="Administrator")
    public class UnauthorizedAccessException extends SecurityException { ... }
    ```

    リピート可能なアノテーションの定義は以下のように、`@Repeatable`を指定します。  
    さらに、当該アノテーションの配列型の`value()`要素を持つ必要があります。  

    ```java
    import java.lang.annotation.Repeatable;

    @Repeatable(Schedules.class)
    public @interface Schedule {
        String dayOfMonth() default "first";
        String dayOfWeek() default "Mon";
        int hour() default 12;
    }

    public @interface Schedules {
        Schedule[] value();
    }
    ```

    `@Repeatable`でリピート可能がマークされていないアノテーションを複数回指定すると、コンパイルエラーが発生します。  

### Typeアノテーション  
Java SE8から、すべての型の使用に対して適用できるようになった。  
これは、型を使うあらゆる場所で、アノテーションが使用できることを意味する。  
具体的には、`new`、`cast`、`implements clauses`、`throws clauses`でも使用できる。  
Typeアノテーションは、強い型チェックを保障するJavaプログラムの改善して分析をサポートするために作られた。  
Java SE8は型チェックフレームワークは提供していないが、Javaコンパイラと一緒に使うためのプラガブルなモジュールを実装するフレームワークを記述したりダウンロードできるようにしている。  
以下のようにマークして、NULLチェックを行うプラグインを指定してコンパイルすることで、コンパイラが潜在的な問題を検出できるようにしている。

```java
import javax.annotation.Nonnull;
@NonNull String str;
```

## Interface  

### インタフェースの継承
インタフェースは、複数のインタフェースを継承することができる。

```java
public interface GroupedInterface extends Interface1, Interface2, Interface3 {
    // constant declarations
    
    // base of natural logarithms
    double E = 2.718282;
 
    // method signatures
    void doSomething (int i, double x);
    int doSomethingElse(String s);
}
```

### インタフェースのボディ  
インタフェースのボディは、以下の要素を含めることができる。  
- ***`abstract methods` - 抽象メソッド***  
- ***`default methods` - `default`を頭につけたデフォルトメソッド***  
- ***`static method` - `static`を頭につけたスタティックメソッド***  
- ***`private method` - `private`を頭につけたメソッド***  
- ***`private static method` - `private static`を頭につけたメソッド***  
- ***定数 - 暗黙的に`public static final`をして扱われる***  
  このため初期値が無いとコンパイルエラーになる。

インタフェースの各メソッドは暗黙的に`public abstract`であり、`public abstract`を省略することができる。  
***逆に、`public`以外を指定すると、コンパイルエラーになる***。



### インタフェースの実装  
慣習的に、`extends`を先に記述して、そのあとに`implements`を記述する。  
複数のインタフェースを実装する場合は、カンマ`,`で区切って定義する。  

```java
public class Worker extends Human implements Programer, Tester, Leader {
}
```

### インタフェースの変更  
インタフェースを変更すると、依存のインタフェースを実装している既存のクラスが動作しなくなる。  
これを避けるために、既存のインタフェースを継承した新しいインタフェースを実装する方法か、`default methods`や`static methods`を既存インタフェースに追加する方法のいずれかをとることが推奨される。  

### `static method`  
**`static method`は、publicかprivateにする必要がある、protectedはコンパイルエラーになる。**
`static method`は、以下の形式で呼び出す。
**インスタンスでは呼び出すことができない。**

```java
インタフェース名.staticメソッド名
```

### `Default methods`  
`Default methods`を使うことで、既存のインタフェースを実装しているクラスに影響を与えることなく、既存のインタフェースにメソッドを追加することができる。  
`Default methods`は`default`を頭につけたメソッドで、暗黙的に`public`とみなされる。  
**public以外を指定するとコンパイルエラーになる。**
`java.lang.Object`クラスの`equals()`、`hashCode()`、`toString()`を`default methods`として定義するとコンパイルエラーになる。


```java
import java.time.*;

public interface TimeClient {
    void setTime(int hour, int minute, int second);
    void setDate(int day, int month, int year);
    void setDateAndTime(int day, int month, int year,
                               int hour, int minute, int second);
    LocalDateTime getLocalDateTime();
    
    static ZoneId getZoneId (String zoneString) {
        try {
            return ZoneId.of(zoneString);
        } catch (DateTimeException e) {
            System.err.println("Invalid time zone: " + zoneString +
                "; using default time zone instead.");
            return ZoneId.systemDefault();
        }
    }
        
    default ZonedDateTime getZonedDateTime(String zoneString) {
        return ZonedDateTime.of(getLocalDateTime(), getZoneId(zoneString));
    }
}
```

### `Default methods`を持つインタフェースの継承  
`Default methods`を持つインタフェースを継承する場合、以下の3つのオプションがあります。
- `Default methods`に一切触れない場合、`Default methods`も含めてインタフェースを継承することになる。  
- `Default methods`を再宣言する場合、それは抽象メソッドになる。
- `Default methods`を再定義する場合、それをオーバーライドすることになる。


```java
// default methodを実装するTestInterface
public interface TestInterface {
    String getName();
    int getAge();
    String getPhoneNumber();

    // default method
    default public String getDescription() {
        return "Test Comment";
    }
}

// TestInterfaceを継承したSubTestInterfaceの定義
public interface SubTestInterface extends TestInterface {
    // Default methodをオーバーライドする
    @Override
    default public String getDescription() {
        // 継承元Interfaceの実装するdefault methodを呼び出す。
        String superComment = TestInterface.super.getDescription();
        return superComment.toUpperCase();
    }
}
```

## 継承によるメソッドのHiding
### オーバーライドによるクラスメソッドとインスタンスメソッドの`Hiding`
サブクラスで`instance method`と`static method`をオーバーライドした場合の違いを以下に示す。  

```java
public class Animal {
    // (1)
    public static void testClassMethod() {
        System.out.println("The static method in Animal");
    }
    // (2)
    public void testInstanceMethod() {
        System.out.println("The instance method in Animal");
    }
}

public class Cat extends Animal {
    // (3)
    public static void testClassMethod() {
        System.out.println("The static method in Cat");
    }
    // (4)
    public void testInstanceMethod() {
        System.out.println("The instance method in Cat");
    }

    public static void main(String[] args) {
        Cat myCat = new Cat();
        Animal myAnimal = myCat;

        // インスタンスメソッドの呼び出し。
        myCat.testInstanceMethod();     // (4)が呼び出される
        myAnimal.testInstanceMethod();  // (4)が呼び出される

        // クラスメソッド（staticメソッド）の呼び出し。
        Cat.testClassMethod();          // (3)が呼び出される
        Animal.testClassMethod();       // (1)が呼び出される
    }
}
```

### 同じメソッドシグネチャを持つ`instance method`と`Default method`がある場合
2つのルールに基づき、優先度が決定される。  

- ***`instance method`が、`Default method`よりも優先される***。  

    ```java
    public class Horse {
        // (1)オーバーライドしたメソッド
        public String identifyMyself() {
            return "I am a horse.";
        }
    }
    public interface Flyer {
        // (2)default method1
        default public String identifyMyself() {
            return "I am able to fly.";
        }
    }
    public interface Mythical {
        // (3)default method2
        default public String identifyMyself() {
            return "I am a mythical creature.";
        }
    }
    public class Pegasus extends Horse implements Flyer, Mythical {
        public static void main(String... args) {
            Pegasus myApp = new Pegasus();
            System.out.println(myApp.identifyMyself()); // (1)が呼び出される。
        }
    }
    ```

- 他の候補によりすでにオーバーライドされているメソッドは無視される。  
※と書かれているが、意味が分からない。  
※普通にオーバーライドされたメソッドが優先されると思ってよい。  

    ```java
    public interface Animal {
        // (1)元のdefault method
        default public String identifyMyself() {
            return "I am an animal.";
        }
    }
    public interface EggLayer extends Animal {
        // (2)オーバーライドされたdefault method
        default public String identifyMyself() {
            return "I am able to lay eggs.";
        }
    }
    public interface FireBreather extends Animal { }
    public class Dragon implements EggLayer, FireBreather {
        public static void main (String... args) {
            Dragon myApp = new Dragon();
            System.out.println(myApp.identifyMyself()); // (2)が呼び出される。
        }
    }
    ```


- 同じシグネチャの`default method`を持つ複数のインタフェースを実装するクラスで、解決できない場合は、コンパイルエラーとなる。  
    どちらのdefaultメソッドか分からないためコンパイルエラーになる。

    ```java
    public interface OperateCar {
        // ...
        default public int startEngine(EncryptedKey key) {
            // Implementation
        }
    }

    public interface FlyCar {
        // ...
        default public int startEngine(EncryptedKey key) {
            // Implementation
        }
    }

    public class FlyingCar implements OperateCar, FlyCar {  // ★コンパイルエラー
    }
    ```

- 同じシグネチャの`default method`を持つ複数のインタフェースを実装するクラスで、`default method`を再定義することで、コンパイルエラーにならなくなる。
    それぞれのインタフェースの`default method`は、 ***`interfaceName.super.defaultMethod`で呼び出すことができる***。  

    ```java
    public interface OperateCar {
        // ...
        default public int startEngine(EncryptedKey key) {
            // Implementation
        }
    }

    public interface FlyCar {
        // ...
        default public int startEngine(EncryptedKey key) {
            // Implementation
        }
    }

    public class FlyingCar implements OperateCar, FlyCar {
        // startEngineを再定義する。
        public int startEngine(EncryptedKey key) {          // ★オーバーライドすることでコンパイル可
            // FlyCarのdefault methodを呼び出す。
            FlyCar.super.startEngine(key);
            // OperateCarのdefault methodを呼び出す。
            OperateCar.super.startEngine(key);
        }
    }
    ```

- 継承するスーパークラスの`instance method`と継承するインタフェースのメソッドのシグネチャが同じ場合、スーパークラスの`instance method`が、インターフェースのメソッドをオーバーライドすることになる。  

    ```java
    public interface Mammal {
        String identifyMyself();
    }
    public class Horse {
        // (1)
        public String identifyMyself() {
            return "I am a horse.";
        }
    }
    public class Mustang extends Horse implements Mammal {
        public static void main(String... args) {
            Mustang myApp = new Mustang();
            System.out.println(myApp.identifyMyself()); // (1)が呼び出される。
        }
    }
    ```

インタフェースの`static method`は、オーバーライドされることはない。  

### 継承によるフィールドの隠ぺい
サブクラスでスーパークラスと同じ名前のフィールドを持つ場合、フィールドの型に関わらず、スーパークラスのフィールドは隠ぺいされる。サブクラスでスーパークラスの隠ぺいされたフィールドを参照するには、`super`キーワードを使う。  
一般的に、フィールドの隠ぺいは推奨されない。  

```java
public class SuperClass {
    public String name;
}
public class SubClass extends SuperClass {
    // スーパークラスのメソッドと同じ名前のフィールドを定義できる。
    // フィールドの型は異なっていてもよい。
    // ※いずれも推奨されないが。
    public int name;
    public void test() {
        // superキーワードでスーパークラスのフィールドを参照することができる。
        System.out.println("name in SuperClass = ", super.name);
        System.out.println("name in SubClass = ", this.name);
    }
}
```

### 継承によるメソッドのオーバーライド
サブクラスでスーパークラスと同じシグネチャのメソッドを持つ場合、スーパークラスのメソッドはオーバーライドされる。  
サブクラスでスーパークラスのメソッドを呼び出すには、`super`キーワードを使う。  
※同じシグネチャで返却値の型が違うメソッドを定義することはできない。  

```java
public class Superclass {
    public void printMethod() {
        System.out.println("Printed in Superclass.");
    }
}
public class Subclass extends Superclass {
    // overrides printMethod in Superclass
    public void printMethod() {
        // superキーワードでスーパークラスのメソッドを呼び出すことができる。
        super.printMethod();
        System.out.println("Printed in Subclass");
    }
    public static void main(String[] args) {
        Subclass s = new Subclass();
        s.printMethod();    
    }
}
```

### 継承時のコンストラクタ
サブクラスのコンストラクタでは、`super`キーワードでスーパークラスのコンストラクタを呼び出すことができる。  
なお、スーパークラスのコンストラクタは、必ずコンストラクタの先頭で行う必要がある。  

```java
public MountainBike(int startHeight, 
                    int startCadence,
                    int startSpeed,
                    int startGear) {
    // 必ず先頭で呼び出す必要がある。
    super(startCadence, startSpeed, startGear);
    seatHeight = startHeight;
}
```

サブクラスで明示的にスーパークラスのコンストラクタを呼び出さない場合、javaコンパイラはスーパークラスの引数無しのコンストラクタを先頭位置に挿入する。この場合、スーパークラスが引数無しのコンストラクタを持っていないと、コンパイルエラーになる。  

サブクラスのコンストラクタで、`this`による自クラスのコンストラクタ呼び出しと、`super`によるスーパーカスのコンストラクタ呼び出し両方呼び出してはいけない。`this`によって呼び出されたコンストラクタ内で、明示的または暗黙的に`super`が呼び出されるため、`super`を二回呼ぶことになるためである。これを行うと、コンパイルエラーになる。

### `Object`クラス
`Object`クラスは、すべてのクラスのトップに位置するクラスである。  
以下のメソッドを必要に応じてオーバーライドする必要がある。  

- `protected Object clone() throws CloneNotSupportedException`  
`Object`クラスのこのメソッドは、サブクラスの`Cloneable`インタフェースを実装しているかチェックして、実装してない場合は`CloneNotSupportedException`例外をスローする。  
クラスが`clone`(コピー)をサポートする場合、`Cloneable`インタフェースを実装する必要がある。  
`Object`クラスの`clone`メソッドの実装は、外部参照を持たないクラスではそのまま使用できるが、
スーパークラスが外部参照を持つ場合は独自の`clone`メソッドを実装する必要がある。  

- `public boolean equals(Object obj)`  
2つのオブジェクトを比較して、その中身が同じ場合に`true`を返却する。  
2つの参照が同じことを確認する場合は、`==`オペレータを使う。  

- `protected void finalize() throws Throwable`  
オブジェクトが`garbage`になったときに、呼び出されるメソッドである。  
`Object`クラスの`finalize`メソッドの実装では、何も行われない。  
`finalize`メソッドはシステムによって自動的に呼び出されるが、いつ呼び出されるかは不確定である。したがって、リソースを解放するために、`finalize`メソッドに頼ることは避けるべきである。
例えば、ファイルハンドルを`finalize`でクローズすることは避けるべきである。  

- `public final Class getClass()`  
`getClass()`メソッドは`Class`オブジェクトを返却するメソッドである。  
このメソッドはオーバーライドできない。  

- `public int hashCode()`  
オブジェクトのハッシュ値を返却するメソッドである。  
2つのオブジェクトの中身が同じ場合、それぞれ同じハッシュ値を持つ必要がある。  
`equals()`メソッドをオーバーライドする場合は、オブジェクトを比較するために、`hashCode()`メソッドもオーバーライドする必要がある。  

- `public String toString()`  
オブジェクトの中身を表す文字列を返却する。  
クラスを実装する場合は、いつも`toString()`メソッドをオーバーライドすることを検討するべきであル。

### `final`クラスとメソッド  
クラスに`final`をつけると、そのクラスを継承できなくなる。  
メソッドに`final`をつけると、サブクラスでオーバーライドできなくなる。  

コンストラクタから呼び出されるメソッドは、一般的には`final`であるべきだ。なぜならば、サブクラスでそれらのメソッドを再定義すると、望まない結果になるからである。  

### `abstract`クラスとメソッド
- `abstract`クラスとは`abstract`キーワードをつけたクラスで、インスタンス化することができないが、継承されることはできる。  
- `abstract`メソッドは`abstract`キーワードをつけたメソッドで、実装を持たない。  
    もし、クラスが`abstract`メソッドを持つ場合、そのクラス自体を`abstract`にする必要がある。  
- `abstract`キーワードを省略しても、実装を持たないメソッドは`abstract`である。
- `abstract`クラスとは、`abstract`メソッドと実装を持つメソッドから構成される。


```java
public abstract class GraphicObject {
   // declare fields
   // declare nonabstract methods
   abstract void draw();
}
```

- `abstract`クラスを継承するサブクラスでは、通常はすべての`abstract`メソッドの実装を行う。  
    しかし、一部しか実装を行わない場合は、そのサブクラスも`abstract`クラスにするべきである。  

- インタフェースの`default`でも`static`でもないメソッドは、暗黙的に`abstract`である。  
    だから`abstract`キーワードはインタフェースのメソッドで指定する必要がない。※指定してもかまわない。  

- `abstract`クラスがインタフェースを実装する場合、すべてのインタフェースのメソッドを実装しなくてもよい。  
    未実装のメソッドは、`abstract`クラスを継承するサブクラスで実装することになる。  


### `abstract`クラスとインタフェースの比較

||`abstract`クラス|インタフェース|
|---|---|---|
|インスタンス化|できない|できない|
|コンストラクタ|持てる|持てない|
|static finalなフィールドの定義|できる|できる|
|static finalでないフィールドの定義|できる|***できない***|
|publicメソッドの実装|できる|できる(`default`メソッド)|
|protectedメソッドの実装|できる|***できない***|
|privateメソッドの実装|できる|できる|

- `abstract`クラスを使うべき状況  
    - いくつかの関連する近いクラス間でコードを共有したい場合  
    - `abstract`クラスを継承するクラスが、多くの共有のフィールドやメソッドを持つことが予測される場合  
    - `static final`でないフィールドを定義する必要がある場合  
- インタフェースを使うべき状況  
    - 関連のないクラスがあなたのインタフェースを実装することが予測される場合。  
      `Comparable`や`Cloneable`のようなインタフェースは、関連のない多くのクラスに実装されている。  
    - 型の多重継承を利用したい場合。


## `Number`クラス
`Number`クラスを継承するクラスには以下がある。  

- `Byte`
- `Integer`
- `Short`
- `Long`
- `Float`
- `Double`  

### `box`と`unbox`
これらのクラスは、`primitive`型を`wrap`している。  
コンパイラは、`primitive`型をから`Number`を継承する型に自動的に変換(`box`)したり、その逆変換(`unbox`)を行う。

### `Number`クラスのメソッド
- `Number`オブジェクトから`primitive`型に変換するメソッド  
    - `byte byteValue()`
    - `short shortValue()`
    - `int intValue()`
    - `long longValue()`
    - `float floatValue()`
    - `double doubleValue()`

- 他の`Number`オブジェクトと比較を行うメソッド  
  `Number`クラスを継承している`Long`や`Integer`クラスは、`Comparable`インタフェースを継承しているため、`compareTo`メソッドで比較することができる。
  しかし、`Number`クラス自体は、`Comparable`インタフェースを継承していないため`compareTo`メソッドによる比較はできない。

    - `int compareTo(Byte anotherByte)`
    - `int compareTo(Double anotherDouble)`
    - `int compareTo(Float anotherFloat)`
    - `int compareTo(Integer anotherInteger)`
    - `int compareTo(Long anotherLong)`
    - `int compareTo(Short anotherShort)`

- 他のオブジェクトと同じ値であるかを判断するメソッド  
  このメソッドは、同じ型で且つ同じ値である場合に同じ値と判断する。  
    - `boolean equals(Object obj)`

- `Integer`クラスの文字列操作のメソッド  
    - `static Integer decode(String s)` - 文字列から`Integer`に変換する。文字列の先頭の記号で何進数か判断される。
    - `static int parseInt(String s)` - 文字列をパースして`int`型を返却する
    - `static int parseInt(String s, int radix)` - 文字列をパースして`int`型を返却する
    - `static Integer valueOf(int i)` - `int`型を`Integer`型に変換する
    - `static Integer valueOf(String s)` - 文字列をパースして`Integer`に変換する。文字列は符号付十進数とみなされる。
    - `static Integer valueOf(String s, int radix)` - 文字列をパースして`Integer`に変換する。
    - `static String toString(int i)` - `int`型を文字列に変換する
    - `String toString()` - このオブジェクトを文字列に変換する

    |引数＼変換先|int|Integer|
    |---|---|---|
    |int|なし|`valueOf(int i)`|
    |10進数の文字列|`parseInt(String s)`|`valueOf(String s)`|
    |Ｎ進数の文字列と基数|`parseInt(String s, int radix)`|`valueOf(String s, int radix)`|
    |Ｎ進数の文字列(基数自動判定)|なし|`decode(String s)`|

### `java.io.PrintStream`クラスのメソッド 
`java.io.PrintStream`クラスは以下の2つのフォーマットメソッドを持つ。  

```java
public PrintStream format(String format, Object... args)
public PrintStream printf(String format, Object... args)
```

`format`で使用可能なフォーマット指定は、`C`言語の`printf`とほぼ同じである。  

- `d` - 整数10進表記
- `o` - 整数8進表記
- `x` - 整数16進表記
- `f` - 少数
- `e` - 少数 浮動小数点の10進数表記
- `n` - プラットフォーム固有の行区切り文字(`\n`の代わりに使う)

- `tB` - ローケールに合わせた長い月名
- `td` - 2桁の日
- `te` - 日（先頭に0が付加されない）
- `ty` - 2桁の年
- `tY` - 4桁の年
- `tl` - 12時間制の時
- `tM` - 2桁の分
- `tp` - ローケースに合わせたpm/am
- `tm` - 2桁の月
- `tD` - %m/%d/%y

### `java.text.DecimalFormat`クラス  
少数のフォーマットを行う。  

```java
import java.text.*;

public class DecimalFormatDemo {

   static public void customFormat(String pattern, double value ) {
      DecimalFormat myFormatter = new DecimalFormat(pattern);
      String output = myFormatter.format(value);
      System.out.println(value + "  " + pattern + "  " + output);
   }

   static public void main(String[] args) {
      customFormat("###,###.###", 123456.789);      // 123,456.789
      customFormat("###.##", 123456.789);           // 123456.79
      customFormat("000000.000", 123.78);           // 000123.780
      customFormat("$###,###.###", 12345.67);       // $12,345.67
   }
}
```

## `Character`クラス
`primitive`型の`char`型をラップするクラスです。  

```java
Character ch = new Character('a');
```
### `Character`クラスのメソッド

- `static boolean isLetter(char ch)` - 文字であるか判定する
- `static boolean isDigit(char ch)` - 数字であるか判定する
- `staic boolean isWhitespace(char ch)` - 空白であるか判定する
- `static boolean isUpperCase(char ch)` - 大文字であるか判定する
- `static boolean isLowerCase(char ch)` - 小文字であるか判定する
- `static char toUpperCase(char ch)` - 大文字に変換する
- `static char toLowerCase(char ch)` - 小文字に変換する
- `static String toString(char ch)` - 文字列に変換する


## `String`クラス
`String`クラスは ***`immutable`*** である。  
つまり、一度作成された`String`オブジェクトは、変更することができない。  
`String`クラスのいくつかのメソッドは文字列を変更するように見えるが、操作対象の`String`オブジェクトは全体に変更されず、変更された新しい`String`オブジェクトが返却されることになる。  

### `String`オブジェクトの作成方法
- リテラルからのStringオブジェクトの作成  
    コンパイラは、文字列リテラルをStringオブジェクトに変換する。  

    ```java
    String greeting = "Hello world!";
    ```

- コンストラクタからのStringオブジェクトの作成  

    ```java
    char[] helloArray = { 'h', 'e', 'l', 'l', 'o', '.' };
    String helloString = new String(helloArray);
    ```

- コンスタントプール  
    文字列リテラルは、コンスタントプールに作成される。  
    このため、同じ文字列リテラルで`String`オブジェクトを初期化すると、同じ参照を指すことになる。  

    ```java
    String a = "test";  // "test"はコンスタントプールに作成される。
    String b = "test";  // コンスタントプールの"test"を参照する。
    System.out.println(a == b);     // trueが出力される。
    ```

    文字列リテラルでない場合は、コンスタントプールには作成されない。

    ```java
    String a = new String("test");  // "test"はコンスタントプールに作成されない。
    String b = "test";  // "test"はコンスタントプールに作成される。
    System.out.println(a == b);     // falseが出力される。
    ```

    コンスタントプール状のインスタンスを参照するには、`intern`メソッドを使用する。  

    ```java
    String a = new String("test");  // "test"はコンスタントプールに作成されない。
    String b = "test";  // "test"はコンスタントプールに作成される。
    System.out.println(a.intern() == b.intern());     // trueが出力される。
    ```

### `String`クラスの主なメソッド

- `int length()` - 文字列の文字数を返却する。
- `char charAt(int index)` - `index`番目の文字を返却する。
- `String concat(String str)` - 元の文字列と引数の文字列を結合した文字列を返却する。

### `format`メソッド
`System.out.format`と同じように使うことができる。  

```java
String fs = String.format("The value of the float " +
                   "variable is %f, while " +
                   "the value of the " + 
                   "integer variable is %d, " +
                   " and the string is %s",
                   floatVar, intVar, stringVar);
System.out.println(fs);
```

### `String`オブジェクトから`Number`オブジェクトへの変換
`Number`クラスを継承するクラスは、`valueOf`メソッドを提供している。  
`valueOf`メソッドは`String`を引数にとり、それぞれのクラスオブジェクトを返却する。  

```java
// 整数の文字列からint型を得る
int a = (Integer.valueOf("123456")).intValue(); 

// 少数の文字列からfloat型を得る
float b = (Float.valueOf("123456.789")).floatValue();

// 整数の文字列から、直接int型を得る
int c = Integer.parseInt("123456"); 

// 少数の文字列から直接float型を得る
float d = Float.parseFloat("123456.789");
```

### `Number`オブジェクトから`String`オブジェクトへの変換

- 文字列と連結することで文字列を得る  

```java
int i = 100;
String s1 = "" + i;
```

- `String.valueOf`メソッドを使用する  

```java
int i = 1000;
String s2 = String.valueOf(i);
```

- `Number`クラスの`toString`メソッドを使用する。

```java
int i = 999;
double d = 12345.5454;
String s3 = Integer.toString(i); 
String s4 = Double.toString(d); 
```

### `String`オブジェクトから部分文字列の取得
`substring`メソッドを使用する。  

- `String substring(int beginIndex, int endIndex)`  
`beginIndex`から`endIndex - 1`までの部分文字列を返却する。  
- `String substring(int beginIndex)`  
`beginIndex`から最後の文字までの部分文字列を返却する。  

### `String`オブジェクトの文字列操作メソッド

- `String[] split(String regex)`  
正規表現を含むも引数の文字列で文字を分割して、分割した文字列の配列を返却する。  
- `String trim()`  
文字列前後の空白を取り除いた文字列を返却する。  
- `String toLowerCase()`  
小文字に変換した文字列を返却する。  
- `String toUpperCase()`  
大文字に変換した文字列を返却する。  
- `int indexOf(int ch)`  
指定した文字が出現する最初の位置を返却する。  
- `int lastIndexOf(int ch)`  
指定した文字が出現する最後の位置を返却する。  
- `int indexOf(String str)`  
指定した文字列が出現する最初の位置を返却する。  
- `int lastIndexOf(String str)`  
指定した文字列が出現する最後の位置を返却する。  
- `boolean contains(CharSequence s)`  
指定した文字列が含まれているかどうかを判定する。  
- `String replace(char oldChar, char newChar)`  
指定した文字を新しい文字にすべて置き換えた新しい文字列を返却する。  
- `String replace(CharSequence target, CharSequence replacement)`  
指定した文字列を新しい文字列にすべて置き換えた新しい文字列を返却する。  
- `String replaceAll(String regex, String replacement)`  
正規表現で指定したパターンにマッチするすべての文字列を指定した文字列で置き換えた新しい文字列を返却する。  
- `String replaceFirst(String regex, String replacement)`  
正規表現で指定したパターンにマッチする最初の文字列を指定した文字列置き換えた新しい文字列を返却する。  
- `boolean endsWith(String suffix)`  
指定した文字列で終了しているかどうか判定する。  
- `boolean startsWith(String prefix)`  
指定した文字列始まっているかどうか判定する。  
- `int compareTo(String anotherString)`  
指定した文字列と比較する。  
- `int compareToIgnoreCase(String str)`  
指定した文字列と大文字小文字を区別せず比較する。  
- `boolean equals(Object anObject)`  
指定されたオブジェクトが、同じ文字列を表す`String`であるかどうか判断する。  
- `boolean matches(String regex)`  
指定された正規表現にマッチするかどうか判定する。  


## StringBuilderクラス  
`StringBuilder`オブジェクトは`String`オブジェクトと異なり変更させることができる。  
`StringBuilder`オブジェクトは、その長さと内容をどの時点でも変更させることができる。  

### StringBuilderクラスの長さとキャパシティ

- `int length()`  
`String`オブジェクトと同様に、文字列の長さが返却されます。  
- `int capacity()`  
現在のキャパシティ(バッファサイズ)が返却される。  
文字列の長さがキャパシティを超えると、自動的に再アロケートされる。  
キャパシティは、コンストラクタで指定することができる。

### StringBuilderクラスのコンストラクタ

- `StringBuilder()`  
***16文字分のからのバッファが用意される***。  
- `StringBuilder(CharSequence cs)`  
***指定した文字列＋16文字分*** のバッファが用意される。  
- `StringBuilder(int initCapacity)`  
指定したキャパシティで文字列バッファを作成する。  

### StringBuilderクラスのメソッド

- `void setLength(int newLength)`  
現在の文字列長よりも短い長さが指定された場合は、文字列を切り詰める。  
現在の文字列長よりも長い長さが指定された場合は、その長さまでnullが詰められる。  
- `StringBuilder append(String s)`  
文字列を末尾に追加する。  
- `StringBuilder delete(int start, int end)`  
指定した範囲の文字列を削除する。  
- `StringBuilder deleteCharAt(int index)`  
指定した位置の文字を削除する。  
- `StringBuilder insert(int offset, String s)`  
指定した位置に文字列を挿入する。  
- `StringBuilder replace(int start, int end, String s)`  
指定した部分文字列を指定した文字列で置き換える。  
- `StringBuilder reverse()`  
文字列をさかさまにする。
- `String toString()`  
`String`オブジェクトを返却する。


## `Generics`  
### `Generic Type`  
型をパラメータにするクラスやインタフェース。  
以下の形式をとる。  

```java
class ClassName<T1, T2, ..., Tn> { /* ... */ }
```

### パラメータ名の慣習
慣習として、型パラメータは大文字１文字を使用する。

- `E` - Element (used extensively by the Java Collections Framework)
- `K` - Key
- `N` - Number
- `T` - Type
- `V` - Value
- `S,U,V` etc. - 2nd, 3rd, 4th types

### コンストラクタ  
コンストラクタでも、`<>`の中に型を記述して`new`でオブジェクトを作成する。  

```java
Box<Integer> integerBox = new Box<Integer>();
```

***java SE7から、コンストラクタの場合のみ`<>`の中の型を省略できるようになった***。  

```java
Box<Integer> integerBox = new Box<>();
```

### `Generic Method`
型をパラメータとするメソッド。  
`Generic Type`と似ているが、パラメータのスコープがメソッドに限定される。  

`static`な`Generic Method`は、返却値の型の前に型パラメータを記述する。  

```java
public class Util {
    public static <K, V> boolean compare(Pair<K, V> p1, Pair<K, V> p2) {
        return p1.getKey().equals(p2.getKey()) &&
               p1.getValue().equals(p2.getValue());
    }
}
public class Pair<K, V> {
    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public void setKey(K key) { this.key = key; }
    public void setValue(V value) { this.value = value; }
    public K getKey()   { return key; }
    public V getValue() { return value; }
}
```

上の`Generic static method`は以下の様に使用する。  

```java
Pair<Integer, String> p1 = new Pair<>(1, "apple");
Pair<Integer, String> p2 = new Pair<>(2, "pear");

// 型パラメータを明示的に指定して使用する
boolean same = Util.<Integer, String>compare(p1, p2);

// 型パラメータを省略することもできる
boolean same2 = Util.compare(p1, p2);
```

### `Bounded Type Parameters`  
***`Generic`で指定する型パラメータが、特定のクラスのサブクラスであることや、特定のインタフェースを実装するように制限する***。  

以下の例では、`inspect`メソッドの引数の型`U`がNumberクラスを継承していることを制限としている。  
しかし、`main`メソッドで`Number`クラスを継承していない`String`クラスのオブジェクトを指定しているので ***コンパイルエラー*** となる。

```java
public class Box<T> {
    private T t;

    public void set(T t) {
        this.t = t;
    }

    public T get() {
        return t;
    }

    public <U extends Number> void inspect(U u){ 
        System.out.println("T: " + t.getClass().getName());
        System.out.println("U: " + u.getClass().getName());
    }

    public static void main(String[] args) {
        Box<Integer> integerBox = new Box<Integer>();
        integerBox.set(new Integer(10));
        integerBox.inspect("some text"); // error: this is still String!
    }
}
```

以下の例では、`T`は`Number`クラスを継承しているので、そのメソッド`intValue`を呼び出すことができている。  

```java
public class NaturalNumber<T extends Integer> {
    private T n;

    public NaturalNumber(T n)  { this.n = n; }

    public boolean isEven() {
        return n.intValue() % 2 == 0;
    }

    // ...
}
```

***以下の様に記述して、複数のクラスやインタフェースを継承・実装していることと制限することができる***。  

```java
<T extends B1 & B2 & B3>
```

例えば、以下のように記述する。  
***継承するクラスを最初に記述しないと、コンパイルエラーになる***。  

```java
Class A { /* ... */ }
interface B { /* ... */ }
interface C { /* ... */ }

class D <T extends A & B & C> { /* ... */ }
```

`Generic Method`で`Bounded Type Parameters`を使う例。

```java
public static <T extends Comparable<T>> int countGreaterThan(T[] anArray, T elem) {
    int count = 0;
    for (T e : anArray)
        if (e.compareTo(elem) > 0)
            ++count;
    return count;
}
```

### `Generic Type`の継承  
`class Child`が`class Parent`を継承しているとする。  

```java
public class Parent {}
public class Child extends Parent {}
public class Box<T> {
}
```

***この場合に、`Box<Child>`と`Box<Parent>`は継承関係にない***。  

つまり、 ***継承関係は、`extends`と`implements`でしか定義できない*** ためである。  
`ArrayList<E>`と`List<E>`と`Collection<E>`は継承関係にある。  

```java
class ArrayList<E> implements List<E> {}
interface List<E> extends Collection<E> {}
```

### 型の推論  
以下の例では、Tは`String`,`ArrayList<String>`,`Serializable`等いろいろ取れる。  
コンパイラの型の推論により、`Serializable`と決定される。

```java
static <T> T pick(T a1, T a2) { return a2; }
Serializable s = pick("d", new ArrayList<String>());
```

以下のように型推論を使うことで、明示的な型の指定を省略することができる。  

```java
Map<String, List<String>> myMap = new HashMap<String, List<String>>();
Map<String, List<String>> myMap = new HashMap<>();
```

***しかし、以下はコンパイラがワーニングを出力する***。  

```java
Map<String, List<String>> myMap = new HashMap();
```

`generic`型であるかどうかに関わらず、コンストラクタは`generic`にすることができる。  

```java
class MyClass<X> {
  <T> MyClass(T t) {
  }
}
```

このコンストラクタは以下のように使用する。  

```java
// Java SE7より前
MyClass<Integer> myObject = new MyClass<Integer>("");
// java SE7以降
MyClass<Integer> myObject = new MyClass<>("");
// すべての型パラメータを指定することもできる。
MyClass<Integer> myObject = new <String>MyClass<Integer>("");
```

### ターゲット型  
コンパイラは型の`generic method`の呼び出し時に型の推論を行う時に、ターゲットの型（最終的に代入される変数の型）を利用する。  

以下の`generic method`を想定する。  

```java
static <T> List<T> emptyList();
```

以下の例では、最終的な型が`List<String>`であるので、型からメータTを`String`と推論される。  

```java
// 明示的に型パラメータを指定しなくてもよい。
List<String> listOne = Collections.emptyList();
// 明示的に型パラメータを指定してもよい。
List<String> listOne = Collections.<String>emptyList();
```

別の例として、以下のメソッドを想定する。  

```java
void processStringList(List<String> stringList) {
    // process stringList
}
```

メソッドの引数の型から、ターゲット型が`List<String>`と推論される。  
しかし、javaのバージョンによって、ターゲット型が型推論に利用されるかどうかが異なる。  

```java
// java SE8の場合は、明示的な型パラメータは指定しなくてもよい。
processStringList(Collections.emptyList());
// java SE7以前の場合は、明示的な型パラメータを指定する必要がある。
processStringList(Collections.<String>emptyList());
```

### `Upper Bounded Wildcards`
引数の型で`generic`型を指定する場合に、少しゆるくした型パラメータの指定方法。  

`List<? extends Foo>`での`?`は、`Foo`のサブクラスである。  

```java
public static double sumOfList(List<? extends Number> list) {
    double s = 0.0;
    for (Number n : list)
        s += n.doubleValue();
    return s;
}
```

***上のワイルドカードは、次の`Bounded Type Parameters`とほぼ同じである***。  
***違いは、`Bounded Type Parameters`の場合は、そのパラメータ型を使用できることである***。  

```java
public static <T extends Number>double test2(List<T> list) {
    double s = 0.0;
    for (Number n : list)
        s += n.doubleValue();
    return s;
}
```

### `Unbounded Wildcards`
全くの不確定の型を指定する場合に使用するワイルドカード  

例えば、任意のリストの中身をコンソールに出力するメソッドを実装する場合。  
***以下のメソッドは、`List<Integer>`, `List<String>`, `List<Double>`等のリストを引数にとることができない***。  
***なぜならば、これらのリストは`List<Object>`のサブクラスではないからである***。

```java
public static void printList(List<Object> list) {
    for (Object elem : list)
        System.out.println(elem + " ");
    System.out.println();
}
```

これに解決するために、以下の`Unbounded Wildcards`を使用する。  
***ここで`?`はすべての型を許容するワイルドカードである***。  

```java
public static void printList(List<?> list) {
    for (Object elem: list)
        System.out.print(elem + " ");
    System.out.println();
}
```

### `Lower Bounded Wildcards`
***ある型のスーパークラスであることを制限する指定方法***。  
***これは`<? super T>`の形式で指定する***。  

以下のメソッドは、引数のIntegerのスーパークラスの`List`型をとる。  
つまり、 `List<Integer>`, `List<Number>`, `List<Object>`のいずれの型の引数でもOKである。  

```java
public static void addNumbers(List<? super Integer> list) {
    for (int i = 1; i <= 10; i++) {
        list.add(i);
    }
}
```

### `Wildcards and Subtyping`
`Generic Type`で継承関係を作るために、ワイルドカードを使用することができる。

ワイルドカードを使用しない場合、以下の例ではコンパイルエラーとなる。  

```java
// BはAのサブクラス
class A { /* ... */ }
class B extends A { /* ... */ }

// BとAではポリモーフィズムを適用できる。
// つまり、以下のコードはOKである。
B b = new B();
A a = b;

// しかし、List<B>はList<A>のサブクラスではない！
List<B> lb = new ArrayList<>();
List<A> la = lb;   // compile-time error
```

`Generic Type`間で、サブタイプの構造を実装するためには、ワイルドカードを使用する。  
以下はコンパイルエラーにならない。  

```java
List<? extends Integer> intList = new ArrayList<>();
List<? extends Number>  numList = intList;
```
`List<? extends Integer>`は`List<? extends Number>`のサブクラスである。

### `wildcard`を使い分けるガイドライン
引数の種類として、メソッドにデータを与える入力引数と、メソッドから結果を受け取る出力引数の2種類に分類する場合。

- 入力引数は`extends`キーワードを使って、`upper bounded wildcard`として定義できる。
- 出力引数は`super`キーワードを使って、`lowerd bounded wildcard`として定義できる。
- `Object`クラスのメソッドを使ってアクセスできる入力引数の場合、`unbounded wildcard`として定義できる。
- 入力引数と出力引数のどちらとしても利用される引数の場合、`wildcard`を使うべきではない。

なお、このガイドラインはメソッドの返却値には適用できない。  
メソッドの返却値に`wildcard`を使うのは、避けるべきである。  

### 型の消去
Javaコンパイラは`generic type`を実装するために、型の消去を行う。  
型の消去とは、型パラメータを`Object`や`mounded type`に置き換える処理である。  

以下のように、型パラメータが`unbounded`の場合  

```java
public class Node<T> {

    private T data;
    private Node<T> next;

    public Node(T data, Node<T> next) {
        this.data = data;
        this.next = next;
    }

    public T getData() { return data; }
    // ...
}
```

Javaコンパイラは型パラメータを`Object`に置き換える。

```java
public class Node {

    private Object data;
    private Node next;

    public Node(Object data, Node next) {
        this.data = data;
        this.next = next;
    }

    public Object getData() { return data; }
    // ...
}
```

以下のように、`upper bounded`な型パラメータの場合  

```java
public class Node<T extends Comparable<T>> {

    private T data;
    private Node<T> next;

    public Node(T data, Node<T> next) {
        this.data = data;
        this.next = next;
    }

    public T getData() { return data; }
    // ...
}
```

以下のように、`bounded type`である`Comparable`に置き換えられる。  

```java
public class Node {
    private Comparable data;
    private Node next;

    public Node(Comparable data, Node next) {
        this.data = data;
        this.next = next;
    }

    public Comparable getData() { return data; }
    // ...
}
```

同様に、`generic method`でも型消去が行われる。  
以下のように、`unbounded`の型パラメータの場合  

```java
public static <T> int count(T[] anArray, T elem) {
    int cnt = 0;
    for (T e : anArray)
        if (e.equals(elem))
            ++cnt;
        return cnt;
}
```

型パラメータは`Object`に置き換えられる。

```java
public static int count(Object[] anArray, Object elem) {
    int cnt = 0;
    for (Object e : anArray)
        if (e.equals(elem))
            ++cnt;
        return cnt;
}
```

以下の様なクラスと`generic method`がある場合  

```java
class Shape { /* ... */ }
class Circle extends Shape { /* ... */ }
class Rectangle extends Shape { /* ... */ }

public static <T extends Shape> void draw(T shape) { /* ... */ }
```

型パラメータは、`bounded type`である`Shape`に置き換えられる。  

```java
public static void draw(Shape shape) { /* ... */ }
```

### 型の消去と`Bridge Method`
javaコンパイラが型消去を行う場合に、`Bridge Method`が自動的に作成される。  

以下の様なクラスがある場合  

```java
public class Node<T> {
    public T data;
    public Node(T data) { this.data = data; }
    public void setData(T data) {
        System.out.println("Node.setData");
        this.data = data;
    }
}

public class MyNode extends Node<Integer> {
    public MyNode(Integer data) { super(data); }
    public void setData(Integer data) {
        System.out.println("MyNode.setData");
        super.setData(data);
    }
}
```

以下のコードは  

```java
MyNode mn = new MyNode(5);
Node n = mn;            // A raw type - compiler throws an unchecked warning
n.setData("Hello");     
Integer x = mn.data;    // Causes a ClassCastException to be thrown.
```

型消去により、以下のコードになる。  

```java
MyNode mn = new MyNode(5);
Node n = (MyNode)mn;         // A raw type - compiler throws an unchecked warning
n.setData("Hello");
Integer x = (String)mn.data; // Causes a ClassCastException to be thrown.
```

`Node`クラスは型消去の結果、型パラメータ`T`は`Object`となる。  
このため、`n.setData("Hello")`は`Object`型のフィールドに`String`の参照が設定される。  
このため、コンパイラはこのフィールドを扱う時に`String`型にキャストする。  

### `Generic`の制限
`Generic`の幾つかの制限を以下に挙げる。  

- 型パラメータに`primitive`型を指定して、インスタンスを生成すること。  

```java
class Pair<K, V> { ... }

Pair<int, char> p = new Pair<>(8, 'a');  // compile-time error
```

- 型パラメータのインスタンスを生成すること。  

```java
public static <E> void append(List<E> list) {
    E elem = new E();  // compile-time error
    list.add(elem);
}
public static <E> void append(List<E> list, Class<E> cls) throws Exception {
    E elem = cls.newInstance();   // OK
    list.add(elem);
}
```

- 型パラメータの`static`フィールドを作成すること。

```java
public class MobileDevice<T> {
    private static T os;    // NG
}
```

- `instanceof`を使うこと。

```java
public static <E> void rtti(List<E> list) {
    if (list instanceof ArrayList<Integer>) {  // compile-time error
    }
}
public static void rtti(List<?> list) {
    if (list instanceof ArrayList<?>) {  // OK; instanceof requires a reifiable type
    }
}
```

- `unbounded wildcard`でない型パラメータの場合にキャストすること。  

```java
List<Integer> li = new ArrayList<>();
List<Number>  ln = (List<Number>) li;  // compile-time error
```

    しかし、ある状況ではキャストが問題にならない場合もある。  

```java
List<String> l1 = ...;
ArrayList<String> l2 = (ArrayList<String>)l1;  // OK
```

- `Generic`タイプの配列を作ること。  

```java
List<Integer>[] arrayOfLists = new List<Integer>[2];  // compile-time error
```

- `Exception`や`Throwable`を継承した`Generic`タイプを宣言すること。  

```java
// Extends Throwable indirectly
class MathException<T> extends Exception { /* ... */ }    // compile-time error
// Extends Throwable directly
class QueueFullException<T> extends Throwable { /* ... */ // compile-time error
```

- 型引数をを`catch`すること。  
以下はコンパイルエラーになる。  

```java
public static <T extends Exception, J> void execute(List<J> jobs) {
    try {
        for (J job : jobs)
            // ...
    } catch (T e) {   // compile-time error
        // ...
    }
}
```

    しかし、以下はコンパイルエラーにならない。  
    ※catchできないので使えないが。  

```java
class Parser<T extends Exception> {
    public void parse(File file) throws T {     // OK
        // ...
    }
}
```

- 型パラメータを変更してオーバーロードすること。  
下のコードはコンパイルエラーになる。  

```java
public class Example {
    public void print(Set<String> strSet) { }
    public void print(Set<Integer> intSet) { }
}
```

## `package`

### `package`とファイルのルール
- `package`ステートメントは、１ファイルに1つだけ記述できる。
- `package`ステートメントは、必ず ***先頭*** に記述する。
- `package`ステートメントの前にはコメントのみ記述することができる。
- １つのクラスを１つのファイルに実装する。
- クラス名とファイル名を一致させる。
- ***`public`でないクラスは、複数同一のファイルに宣言できる*** が、推奨されない。
- ***明示的なパッケージを指定しない場合、無名パッケージ(デフォルトパッケージ)に所属することになる***。  
- 無名パッケージに属するクラスは、**他のパッケージからは使用することができない**。

### `package`名の規則
- `package`名はすべて小文字にする。
- 企業は、`internet domain name`をパッケージ名として使う。  
- 特殊なドメイン名の場合は、以下のように変更して使う。  
    + `hyphenated-name.example.org` -> `org.example.hyphenated_name`
    + `example.int` -> `int_.example`
    + `123name.example.com` -> `com.example._123name`

### `package`内のクラスの参照の方法  

- `java.lang.*`は自動的にインポートされる。  
- 完全な装飾名を使用する。  

```java
java.util.List<Integer> a = new java.util.ArrayList<Integer>();
```

- 使用するクラスをインポートする。  

```java
import java.util.List;
import java.util.ArrayList;
```

- パッケージ内の全クラスをインポートする。  

```java
import java.util.*;
```

### `Nested Class`のインポート方法
`Nested Class`をインポートする場合は、以下のようにする。  
なお、`graphics.Rectangle.*`では`graphics.Rectangle`の様なサブパッケージはインポートされない。  

```java
import graphics.Rectangle;
import graphics.Rectangle.*;
```

### パッケージ階層の意味
以下のパッケージがあり、改造関係を形成しているように見えるが、階層関係ではない。  
- `java.awt`
- `java.awt.color`
- `java.awt.font`

したがって、`import java.awt.*;`では、`java.awt.color`や`java.awt.font`に含まれるクラスをインポートすることはできない。  
それぞれのパッケージに対して`import`する必要がある。  

```java
import java.awt.*;
import java.awt.color.*;
```

### あいまいなクラスの指定
`import`する複数のパッケージで同じ名前のクラスが存在する場合、完全な装飾名を指定する必要がある。  
例えば、`graphics`パッケージで`Rectangle`クラスを定義して、`java.awt`パッケージも`import`する場合、以下のようになる。

```java
import graphics.*;
import java.awt.*;

class aaa {
    public void test() {
        // 完全な装飾名でクラスを指定する。
        graphics.Rect rect = new graphics.Rect();
    }
}
```

### `static import`
クラスが定義している`static`フィールドやメソッドは、`import static`でインポートすることができる。  
例えば、`java.lang.Math`クラスは、`PI`を`static final`で定義している。  

```java
public class Math {
    public static final double PI = 3.141592653589793;
    public static double cos(double a) {}
}
```

通常は、以下のように使う必要がある。  

```java
double r = Math.cos(Math.PI * theta);
```

そこで、`static import`を使うとシンプルになる。  

```java
import static java.lang.Math.PI;
(あるいは)
import static java.lang.Math.*;

double r = cos(PI * theta);
```

## その他試験対策
### エントリーポイント  
プログラムのエントリーポイントは`main`で以下の決まりがあります。  
- `public`であること。
- `static`であること。
- `String[]`型の引数か、String型の可変長引数であること。
- `void`型であること。

つまり、以下の形式である。

```java
public static void main(String[] args) {
}
```

### プログラムの起動
以下の2つの方法でプログラムを起動することができる。

- コンパイルしたクラス指定  

    ```java
    java Test
    ```

- ソースファイル指定(java se11から)
ソースファイルを指定すると、動的にコンパイルして、実行される。  

    ```java
    java Test.java
    ```

    `--source {java version}`オプションを使用すと、自動的にソースファイルモードになる。  
    こうすることで`.java`以外の拡張子でも対応できる。  

    ```java
    java --source 11 Test.txt
    ```

### 起動パラメータ
- プログラムの起動パラメータがスペースを含む場合は、`"`(ダブルクォーテーション)で囲む。  

    ```java
    java Hello "My Name is Ken." "I am 48 year old."
    ```

- `"`(ダブルクォーテーション)自体を引数として指定したい場合は、`\`でエスケープする。  
以下は、`"A`と`B"`を引数で指定している。  
    
    ```java
    java Hello \"A B\"
    ```

- `"`(ダブルクォーテーション)と空白以外を続けると一つの引数とみなされる。  
以下は`ABCD`を指定したことになる。  

    ```java
    java Hello "ABC"D
    ```

### var型変数（java se10から）
`var`型変数は、コンパイル時にコンパイラが型推論を行い、特定した型で`var`を置き換える機能です。  
このため、コンパイル時の型推論ができないと、コンパイルエラーになります。  
***`var`はローカル変数にしか使用できません***。

以下に、コンパイルエラーになる例を示す。  

```java
var a;
var b = null;
var c = () -> ();   // lamdba型は明示的な型が必要なため。
var d = {1, 2, 3}   // int[]かlong[]が判断できない。
```

以下は、OKな例です。
```java
var a = new Smaple();
var b = test.method1();
var b = Math.PI;
var c = new ArrayList<>();  // new ArrayList<Object>と型推論される。
```

### 文字列と数値の結合
左から順に結合して行く。  
最初に数値の演算があれば、それを行う。  
文字列と数値の`+`での結合は、数値を文字列に直してから結合する。  

```java
System.out.println(10 + 20 + "a");      //  出力は 30a
System.out.println("a" + 10 + 20);      //  出力は a1020
```

### Stringの結合
`null`である`String`は`"null"`という文字列になる。  

```java
String a = null;
String b = a + "null";      // bは"nullnull"となる。
```

### Comparatorインタフェース
`compare`メソッドで、`-1`を返すと、第１引数、第２引数の順番でソートされる。
`compare`メソッドで、`1`を返すと、第２引数、第１引数の順番でソートされる。
`compare`メソッドで、`0`を返すと、第１引数、第２引数の順番は変更されない。

```java
public int compare(T a, T b) {
    if (....) {
        return -1;
    } else if (....) {
        return 1;
    } else {
        return 0;
    }
}
```

`Integer.compareTo`メソッドは、昇順ソートする場合の結果を返却します。

```java
Integet a = 1;
Integer b = 2;
int  ret = a.compareTo(b);  // -1
```

### LocalDateクラス
`java.util.Calendar`と異なり、`immutable`なクラスである。  
`java.util.Calendar`と異なり、月は`1`から始まる。

```java
LocalDate a = LocalDate.of(2020, 1, 2)      // 2020年1月2日
LocalDate b = LocalDate.parse("2020-01-01") // 2020年1月2日
LocalDate c = LocalDate.now                 // 今日の日付
```

### ArrayListクラス
- `null`を追加することもできる。
- 値が重複しても良い。
- スレッドセーフでない。
- スレッドセーフなリストは、`java.util.Vector`クラス
- 動的な配列として動作する。
- 値を追加する箇所を制御できる。

`ArrayList.add`では追加する位置を指定できる。
ただし、存在しない位置を指定すると、実行時エラーになる。

```java
ArrayList<String> list = new ArrayList<>();
list.add("A");              // A 
list.add("B");              // A, B
list.add(1, "C");           // A, C, B
```

`ArrayList.set`メソッドは、置換する。

```java
ArrayList<String> list = new ArrayList<>();
list.add("A");              // A 
list.set(0, "B");           // B
list.add("C");              // B, C
list.add(1, "D");           // B, D
```

`ArrayList.remove`メソッドは、削除する。
インデックスを指定した場合は、指定した位置の要素を削除する。
`Object`を指定した場合は、最初の同値(`equals`メソッドでの比較)の要素が削除される。

```java
ArrayList<String> list = new ArrayList<>();
list.add("A");              // A 
list.add("B");              // A, B
list.add("C");              // A, B, C
list.add("A");              // A, B, C, A
list.remove(1);             // A, C, A
list.remove("A");           // C, A
```

`ArrayList`を列挙中に要素を削除すると、カーソルがずれる。
下の例では、`B`を削除すると、その瞬間にカーソルが`C`を指すので、次の`for`で`C`が列挙されない。

```java
ArrayList<String> list = new ArrayList<>();
list.add("A");              // A 
list.add("B");              // A, B
list.add("C");              // A, B, C

for (String str: list) {
    if ("B".equals(str)) {
        list.remove(str);
    } else {
        System.out.println(str);
    }
}
```

しかし、`remove`を行った後、取り出しを行うと例外になる。
下の例では、`A`を削除することで、カーソルが`B`を指し、次の`for`で`C`を取り出そうとして、例外になる。
```java
ArrayList<String> list = new ArrayList<>();
list.add("A");              // A 
list.add("B");              // A, B
list.add("C");              // A, B, C

for (String str: list) {
    if ("A".equals(str)) {
        list.remove(str);
    } else {
        System.out.println(str);
    }
}
```

`removeIf`は`Predicate<T>`を引数にとり、マッチした要素を取り除く。
下の例では、`Lambda`関数を使って偶数を取り除いている。

```java
ArrayList<Integer> list = new ArrayList<>(List.of(1, 2, 3));
list.removeIf((value)->{return value % 2 == 0;});   // 偶数を取り除く
for (Integer v : list) {
    System.out.println(v);
}
```


### Arrays.asList
`mutable`だが、固定サイズのリストを作成する。  
`add`,`remove`を行うと例外が発生する。  
`set`では例外は発生しない。  
`primitive`型は指定できない

```java
var list = Arrays.asList(new Integer[] {1, 2, 3});
list.add(5); // 例外が発生する
```

### List.of
`immutable`なリストを作成する。
`set`,`add`,`remove`を行うと例外が発生する。

```java
var list = List.of(1, 2, 3);
list.set(0, 5); // 例外が発生する
```

### Arrays.mismatch
２つの配列の要素を先頭から比較して、最初の不一致の要素のインデックスを返却する。  
全ての要素が一致している場合は、`-1`が返却される。  

```java
String [] a = {"a", "b"};
String [] b = {"a", "c", "d"};
System.out.println(Arrays.mismatch(a, b));  // 1が出力される
```

### Arrays.compare
２つの配列の要素を先頭から辞書順で比較して、比較結果を返却する。
比較結果は、第1引数と第2引数が入れ替わる場合に、`1`となる。

```java
String [] a = {"a", "b"};
String [] b = {"a", "c"};
System.out.println(Arrays.compare(a, b));  // -1が出力される
```

### List.forEach
`List.forEach`は`Consumer`型の引数を指定できる。

```java
List<String> list = List.of("a", "b", "c");
lsit.forEach((str)->{System.out.println(str);});
```

メソッド参照を使用すると、以下の様にも記述できる。
`System.out.println`メソッドは`Consumer`型であるとみなされるため。

```java
List<String> list = List.of("a", "b", "c");
lsit.forEach(System.out::println);
```

### HashMap
`HashMap`はキーとバリュー共に`null`を許容している。

```java
Map<Integer, String> map = new HashMap<Integer, String>();
map.put(1, "a");
map.put(2, "b");
map.put(1, "A");    // キー1の値が"A"になる。
map.put(null, "c"); // キーとしてnullも許与される。
```

### try-catch
到達不可能なコードになる`catch`が存在するとコンパイルエラーになる。  
以下は到達不可能な`catch`の例です。
`RuntimeException`は`Exception`のサブクラスであるので、先に`Exception`にマッチしてしまうので、`RuntimeException`が`catch`されることはない。
コンパイルエラーにならないようにするためには、サブクラスから先にcatchするように記述する。  

```java
try {
} catch (Exception e) {
} catch (RuntimeException e) {
}
```

tryにはcatchまたはfinallyのいずれかがあればよい。

```java
// コンパイル成功
try {
} finally {
}
```

`catch`と`finally`の両方で`return`した場合は、`finally`の返却値が返る。

```java
public class Test {
    public static void main(String[] args) {
        int result = test();
        System.out.println(result); // 20になる
    }
    public static void test() {
        try {
            throw new RuntimeException();
        } catch (RuntimeException e) {
            return 10;
        } finally {
            return 20;
        }
        return 30;
    }
}
```

### try-with-resouces
`try-with-resources`構文は、`java.io.Closeable`インターフェースを実装しているリソースを自動的に閉じます。  
ここで宣言された変数は、`try`内でしか参照することができません。

```java
    try (BufferedReader br = new BufferedReader(new FileReader(path))) {
        return br.readLine();
    } catch (Exception e) {
    } finally {
        // ここではbrを参照できない。
    }
```

### エラー、検査例外、非検査例外

- エラー - プログラムで対処できない問題  
    `OutOfMemoryError`/`NoClassFoundError`/`StackOverflowError`等

- 非検査例外 - `RuntimeException`とそのサブクラス
- 検査例外 - 非検査例外以外

検査例外は、明示的に`catch`するか、`throws`句でスローすることを示さないと、コンパイルエラーになる。
エラーと非検査例外は、明示的に`catch`する必要も、`throws`句でスローすることを示す必要がない、

### ArrayIndexOutOfBoundsException
以下の様に配列のサイズを超えて操作を行うと、`ArrayIndexOutOfBoundsException`例外が発生する。  
`ArrayIndexOutOfBoundsException`クラスは`IndexOutOfBoundsException`のサブクラス。

```java
int [] a = new int [] { 1, 2, 3};
int x = a[4];
```

### IndexOutOfBoundsException
以下の様に`ArrayList`のサイズを超えて操作を行うと、`IndexOutOfBoundsException`例外が発生する。

```java
List<String> list = new ArrayList<>();
list.get(0);
```

### モジュールシステム
- `java9`から導入された。
- `module-info.java`にモジュールの設定を記述する。
- モジュールの設定では、どのパッケージを公開するか記述する。
- モジュールの設定では、どのモジュールを使用するか記述する。
- `module-info.java`は、モジュールのルートディレクトリに配置する。
- 実行時には`--module-path`オプションを指定して、モジュールのディレクトリを指定する。
- 実行時には`-m`オプションを指定して、実行するモジュールのクラスを`モジュール名/クラス名`形式で指定する。  
    例えば、`hello/com.sample.Main`の様に指定する。

    `module-info.java`の内容  
    - `exports`で公開するパッケージを記述する。
    - `requires`で使用するモジュールを指定する。

    ```java
    module Sample {
        exports com.sample;
        requires test;
    }
    ```

- `requires`に`transitive`をつけると、階層化されたモジュールの依存関係で、モジュールを公開することができる。
例えば、以下の様に`a`が`b`を参照でき、`b`が`c`を参照できる状況で、`a`が`c`を参照できるようになる。

    ```java
    module a {
        requires b;
    }

    module b {
        requires transitive c;
    }
    ```


- モジュールは`jar`コマンドでまとめることができる。

    ```batch
    jar --create --file=jarの名前 --main-class=メインクラス -C モジュールのディレクトリ
    ```

- `java.base`モジュールは、標準で組み込まれるモジュールで、`requires`を指定する必要はない。
- モジュールの設定情報を確認するためのコマンドは、以下の２つがある。

    ```batch
    java --describe-module
    jmod describe
    ```

- `module-info.java`で`exports`されていないパッケージを、一時的に利用する場合は、`--add-exports`コンパイルオプションを使用する。
- 相互参照するモジュールの関係は認められていない（コンパイルエラーになる）。
- モジュール名はパッケージ名と同じ命名ルールが推奨されるが、必須ではない。

- クラスの依存関係を調べるコマンド

    ```batch
    jdeps --list-deps
    ```

- モジュールの依存関係を調べるコマンド

    ```batch
    java --show-module-resolution
    ```

- アプリケーションのモジュールを作るときに、そのアプリケーションが使うJREのモジュールを含めると、Javaがインストールされていないプラットフォームでも、そのアプリケーションを実行することができる様になる。

- モジュールの実行  
    XXXXモジュール内にあるmainメソッドを持つYYYYクラスを実行する場合。

    ```java
    // 以下の様に起動する
    java --module-path モジュールのルートディレクトリ -m 実行したいモジュール名/完全修飾クラス名

    // 具体的には以下の様になる。
    java --module-path mods -m XXXX/YYYY
    ```

### 重箱の墨を突っつく問題
- 以下の様な変数の初期化を考える。  
    ```java
    int a = 3;
    int b = a += 5;
    ```

    まず、`a += 5`で`a`が`8`になる。  
    その後、`b`に`a`が代入されて、`b`も`8`になる。

- `boolean`の比較  
    `boolean`に利用できる比較演算子は、`==`と`!=`だけである。  
    したがって、以下はコンパイルエラーになる。

    ```java
    boolean a = true;
    boolean b = true;
    System.out.println(a <= b);
    ```

- 論理演算子`&&`と`||`  
    右側の評価が不要な場合は、評価されない。  
    以下の`if`文では、左側の`a < 3`が`false`であるので、右側は一切評価されない。  
    このため、`b`は`1`のままである。  

    ```java
    int a = 5;
    int b = 1;
    if (a < 3 && b++ > 1) {
    }
    ```

- 論理演算子`&`と`|`
    常に左右の式が評価される。 
    以下の`if`文では、左側の`a < 3`が`false`であるが、右側の`b++ > 1`は評価される。
    このため、`b`は`2`になる。

    ```java
    int a = 5;
    int b = 1;
    if (a < 3 & b++ > 1) {
    }
    ```

- ラベルをつけられる位置  
以下のすべて  
    - `if`文や`switch`文
    - 式
    - 代入
    - `return`文
    - `try`ブロック

- `System.out.println`でオブジェクトを指定する。  
    オブジェクトのハッシュコードが表示される。  

- `static`なフィールドは、`クラス.`でも`インスタンス.`でも参照できる。  

    ```java
    class Test {
        static int a = 100;
    }
    class Main {
        public void main(STring[] args) {
            Test test = new Test();
            int x = test.a; // これもOK
            int y = Test.a; // これもOK
        }
    }
    ```

- 到達不能なコード 
    到達不能なコードでコンパイルエラーになる。  

    ```java
    public void main(String[] srgv) {
        int a = 0;
        return a;
        for (int x = 0; x < 5; ++ x) {  // ここでコンパイルエラーになる。
            ;
        }
    }
    ```

- どのオーバーライドメソッドを呼び出せばよいか分からない場合は、コンパイルエラーになる。  

    ```java
    public class Main {
        public void main(String[] argv) {
            Main m = new Main();
            m.test(1, 2);       // どちらのメソッドを呼び出しているか分からないためコンパイルエラーになる。
        }
        public void test(int a, int b) {
            return a + b;
        }
        public void test(double a, double b) {
            return a + b;
        }
    }
    ```

- コンストラクタのアクセス修飾子  
    あらゆるアクセス修飾子が指定可能！

- コンストラクタの返却値  
    - 指定できない！  
    - 指定するとコンストラクタではなく、メソッドになる。  

- コンストラクタのオーバーライド  
    コンストラクタから別のコンストラクタを呼び出す場合は、`this`を使うが、必ず先頭行に記述する必要がある。

- interfaceにabstractをつけても良い。→もともとabstractなので意味はない。 
- interfaceはfinalにできない。 →常に継承されるため。

- サブクラスでスーパークラスと同じ名前のフィールドを定義しても良い。  
  ただしメソッドの様にオーバーライド（上書き）するわけではない。
- フィールドに対するアクセスは、インスタンスの型に依存する。

    ```java
    public class A {
        String x = "A";
    }
    public class B extends A {
        String x = "B";
    }
    public class Main {
        public static void main(String[] args) {
            A a = new B();
            B b = new B();
            String x1 = a.x;    // "A"
            String x2 = b.x;    // "B"
        }
    }
    ```
