## Presented by dagezi

Quipper社 ~~営業~~ Android Engineer

<img src="ss.png">  スタディサプリ

<img width="80px" class=logo src="Qmark.png"> Quipper School

kotlin歴 7日



# Jack & Jill & Kotlin

もしくは「Kotlinはこの先生

きのこることができるか」

dagezi



## Jack & Jillとは?

- Android用 Javaコンパイラ群
- AndroidNでサポート強化
- AndroidNで機能強化



## APKができるまで

![old](javac_proguard_dex.png)

(from [saikoa](https://www.saikoa.com/blog/the_upcoming_jack_and_jill_compilers_in_android))



## Jackと Jillで

![new](jack_jill.png)

(from [saikoa](https://www.saikoa.com/blog/the_upcoming_jack_and_jill_compilers_in_android))


## Android Nで進化した

Lambdaが使えるように!!

しかも api level 7とかでも! やったぜ!!

あれ、でも Kotlin いらない子??


## lambdaとは何だったのか?

Java8にあったもの:

- 関数みたいなリテラルを簡単に書ける機能
- それを有効に使ったライブラリ群
- 効率的な lambdaの実装



## 関数みたいなリテラル

Java7:
```
  view.setOnClickListner(new View.OnClickListner() {
    @Override
    public void onClick(VIew view) {
      Log.d(TAG, "clicked");
    }
  });
```

Java8:
```
  view.setOnClickListner((v) -> Log.d(TAG, "clicked"));
```

Kotlin:
```
  view.setOnClickListner { Log.d(TAG, "clicked") }
```


## ライブラリ群

Android N でも Stream, Option とかはない
-  Java8のありがたみ半減

一方、Kotlinの collectionライブラリ




## 効率的な lambdaの実装

「~~非効率~~ Naiveな実装」とは?

- ラムダ式→ innerclass
- ロードに時間が掛かる
- 無駄にクラスを消費!
- retrolambdaは多分これ



## invokedynamic (indy) とは

- JRubyなどの動的言語のために Java 1.7で追加
- メソッド呼び出し法を実行時に決められる。
- リフレクションではないので JVMの最適化が効く!



## lambdaにおける indy

ラムダ式の変換を Runtime に追い出す
- ラムダ式の表現だけは固定
- 現状はメソッドをクラスを動的生成 & ロード
- 使われるかわからないものをロードしなくていい
- 将来的によりいい方法に移行できる

- [Translation of Lambda Expressions](http://cr.openjdk.java.net/~briangoetz/lambda/lambda-translation.html), Brian Goetz, 2012



## Jack & Jill では?

Java:
```Java
    public static void aho(List<String> ss) {
        Collections.sort(ss, (s0, s1) ->
            s0.length() - s1.length());
        Collections.sort(ss, (s0, s1) ->
            s0.length() + s1.length());
    }
```

dexdump:
 
```
Class descriptor  :
    'LStreamTest$-void_aho_java_util_List_ss_LambdaImpl0;'
Class descriptor  :
    'LStreamTest$-void_aho_java_util_List_ss_LambdaImpl1;'
```

ラムダ式ごとにクラスができてる!



## Kotlinは?

基本 Naiveな実装方法

- ただし、inline頑張る!

```Kotlin
fun ttt(ls: List<String>) : Boolean =
    ls.any {it.startsWith("aho")}
```

ここで `any` は:
```Kotlin
public inline fun <T> Array<out T>.all(predicate: (T) -> Boolean): Boolean {
    for (element in this) if (!predicate(element)) return false
    return true
}
```


## labmdaなくなる!

```
  public static final boolean ttt(java.util.List<java.lang.String>);
    Code:
       0: aload_0
       1: ldc           #9                  // String ls
       3: invokestatic  #15                 // Method kotlin/jvm/internal/Intrinsics.checkParameterIsNotNull:(Ljava/lang/Object;Ljava/lang/String;)V
       6: aload_0
       7: checkcast     #17                 // class java/lang/Iterable
      10: astore_1
      11: nop
      12: aload_1
      13: invokeinterface #21,  1           // InterfaceMethod java/lang/Iterable.iterator:()Ljava/util/Iterator;
      18: astore_2
      19: aload_2
      20: invokeinterface #27,  1           // InterfaceMethod java/util/Iterator.hasNext:()Z
      25: ifeq          60
      28: aload_2
      29: invokeinterface #31,  1           // InterfaceMethod java/util/Iterator.next:()Ljava/lang/Object;
      34: astore_3
      35: aload_3
      36: checkcast     #33                 // class java/lang/String
      39: astore        4
      41: aload         4
      43: ldc           #35                 // String aho
      45: iconst_0
      46: iconst_2
      47: invokestatic  #41                 // Method kotlin/text/StringsKt.startsWith$default:(Ljava/lang/String;Ljava/lang/String;ZI)Z
      50: ifeq          57
      53: iconst_1
      54: goto          61
      57: goto          19
      60: iconst_0
      61: ireturn
```


## Jack & Jillで失うもの

- JVMを使ったエコシステム
- Annotation Processing



## まとめ

- 関数リテラル: 引き分け
- ライブラリ: Kotlin 辛勝
- 効率性: Kotlin 勝ち 
- その他にもいっぱいいいところ

よって Kotlinは不滅



## まとめ2

とはいうものの、Jack & Jillも進化するかも?

