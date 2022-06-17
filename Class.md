# インスタンス変数・インスタンスメソッド

![words](https://user-images.githubusercontent.com/85237728/174412755-01b5f7e6-16e1-42ca-9acf-c879a2d5bb56.png)

Tomyというインスタンスを作成。インスタンス変数をそれぞれ与える（ここではTomyと18）。

![tomy](https://user-images.githubusercontent.com/85237728/174412788-656ef188-447a-4fbc-8371-7395d6413ae7.png)

## インスタンス変数の参照

**インスタンス名.インスタンス変数**で参照する。

![ref](https://user-images.githubusercontent.com/85237728/174412821-1b8fc3b5-e2e8-4648-beef-5362400b41a1.png)

## インスタンスメソッドの利用

メソッドとは要は”関数”のこと。

それを踏まえ、インスタンスメソッドを使う方法は**「インスタンス名.インスタンスメソッド(引数)」**

今回は引数なしのインスタンスメソッドaboutme：

![method](https://user-images.githubusercontent.com/85237728/174412861-72093664-0302-4976-aba9-7ecd69853846.png)

インスタンスはいくらでも作成することができる。

Tomyに加え、Mikeというインスタンスを作る：

![mike](https://user-images.githubusercontent.com/85237728/174412890-d0120ee4-7aa9-44b4-8113-dacf2af13f16.png)

- インスタンスはクラスで定義された性質（インスタンス変数やメソッド）を持っている
- インスタンスは複数作れる
- 各インスタンスは、それぞれのインスタンス変数を持つ。すなわちそれぞれ個別のインスタンス変数の値を持つ（18と43のように違った値を持つ）

### selfについて

「self」は**インスタンス自身を表すもの**。

クラス内で定義される関数（＝メソッド）の第一引数にはselfを書く。

# クラス変数・クラスメソッド

MyFriendクラスにクラス変数とクラスメソッドを追加した：

![classmeth](https://user-images.githubusercontent.com/85237728/174413010-7a9a9939-1a17-415c-90e9-26c20d203959.png)

## クラス変数

クラス変数の参照は**「インスタンス名.クラス変数」**または**「クラス名.クラス変数」**。

![reftclass](https://user-images.githubusercontent.com/85237728/174413075-38f1a33e-73f5-48d8-acd4-0a609106b670.png)

- インスタンス変数 → 各インスタンスで独自の値
- クラス変数 →（そのクラスの）全てのインスタンスで共通

## クラスメソッド

`@classmethod`をメソッド定義の上に書くだけ。


**「クラス名.クラスメソッド(引数)」**で参照する。

![class_method_8](https://user-images.githubusercontent.com/85237728/174413141-ab821092-653a-4ca1-9e36-5bcee6e388e7.png)


# まとめ

![matome](https://user-images.githubusercontent.com/85237728/174413155-b7d18709-d2ca-471f-86b1-60707f986736.png)
