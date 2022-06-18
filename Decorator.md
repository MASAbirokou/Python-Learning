**デコレータ**とは、関数やクラスの前後に（それら自体の定義は変更せず）処理を追加できる機能のこと。

`@〜`を付けるだけでできる。

デコレータで実現出来ること
- 関数の引数チェック
- 関数呼び出し結果のキャッシュ
- 関数の実行時間の計測

# デコレータの例

## functools.lru_cached()

`functools.lru_cached()`は、**関数の結果をキャッシュしてくれるデコレータ**。

同じ引数での呼び出し結果が既にキャッシュされている場合は、関数を実行することなくキャッシュ済みの結果を返します。

（キャッシュの際、関数の引数と結果の対応付けに辞書が使われるため、`@lru_cached()`を付けた関数の引数は、辞書のキーに使えるイミュータブルなオブジェクトである必要がある）。

> イミュータブル：固定の値を持ったオブジェクト。（例）数値、文字列、タプル。イミュータブルなオブジェクトは値を変えることができない。

```python
from functools import lru_cache
from time import sleep

@lru_cache(maxsize = 32)  #最近の呼び出し最大32回文までキャッシュ
def plus_one(n):
    sleep(3)
    return n + 1
```    

```
-------------------------------------
plus_one(99)  #しっかり３秒スリープしてから出力する
-------------------------------------
#出力
100
-------------------------------------
plus_one(99)  #一瞬で結果が出力される
-------------------------------------
#出力
100
```

## 関数の呼び出し時にログを出力するデコレータ

関数デコレータの実体は、**引数に関数を１つ受け取る呼び出し可能オブジェクト**である。

つまりプログラムの実行中にデコレータが戻り値として返した、新しい関数が元の関数名に紐付けられている。

```python
def deco(f):
    print('deco called')
    def wrapper():
        print('before exec')
        v = f()  #元の関数を実行
        print('after exec')
        return v  #元の関数の結果を返す
    return wrapper

@deco
def func():
    print('exec')
    return 1
```

`func()`関数を定義した瞬間に「deco called」と出力される。

さらに見ると：

```
func.__name__ 
------------------------------------
#出力
'wrapper' #新しい関数wrapperが元の関数名に紐付けられている

------------------------------------
func()  #func()の呼び出しはwrapper()の呼び出しになる
------------------------------------
#出力
before exec
exec
after exec
1
```

上のデコレータ`deco`は、**デコレート対象の関数が引数をとるもの**だとエラーになってしまう。

それを改善する。

**プログラムの実行中に実際に呼び出される関数はwrapper()だから**、`wrapper()`関数に、元の関数が受け取った引数を渡してやればいい！

```python
def deco_new(f):
    print('deco_new called')
    
    def wrapper(*args, **kwargs):
        print('before exec')
        v = f(*args, **kwargs)  #引数をきちんと渡して元の関数を実行
        print('after exec')
        return v  #しっかりと関数の戻り値を返す
    return wrapper
------------------------------------
@deco_new
def x_and_y(x, y):
    print('exec')
    return x, y
```

関数`x_and_y()`が定義された直後に「deco_new called」が出力される。

```x_and_y.__name__
------------------------------------
#出力
'wrapper'
------------------------------------
x_and_y(1, 2)
------------------------------------
#出力
before exec
exec
after exec
(1, 2)
```

## 関数の処理時間を計測するデコレータ

このデコレータの名前を`elapsed_time()`とする。

`@elapsed_time`と、関数の直前に付けるだけでその関数の処理時間を計測できる。


```python
import time
from functools import wraps  #便宜上使うデコレータ（説明は省略）

def elapsed_time(f):
    @wraps(f)
    def wrapper(*args, **kwargs):
        start = time.time()
        v = f(*args, **kwargs)
        print(f"{f.__name__}: {time.time() - start}")
        return v
    return wrapper
------------------------------------
@elapsed_time
def func(n):
    return sum(i for i in range(n))
```    

```
func(100)
------------------------------------
#出力
func: 1.0013580322265625e-05
4950
------------------------------------
func(101)
------------------------------------
#出力
func: 1.0013580322265625e-05
5050
```





