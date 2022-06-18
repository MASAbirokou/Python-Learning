デスクリプタの例としてpropertyやメソッドがある。

特殊メソッド`__get__()`、`__set__()`、`__delete__()`のいずれか１つでも持っていればそれはデスクリプタである。

- データデスクリプタ → `__set__()`、`__delete__()`のいずれかまたは両方を持つもの
- 非データデスクリプタ → `__get__()`しか持たないもの

# デスクリプタを使ってみる

デスクリプタのインスタンスをクラス変数として利用すると、そのクラス変数をインスタンス変数のように扱える。

属性の取得や代入、削除時にはデスクリプタが実装している`__get__()`や`__set__()`、`__delete__()`の対応するメソッドが呼ばれる。

## `_set_()`を実装する（データデスクリプタ）

属性代入時の処理をオーバーライドするデスクリプタTextField：

```python
class TextField:
    def __set_name__(self, owner, name):
        print('~~~__set_name__ was called~~~')
        print(f'{owner=}, {name=}')
        self.name = name
        
    def __set__(self, instance, value):
        print('~~~__set__ was called~~~')
        if not isinstance(value, str):
            raise AttributeError('must be str!')
        instance.__dict__[self.name] = value
        
    def __get__(self, instance, owner):
        print('~~~__get__ was called ~~~')
        return instance.__dict__[self.name]  #下の例でbookがinstance、nameがtitle
```        

このデスクリプタを利用するBookという名のクラスを次のように定義する：

```python
class Book:  # デスクリプタのインスタンスをクラス変数として利用
    title = TextField()

#出力
# ~~~__set_name__ was called~~~
# owner=<class '__main__.Book'>, name='title'
```

デスクリプタを利用するクラス（つまりBook）では、デスクリプタのインスタンスをクラス変数として利用する。

特殊メソッド`__set_name__()`には、そのデスクリプタを利用するクラスオブジェクト（ここではBook）と、デスクリプタに割り当てられた変数名が渡される。

Bookクラスを利用するときは、クラス変数`title`をインスタンス変数のように使える：

```
book = Book()
book.title = 'python practive book'
-------------------------------------------------
#出力
~~~__set__ was called~~~
-------------------------------------------------
book.title
-------------------------------------------------
#出力
~~~__get__ was called ~~~
'python practive book'
-------------------------------------------------
notebook = Book()
notebook.title = 'Notebook'
-------------------------------------------------
#出力
~~~__set__ was called~~~
-------------------------------------------------
notebook.title
-------------------------------------------------
#出力
~~~__get__ was called ~~~
'Notebook'
-------------------------------------------------
book.title = 3
-------------------------------------------------
#出力
-------------------------------------------------
~~~__set__ was called~~~
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
 in 
----> 1 book.title = 3

 in __set__(self, instance, value)
      8         print('~~~__set__ was called~~~')
      9         if not isinstance(value, str):
---> 10             raise AttributeError('must be str!')
     11         instance.__dict__[self.name] = value
     12 

AttributeError: must be str!
```

## `__get__()`のみを実装する（非データデスクリプタ）

**非データデスクリプタはインスタンス変数よりも優先度が低い**ということに注意する。

```python
class TextField:
    def __init__(self, value):
        if not isinstance(value, str):
            raise AttriubuteError('must be str!')
        self.value = value
        
    def __set_name__(self, owner, name):
        print('~~~__set_name__ was called~~~')
        print(f'{owner=}, {name=}')
        self.name = name
        
    def __get__(self, instance, owner):
        print('~~~__get__ was called~~~')
        return self.value

class Book:
    title = TextField('Python practice book')

#出力
# ~~~__set_name__ was called~~~
# owner=<class '__main__.Book'>, name='title'

book = Book()
book.title

#出力
#~~~__get__ was called~~~
# 'Python practice book'

book.title = 'new setting'  #__get__()のみにもかかわらず代入するとインスタンス変数になる
book.title  #インスタンス変数があると__get__()は呼ばれない

#出力
# 'new setting'
```
