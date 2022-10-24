# DateTime

## 日付事項関連のパッケージ

- [`java.time`](https://docs.oracle.com/javase/jp/8/docs/api/java/time/package-summary.html)  
    日付と時刻を表すコアAPIを提供する。

- [`java.time.chrono`](https://docs.oracle.com/javase/jp/8/docs/api/java/time/chrono/package-summary.html)  
    ISO-8601以外のカレンダーシステム(和暦など)を表すAPIを提供する。  
    独自のカレンダーシステムを定義することも可能。

- [`java.time.format`](https://docs.oracle.com/javase/jp/8/docs/api/java/time/format/package-summary.html)  
    日付と時刻のフォーマット及び解析用APIを提供する。

- [`java.time.temporal`](https://docs.oracle.com/javase/jp/8/docs/api/java/time/temporal/package-summary.html)  
    基本的な日時の単位(長さの概念)やフィールドに関するAPIを提供する。

- [`java.time.zone`](https://docs.oracle.com/javase/jp/8/docs/api/java/time/zone/package-summary.html)  
    タイムゾーンのサポートAPIを提供する。


## 日付時刻関連のクラスとインタフェース

![excption](java_se11_gold/datetime.svg)

- `Instant` - 時系列の時点を表すクラス
- `Period` - 日付単位での「間隔」を表すオブジェクトのクラス
- `Duration` - 時刻単位での「間隔」を表すオブジェクトのクラス

- `LocalDate` - 日付のみを表すクラス
- `LocalDateTime` - 日付と時刻を表すクラス
- `LocalTime` - 時刻を表すクラス

- `OffsetDateTime` - 時差情報ありの日付と時刻を表すクラス
- `OffsetTime` - 時差情報ありの時刻を表すクラス
- `ZonedDateTime` - 時差情報とタイムゾーン情報ありの日付と時刻を表すクラス


## java.time.DayOfWeek

曜日を表す列挙型

```java
enum DayOfWeek {
    FRIDAY,
    MONDAY,
    SATURDAY,
    SUNDAY,
    THURSDAY,
    TUESDAY,
    WEDNESDAY
};
```

## java.time.Month

月を表す列挙型

```java
enum Month {
    JANUARY,
    FEBRUARY,
    MARCH,
    APRIL,
    MAY,
    JUNE,
    JULY,
    AUGUST,
    SEPTEMBER,
    OCTOBER,
    NOVEMBER,
    DECEMBER
};
```

## java.time.Instantクラス

[`java.time.Instant`](https://docs.oracle.com/javase/jp/8/docs/api/java/time/Instant.html)
時系列上の単一の点を表すオブジェクトのクラスです。

```java
// システム・クロックから現在のインスタントを取得します
Instant n1 = Instant.now();

// Instantのインスタンスをエポック1970-01-01T00:00:00Zからのミリ秒数を使用して取得します
Instant n2 = Instant.ofEpochMilli(100000);

// Instantのインスタンスをエポック1970-01-01T00:00:00Zからの秒数を使用して取得します。
Instant n3 = Instant.ofEpochSecond(200);

// Instantのインスタンスをエポック1970-01-01T00:00:00Zからの秒数と秒のナノ秒部分を使用して取得します。
Instant n3 = Instant.ofEpochSecond(2000, 50000);
```

ゾーン情報もあるZonedDateTimeインスタンスから`toInstant`メソッドを使ってInstantを取得できます。
注意：ゾーン情報のないLocalDateTimeインスタンスの`toInstant`メソッドでは、ゾーン情報を指定する必要があります。

```java
Instant instant = ZonedDateTime.now().toInstant();
```


## java.time.Periodクラス

[java.time.Period](https://docs.oracle.com/javase/jp/8/docs/api/java/time/Period.html)

日付単位での「間隔」を表すオブジェクトのクラスです。

```java
// 2001-01-05から2025-12-31(含まない)までの期間を取得する
Period p1 = Period.between(LocalDate.of(2001, 1, 5), LocalDate.of(2025, 12, 31));

// 期間の日部分
int days = p1.getDays();

// 期間の月部分
int months = p1.getMonths();

// 期間の年部分
int years = p1.getYears();

System.out.println(years + "年" + months + "月" + days + "日間");
```



## java.time.Durationクラス

[`java.time.Duration`](https://docs.oracle.com/javase/jp/8/docs/api/java/time/Duration.html)

時刻単位での「間隔」を表すオブジェクトのクラスです。
時間ベースの時間(「34.5秒」など)。

```java
// 2001-01-05T10:00:00.0から2025-12-31T12:03:04.0までの間隔を取得する。
Duration d1 = Duration.between(LocalDateTime.of(2001, 1, 5, 10, 0, 0, 0), LocalDateTime.of(2025, 12, 31, 12, 3, 4, 0));

// 秒数を取得する。
long second1 = d1.getSeconds();

// ナノ秒数を取得する。
// メソッド名に s が付いていない点に注意！
long nano1 = d1.getNano();

System.out.println(second1);
```

`Duration.between`で指定することができる２つ`Temporal`を表すオブジェクトは、秒までを表している必要がある。
したがって、`LocalDate`を指定すると、ランタイムエラーになる。
また、`LocalDateTime`でも秒まで指定していないと、ランタイムエラーになる。

## java.time.ZoneIdクラス

[`java.time.ZoneId`](https://docs.oracle.com/javase/jp/8/docs/api/java/time/ZoneId.html)

```java
// システムデフォルトのゾーンIDを取得する
ZoneId here = ZoneId.systemDefault();

// ゾーンIDの文字列
// Asia/Tokyo
System.out.println(here.getId());

// Europe/LondonからゾーンIDを取得する
ZoneId there = ZoneId.of("Europe/London");
```

## `java.time.temporal.TemporalUnit`インタフェース

[java.time.temporal.TemporalUnit](https://docs.oracle.com/javase/jp/8/docs/api/java/time/temporal/TemporalUnit.html)

日付や時刻の単位を定義するためのインタフェース


## `java.time.temporal.ChronoUnit`列挙子

[`java.time.temporal.ChronoUnit`](https://docs.oracle.com/javase/jp/8/docs/api/java/time/temporal/ChronoUnit.html)

```java
enum ChronoUnit implements TemporalUnit {
    // 1世紀の概念を表す単位。
    CENTURIES,
    // 1日の概念を表す単位。
    DAYS,
    // 1デケイドの概念を表す単位。
    DECADES,
    // 1紀元の概念を表す単位。
    ERAS,
    // 永遠の概念を表す人為的単位。
    FOREVER,
    // 午前/午後で使用される半日の概念を表す単位。
    HALF_DAYS,
    // 1時間の概念を表す単位。
    HOURS,
    // 1マイクロ秒の概念を表す単位。
    MICROS,
    // 1ミレニアムの概念を表す単位。
    MILLENNIA, 
    // 1ミリ秒の概念を表す単位。
    MILLIS,
    // 1分の概念を表す単位。
    MINUTES,
    // 1か月の概念を表す単位。
    MONTHS,
    // 1ナノ秒の概念を表す単位。サポートされている最小の時間単位。
    NANOS,
    // 1秒の概念を表す単位。
    SECONDS,
    // 1週の概念を表す単位。
    WEEKS,
    // 1年の概念を表す単位。
    YEARS
}
```

## `java.time.LocalDate`クラス

[`java.time.LocalDate`](https://docs.oracle.com/javase/jp/8/docs/api/java/time/LocalDate.html)

ISO-8601暦体系のタイムゾーンのない日付、2007-12-03など
LocalDateにはコンストラクタはないため、`of`や`parse`でインスタンスを得る。

- 現在の日付を取得する  

    ```java
    LocalDate current = LocalDate.now();
    ```

- 指定した年月日でインスタンスを生成する

    ```java
    // 2000年1月1日のインスタンスを取得する
    LocalDate date1 = LocalDate.of(2000, 1, 1);
    ```

    ```java
    // 2020年4月1日のインスタンスを取得する
    LocalDate date1 = LocalDate.of(2020, Month.APRIL, 1);
    ```

- 日付文字列をパースする

    `parse`で第二引数が指定されない場合は、`ISO_LOCAL_DATE`フォーマッタが使用される。

    ```java
    // 1999年8月4日のインスタンスを取得する
    LocalDate date = LocalDate.parse("1999-08-04");
    ```

    第二引数でフォーマッタを指定することもできる。
    指定可能な定義済みフォーマッタは以下になる。

    + `ISO_ZONED_DATE_TIME` - 2011-12-03T10:15:30+01:00[Europe/Paris]
    + `ISO_LOCAL_DATE` - 2014-09-30

    ```java
    // 1999年8月4日のインスタンスを取得する
    LocalDate date = LocalDate.parse("1999-08-04", DateTimeFormatter.ISO_LOCAL_DATE);
    ```

    第二引数で独自のフォーマッタを指定することもできる。

    ```java
    // 1999年8月4日のインスタンスを取得する
    LocalDate date = LocalDate.parse("1999年08月04日", DateTimeFormatter.ofPattern("yyyy年MM月dd日"));
    ```

- 日付を比較する

    ```java
    // 2000-01-03は2021-01-05よりも前
    Assert LocalDate.of(2000, 1, 3).isBefore(LocalDate.of(2021, 1, 5))

    // 2000-01-03は1999-12-05よりも後
    Assert LocalDate.of(2000, 1, 3).isAfter(LocalDate.of(1999, 12, 5))

    // 2000-01-03は2000-01-03と同じ
    Assert LocalDate.of(2000, 1, 3).isEqual(LocalDate.parse("2000-01-03"))
    ```

- 加減算する

    `LocalDate`は`immutable`なクラスなので注意！

    ```java
    // 2020年4月1日のインスタンスを取得する
    LocalDate date1 = LocalDate.of(2020, Month.APRIL, 1);
    // 日を100日追加したインスタンスを得る。
    LocalDate date2 = date1.plus(100, ChronoUnit.DAYS);

    System.out.println(date2.toString());
    ```

## `java.time.LocalTime`クラス

[`java.time.LocalTime`](https://docs.oracle.com/javase/jp/8/docs/api/java/time/LocalTime.html)

ISO-8601暦体系における、タイムゾーンのない時間(10:15:30など)。
LocalTimeにはコンストラクタはないため、`of`や`parse`でインスタンスを得る。

- 現在の時刻を取得する  

    ```java
    LocalTime current = LocalTime.now();
    ```

- 指定した時分秒でインスタンスを生成する

    ```java
    // 15:28:07のインスタンスを取得する
    LocalTime time1 = LocalTime.of(15, 28, 7);
    ```

    ```java
    // 23:01:23.000000123のインスタンスを取得する
    // 最後に引数はナノ秒の指定になる。
    LocalTime time2 = LocalTime.of(23, 1, 23, 123);
    ```

- 時刻文字列をパースする

    ```java
    // 18:54:11.000001234のインスタンスを取得する
    LocalTime time3 = LocalTime.parse("18:54:11.1234");
    ```

- 時刻を比較する

    ```java
    // 12:03:01は12:04:01よりも前
    Assert LocalTime.of(12, 3, 1).isBefore(LocalTime.of(12, 4, 1))

    // 13:03:01は12:04:01よりも後
    Assert LocalTime.of(13, 3, 1).isAfter(LocalTime.of(12, 4, 1))

    // 13:03:01は13:03:01と同じ
    Assert LocalTime.of(13, 3, 1).isEqual(LocalTime.parse("13:03:01"))
    ```

- 加減算する

    `LocalTime`は`immutable`なクラスなので注意！

    ```java
    // 12:03:01のインスタンスを取得する。
    LocalTime time1 = LocalTime.of(12, 3, 1);
    // 秒を100秒減らしたインスタンスを得る。
    LocalTime time2 = time1.minus(100, ChronoUnit.MINUTES);
    // 10:23:01
    System.out.println(time2.toString());
    ```


## `java.time.LocalDateTime`クラス

[`java.time.LocalDateTime`](https://docs.oracle.com/javase/jp/8/docs/api/java/time/LocalDateTime.html)

ISO-8601暦体系のタイムゾーンのない日付/時間、2007-12-03T10:15:30など。
LocalDateTimeにはコンストラクタはないため、`of`や`parse`でインスタンスを得る。

- 現在の日付時刻でインスタンスを取得する  

    ```java
    LocalDateTime current = LocalDateTime.now();
    ```
 
- 年月日時分秒を指定してインスタンスを取得する  

    ```java
    // 2001年2月3日12時25分48秒
    LocalDateTime current = LocalDateTime.of(2001, 2, 3, 12, 25, 48);
    ```

    ```java
    // 2021年2月3日12時25分48秒
    LocalDateTime current = LocalDateTime.of(2001, Month.FEBRUARY, 3, 12, 25, 48);
    ```

- LocalDateとLocalTimeを指定してインスタンスを取得する  

    ```java
    // 2010-10-08
    LocalDate date = LocalDate.of(2010, 10, 8);
    // 13:04:25
    LocalTime time = LocalTime.of(13, 4, 25);
    // 2010-10-08T13:04:25
    LocalDateTime dt = LocalDateTime.of(date, time);
    ```

- 日付時刻文字列をパースする

    ```java
    // 2019年1月2日4時5分35秒のインスタンスを取得する
    LocalDateTime time3 = LocalDateTime.parse("2019-01-02T04:05:35");
    ```

## `java.time.format.DateTimeFormatter`クラス

[`java.time.format.DateTimeFormatter`](https://docs.oracle.com/javase/jp/8/docs/api/java/time/format/DateTimeFormatter.html)
覚えておくべきDateTimeFormatterクラスの定数

```java
// 基本的なISO日付
// --->> '20111203'
DateTimeFormatter.BASIC_ISO_DATE

// オフセット(時差)の情報を付加できるISO日付
// --->> '2011-12-03+01:00'; '2011-12-03'
DateTimeFormatter.ISO_DATE

// ISOローカル日付
// --->> '2011-12-03'
DateTimeFormatter.ISO_LOCAL_DATE

// オフセット(時差)の情報を付加できるISO時間
// --->> '10:15:30+01:00'
DateTimeFormatter.ISO_OFFSET_TIME
```

覚えておくべきDateTimeFormatterクラスのメソッド


```java
// ロケールの日付スタイルを持つフォーマッタ
// '2011-12-03'
DateTimeFormatter.ofLocalizedDate(dateStyle);

// ロケールの時間スタイルを持つフォーマッタ
// '10:15:30'
DateTimeFormatter.ofLocalizedTime(timeStyle);

// ロケールの日付と時間のスタイルを持つフォーマッタ
// '3 Jun 2008 11:05:30'
DateTimeFormatter.ofLocalizedDateTime(dateTimeStyle);

// ロケールの日付スタイルおよび時間スタイルを持つフォーマッタ
// '3 Jun 2008 11:05'
DateTimeFormatter.ofLocalizedDateTime(dateStyle,timeStyle);
```

`dateStyle`と`timeStyle`には`java.time.format.FormatStyle`列挙型を指定する。
`java.time.format.FormatStyle`は以下のような列挙型である。

```java
public enum FormatStyle {
    // もっとも多くの詳細を含むフルテキスト・スタイル。
    FULL,
    // 多くの詳細を含む長テキスト・スタイル。
    LONG,
    // いくらかの詳細を含む中テキスト・スタイル。
    MEDIUM,
    // 短テキスト・スタイル。通常は数値です。
    SHORT
}
```

独自の日付時刻のフォーマットパターンを作成する場合は`of`メソッドを使用する。

```java
DateTimeFormatter format = DateTimeFormatter.of("yyyy/MM/dd");
```

