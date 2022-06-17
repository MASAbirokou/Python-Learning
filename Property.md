# プロパティとは？

簡単に言うと **「インスタンスメソッド」を「インスタンス変数」のように扱える機能** のこと。

以下の要望にこたえる：
- 他のインスタンス変数から求められる値を使いたい！
- インスタンス変数に代入される値をチェックしたい！

# プロパティの例

```python
class Store:
    def __init__(self, raw_price):
        self.raw_price = raw_price
        self._discounts = 0  #値引き値の初期設定
        
    @property
    def discounts(self):
        return self._discounts
    
    @discounts.setter
    def discounts(self, value):
        if value < 0 or 100 < value:
            raise ValueError('0 <= value <= 100')
        
        self._discounts = value
        
    @property
    def price(self):
        return int(self.raw_price * (100 - self._discounts)/100)
```

まずはインスタンス「pen」を作る：

```python
pen = Store(90)  #penの値段は90
```

まだ`discounts`を設定していないので初期値の0が出力される：

```python
pen.discounts  #discountsというインスタンス変数を参照
# 0
```

まだ割り引きしていないので値段はそのまま90：

```python
pen.price
# 90
```

ここで注目すべき点は、**`price`はメソッドなのにもかかわらず、`price()`と括弧を付けなくても参照できている**ということ。

これがプロパティの特徴。**メソッドがまるでインスタンス変数のよう**に簡単に参照できる。

ここで値引き値（`discounts`）を設定してみる：

```python
pen.discounts = 50
```

その後、再びメソッド`price`の結果を見てみると

```python
pen.price  #プロパティゆえ、「price()」と括弧を付けずに呼び出せる
# 45
```

割引が反映された値段になっている。

`discounts`というインスタンスメソッドに関しても、`@property`をつけて定義したから、 インスタンス変数のようにアクセスできる。

（`discounts()`と括弧を付ける必要もありません。）

# @discounts.setterについて

`pen.discounts = 50`のように値を代入するときに呼ばれる。

この`@discounts.setter`を用意することで、不正なdiscountsの代入をはじいてくれ流。

ちなみに、インスタンスメソッド`discounts`に対する`setter`だから、**@discounts.setter**という名前。

# まとめ

上のサンプルコードについて、プロパティのおかげで

- インスタンス変数のように各メソッド`price`と`discounts`にアクセスできる
- 値引率に不正な値が設定できないようになっている
