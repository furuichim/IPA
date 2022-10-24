<style>
strong {
    background: linear-gradient(transparent 0%, #6f6 0%) !important;
}

strong * {
    background: linear-gradient(transparent 0%, #6f6 0%) !important;
}

</style>
    
# Functional Interface

[`java.util.function`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/package-summary.html)パッケージで提供される、インスタンスメソッドが一つのインタフェース

## 重要な４つのインタフェース

|Interface|Method|Description|
|---|---|---|
|[`Function<T,R>`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/Function.html)|`R apply(T t)`|一つの引数を受け取り、結果を返却する関数|
|[`Predicate<T>`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/Predicate.html)|`boolean test(T t)`|一つの引数を受け取り、評価結果を返却する関数|
|[`Consumer<T>`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/Consumer.html)|`void accept(T t)`|1つの引数を受け取り、結果を返却しない関数|
|[`Supplier<T>`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/Supplier.html)|`T get()`|引数を受け取らず、値を返却する関数|


## ２つの引数をとるインタフェース
`BiXXXXX`のような名称のインタフェースになる。  

|Interface|Method|Description|
|---|---|---|
|[`BiFunction<T,U,R>`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/BiFunction.html)|`R apply(T t, U u)`|2つの引数を受け取って結果を生成する関数|
|[`BiPredicate<T,U>`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/BiPredicate.html)|`boolean test(T t, U u)`|2つの引数を受け取って評価する関数|
|[`BiConsumer<T,U>`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/BiConsumer.html)|`void accept(T t, U u)`|2つの入力引数を受け取って結果を返さないオペレーション|


## 引数と返却値の型が同じFunctionインタフェース
`XXXOperator`のような名称のインタフェースになる。  

|Interface|parent interface|Method|Description|
|---|---|---|---|
|[`UnaryOperator<T>`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/UnaryOperator.html)|`Function<T,T>`|`T apply(T a)`|１つの引数を受け取り、引数と同じ型を返却する関数|
|[`BinaryOperator<T>`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/BinaryOperator.html)|`BiFunction<T,T,T>`|`T apply(T a, T b)`|2つの同じ型の引数を受け取り、同じ型を返却する関数|


## 特定の型引数を適用したインタフェース

- 引数の型がXXXの場合
    XXXFunctionやXXXConsumerという名称の関数インタフェースになる。

- 返却値の型がXXXの場合
    ToXXXFunctionという名称の関数インタフェースになる。

- メソッドの名称
    **返却値の型**XXXに合わせて、getAsXXX、applyAsXXXという名称のメソッドに変わる。


|Interface|Method|Description|
|---|---|---|
|[`BooleanSupplier`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/BooleanSupplier.html)|`boolean getAsBoolean()`|booleanを返却する関数|


|Interface|Method|Description|
|---|---|---|
|[`IntFunction<R>`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/IntFunction.html)|`R apply(int x)`|int型引数を一つ受け取る関数|
|[`IntPredicate`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/IntPredicate.html)|`boolean test(int x)`|int型引数を一つ受け取り、評価結果を返却する関数|
|[`IntConsumer`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/IntConsumer.html)|`void accept(int x)`|int型引数を一つ受け取り、結果を返却しない関数|
|[`IntSupplier`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/IntSupplier.html)|`int getAsInt()`|int型を返却する関数|
|[`IntUnaryOperator`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/IntUnaryOperator.html)|`int applyAsInt(int)`|int型引数を1つ受け取り、int型を返却する関数|
|[`IntBinaryOperator`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/IntBinaryOperator.html)|`int applyAsInt(int, int)`|int型引数を2つ受け取り、int型を返却する関数|
|[`ToIntFunction<T>`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/ToIntFunction.html)|`int applyAsInt(T value)`|引数を一つ受け取り、int値の結果を返却する関数|

|Interface|Method|Description|
|---|---|---|
|[`LongFunction<R>`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/LongFunction.html)|`R apply(long x)`|long型引数を一つ受け取る関数|
|[`LongPredicate`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/LongPredicate.html)|`boolean test(long x)`|long型引数を一つ受け取り、評価結果を返却する関数|
|[`LongConsumer`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/LongConsumer.html)|`void accept(long x)`|long型引数を一つ受け取り、結果を返却しない関数|
|[`LongSupplier`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/LongSupplier.html)|`long getAsLong()`|long型を返却する関数|
|[`LongUnaryOperator`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/LongUnaryOperator.html)|`long applyAsLong(long)`|long型引数を1つ受け取り、long型を返却する関数|
|[`LongBinaryOperator`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/LongBinaryOperator.html)|`long applyAsLong(long, long)`|long型引数を2つ受け取り、long型を返却する関数|
|[`ToLongFunction<T>`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/ToLongFunction.html)|`long applyAsLong(T value)`|引数を一つ受け取り、long値の結果を返却する関数|

|Interface|Method|Description|
|---|---|---|
|[`DoubleFunction<R>`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/DoubleFunction.html)|`R apply(double x)`|double型引数を一つ受け取る関数|
|[`DoublePredicate`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/DoublePredicate.html)|`boolean test(double x)`|double型引数を一つ受け取り、評価結果を返却する関数|
|[`DoubleConsumer`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/DoubleConsumer.html)|`void accept(double x)`|double型引数を一つ受け取り、結果を返却しない関数|
|[`DoubleSupplier`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/DoubleSupplier.html)|`double getAsDouble()`|double型を返却する関数|
|[`DoubleUnaryOperator`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/DoubleUnaryOperator.html)|`double applyAsLong(double)`|double型引数を1つ受け取り、double型を返却する関数|
|[`DoubleBinaryOperator`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/DoubleBinaryOperator.html)|`double applyAsLong(double, double)`|double型引数を2つ受け取り、double型を返却する関数|
|[`ToDoubleFunction<T>`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/ToDoubleFunction.html)|`double applyAsDouble(T value)`|引数を一つ受け取り、double値の結果を返却する関数 |

## 引数と返却値の型を指定した`Function`インタフェース

|Interface|Method|Description|
|---|---|---|
|[`DoubleToIntFunction`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/DoubleToIntFunction.html)|`int applyAsInt(double)`|1つのdouble値引数を受け取ってint値の結果を生成する関数を表します。|
|[`DoubleToLongFunction`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/DoubleToLongFunction.html)|`long applyAsLong(double)`|1つのdouble値引数を受け取ってlong値の結果を生成する関数を表します。|
|[`IntToDoubleFunction`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/IntToDoubleFunction.html)|`double applyAsDouble(int)`|1つのint値引数を受け取ってdouble値の結果を生成する関数を表します。|
|[`IntToLongFunction`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/IntToLongFunction.html)|`long applyAsLong(int)`|1つのint値引数を受け取ってlong値の結果を生成する関数を表します。|
|[`LongToDoubleFunction`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/LongToDoubleFunction.html)|`double applyAsDouble(long)`|1つのlong値引数を受け取ってdouble値の結果を生成する関数を表します。|
|[`LongToIntFunction`](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/LongToIntFunction.html)|`int applyAsInt(long)`|1つのlong値引数を受け取ってint値の結果を生成する関数を表します。|

## Predicateインタフェースのstaticとdefaultメソッド

- `static <T> Predicate<T> isEqual(Object targetRef)`
    2つの引数が等しいかどうかをObjects.equals(Object, Object)に従ってテストする述語を返します。

- `static <T> Predicate<T> not(Predicate<? super T> target)`
    指定された述語の否定である述語を戻します。

- `default Predicate<T> and(Predicate<? super T> other)`
    この述語と別の述語の短絡論理積を表す合成述語を返します。

    ```java
    // 100より小さい
    Predicate<Integer> below100 = num -> num < 100;

    // 50より大きい
    Predicate<Integer> over50 = num -> num > 50;

    // 50より大きく100より小さい
    Predicate<Integer> checker = over50.and(below100);
    ```

- `default Predicate<T> negate()`
    この述語の論理否定を表す述語を返します。

- `default Predicate<T> or​(Predicate<? super T> other)`
    この述語と別の述語の短絡論理和を表す合成述語を返します。

    ```java
    // 100より小さい
    Predicate<Integer> below100 = num -> num < 100;

    // 50より大きい
    Predicate<Integer> over50 = num -> num > 50;

    // 50より大きい、または100より小さい
    Predicate<Integer> checker = over50.or(below100);
    ```

## Consumerインタフェースのstaticメソッド

- `default Consumer<T> andThen(Consumer<? super T> after)`
    このオペレーションを実行した後、続けてafterオペレーションを実行する合成Consumerを返します。

## Functionインタフェースのstaticと`default`メソッド

- `static <T> Function<T, T> identity()`
    常に入力引数を返す関数を返します。

- `default <V> Function<T, V> andThen(Function<? super R,​? extends V> after)`
    まず入力にこの関数を適用し、次に結果に関数afterを適用する合成関数を返します。

    ```java
    // Mr.を名前の前につけるメソッド
    Function<String, String> sir = name -> "Mr. " + name;

    // 名前の前にHelloをつけるメソッド
    Function<String, String> hello = name -> "Hello " + name;

    // Mr.をつけてからHelloをつける
    Function<String, String> hello_sir = sir.andThen(hello);
    ```

- `default <V> Function<V, R> compose(Function<? super V,​? extends T> before)`
    まず入力に関数beforeを適用し、次に結果にこの関数を適用する合成関数を返します。

    ```java
    // 3倍にする
    Function<Integer, Integer> tripple = num -> num * 3;

    // 100を足す
    Function<Integer, Integer> add100 = num -> num + 100;

    // add100をした後、Trripleを呼び出す
    Function<Integer, Integer> add100andTripple = tripple.compose(add100);
    ```


## 関数インタフェースとメソッド参照

以下のインタフェースとクラスが定義されている場合。

```java
public interface Gomi4 {
    int method(Integer x);
}
```

```java
public class Gomi4impl implements Gomi4 {
    @Override
    public int method(Integer x) {
        return x * 2;
    }

    public static int staticMethod(Integer x) {
        return x * 3;
    }
}
```

以下の様にメソッド参照を使用することができる。

```java
// Gomi4implインスタンスを作成する
Gomi4impl gomi4 = new Gomi4impl();

// Gomi4implはToIntFunction<Integer>ではないので、コンパイルエラーになる
// ToIntFunction<Integer> test = gomi4

// 匿名クラスは関数インタフェースに代入できる
ToIntFunction<Integer> test1 = new ToIntFunction<Integer>() {
    @Override
    public int applyAsInt(Integer x) {
        return x;
    }
};

// ラムダ型は関数インタフェースに代入できる
ToIntFunction<Integer> test2 = (x) -> 2 * x;

// スタティックメソッドの関数参照は関数インタフェースに代入できる
ToIntFunction<Integer> test3 = Gomi4impl::staticMethod;

// インスタンスメソッドの関数参照は関数インタフェースに代入できる
ToIntFunction<Integer> test4 = gomi4::method;

// 引数にスタティックメソッドの関数参照を指定する
Stream<Integer> st1 = Stream.of(1, 2, 3);
st1.mapToInt(Gomi4impl::staticMethod).forEach(System.out::println); // 3 6 9

// 引数にインスタンスメソッドの関数参照を指定する
Stream<Integer> st2 = Stream.of(1, 2, 3);
st2.mapToInt(gomi4::method).forEach(System.out::println);   // 2 4 6
```

[https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/package-summary.html](https://docs.oracle.com/javase/jp/11/docs/api/java.base/java/util/function/package-summary.html)

