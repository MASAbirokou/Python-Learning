`with`に対応したオブジェクトを**コンテキストマネージャー**という。

ある処理の前後の処理をまとめて再利用可能にしてくれる。

コンテキストマネージャーの実体は特殊メソッド`__enter__()`と`__exit__()`を実装したクラスのインスタンスである。

# コンテキストマネージャーの構文と実装

with分で`with コンテキストマネージャー名:`と書く。

with文の前後に呼ばれる特殊メソッド`__enter__()`と`__exit__()`について：

- `__enter__()`：withブロックに入る際に呼ばれる処理が記述される
- `__exit__()`：withブロックを抜ける際に呼ばれる処理が記述される

```python
class ContextManager:
    #前処理（withブロックに入る際に呼ばれる）
    def __enter__(self):
        print('you entered the with block (__enter__ was called)')

    #後処理（withブロックを抜ける際に呼ばれる）
    def __exit__(self, exc_type, exc_value, traceback):
        print('__exit__() was called')
        print(f'{exc_type=}')
        print(f'{exc_value=}')
        print(f'{traceback=}')

#withブロックが正常終了の場合は、__exit__の引数は全てNone
with ContextManager():
    print('inside the block')
    
# 出力    
# you entered the with block (__enter__ was called)
# inside the block
# __exit__() was called
# exc_type=None
# exc_value=None
# traceback=None    
```    

## with文と例外処理

withブロック内で発生した例外は、特殊メソッド`__exit__()`の引数でその情報を受け取れる。

それゆえ`__exit__()`内では`raise`は不要。

上のコンテキストマネージャーを使って、with文内で例外を発生させてみる：

```#withブロック内で発生した例外の情報は__exit__()に渡される
with ContextManager():
    1 / 0

#出力
you entered the with block (__enter__ was called)
__exit__() was called
exc_type=<class 'ZeroDivisionError'>
exc_value=ZeroDivisionError('division by zero')
traceback=
-------------------------------------------------------------------
ZeroDivisionError                         Traceback (most recent call last)
 in 
      1 #withブロック内で発生した例外の情報は__exit__()に渡される
      2 with ContextManager():
----> 3     1 / 0

ZeroDivisionError: division by zero
```

## asキーワードで__enter__()の戻り値を利用する

コンテキストマネージャーからwithブロック内に渡したい値がある場合、その値を特殊メソッド`__enter__()`の戻り値にすると`as`キーワードで受け取れる。

```python
class ContextManager_2:    
    def __enter__(self):
        return 1       #__enter__()の戻り値がasキーワードに渡される
    
    def __exit__(self, exc_type, exc_value, traceback):
        pass

with ContextManager_2() as f:
    print(f)

#出力
# 1
```

## withブロック内から任意の値を特殊メソッド__exit__()に直接渡したい場合

インスタンス変数を介することで、withブロックから値を特殊メソッド`__exit__()`に渡す。

```python
class Point:
    def __init__(self, **kwargs):
        self.value = kwargs
        
    #withブロックに入る際に呼ばれる（前処理）
    def __enter__(self):  
        print('__enter__ was called')
        return self.value  #__enter__()の戻り値がasキーワードに渡される
    
    #withブロックから抜ける際に呼ばれる（後処理）
    def __exit__(self, exc_type, exc_value, traceback):
        print('__exit__ was called')
        print(self.value)

with Point(x = 1, y = 2) as p:
    print(p)
    p['z'] = 3
#出力
# __enter__ was called
# {'x': 1, 'y': 2}
# __exit__ was called
# {'x': 1, 'y': 2, 'z': 3}    
```    
