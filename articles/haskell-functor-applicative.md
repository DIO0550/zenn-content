---
title: "[Haskell]：FunctorとApplicative"
emoji: "📓"
type: "tech"
topics: ["haskell", "初心者", "memo", "学習メモ"]
published: false
---

## はじめに

Haskell の**モナド**を理解するために、まず**Functor**と**Applicative**という型クラスを知る必要があったため、自分なりに調べてみました。

## Functor

Functor は、簡単に説明すると「箱に入っている値に対して関数を適用して別の値にする処理を提供する」型クラスになります。  
ここでいう「箱」とは、`Maybe`、`[]`（リスト型コンストラクタ）のような、型引数を「1 つ」受け取る型コンストラクタのことを指します。
「箱に入った値」とは、`Just 5`、`[1,2,3]`、`Right "hello"` のような、型コンストラクタ（Maybe や []）によって定義された型の実際の値のことを指します。

### 定義

Functor は以下のように定義されています。

```haskell
class Functor f where
    fmap :: (a -> b) -> f a -> f b
```

この型クラスの定義の意味は、以下になります。

- 型クラス Functor は、型変数 f を受け取る
- `fmap`関数が「a を引数にして、b を返す」関数を受け取り、「コンテナ f に包まれた a」を「コンテナ f に包まれた b」に変換する

### 例

以下の例は、型引数を 1 つ受け取る型`Box`を定義し、それを Functor 型クラスのインスタンスにして、`fmap`関数を実行してみたものになります。

```haskell
convertIntToString :: Int -> String
convertIntToString x = "Number: " ++ show x

data Box a = Box a deriving Show
instance Functor Box where
  fmap function (Box value) = Box (function value)
  -- function: 関数(a->b)
  -- value: 中身のa
  -- Box (function value): 関数適用、結果はb

box10 :: Box Int
box10 = Box 10

main :: IO ()
main = do
    print (fmap convertIntToString box10 ) -- Box "Number: 10"

```

## Functor 則

Functor のインスタンスは、満たさなければいけないルールが２つあります。

### 法則１ 恒等関数の保存

受け取った値をそのまま返す関数`id`があるとします。

```haskell
id x = x
```

この時、`fmap id`を適用しても、何も変化しないことを保証する必要があります。

```haskell
fmap id = id
```

#### 具体例

```haskell
data Box a = Box a deriving Show

instance Functor Box where
  fmap function (Box value) = Box (function value)

box10 :: Box Int
box10 = Box 10

main :: IO ()
main = do
    print (fmap id box10) -- Box 10
    print (id box10)      -- Box 10
```

### 法則２ 合成の保存

２つの関数`f`と`g`があった場合に、以下の２つが同じ結果になることを保証する

- 「合成してから`fmap`適用する」
- 「個別に`fmap`適用してから合成する」

```haskell
fmap (f . g)  ==  fmap f . fmap g
```

#### 具体例

```haskell
data Box a = Box a deriving Show

instance Functor Box where
  fmap function (Box value) = Box (function value)

box10 :: Box Int
box10 = Box 10

addOne :: Int -> Int
addOne x = x + 1

double :: Int -> Int
double x = x * 2

main :: IO ()
main = do
    -- 合成してからfmap
    print (fmap (double . addOne) box10)  -- Box 22

    -- 個別にfmapしてから合成
    print ((fmap double . fmap addOne) box10)  -- Box 22

```

## Applicative

Applicative は、Functor を拡張した型クラスで、Functor のサブクラスに当たります。
Functor と Monad の間に位置する型クラスで、Applicative は「箱に入った関数を、箱に入った値に適用する」型クラスになります。

### 定義

上記で書いた通り、Applicative は、Functor のサブクラスであり、型変数 f は、Functor である必要があります
実際の定義の一部は以下のようになります。

```haskell
class Functor f => Applicative f where
  pure :: a -> f a
  (<*>) :: f (a -> b) -> f a -> f b
```

#### pure :: a -> f a

pure 関数は、単純に a の値を f で包む関数になります。

##### 例

以下の例では、`pure`を使って`20`という値を`Box`で包む処理になります。
`pure 20 :: Box Int`と型注釈を付けることで、どの型で包むかを指定しています。

```haskell
data Box a = Box a deriving Show

instance Functor Box where
  fmap function (Box value) = Box (function value)

instance Applicative Box where
  pure = Box
  Box f <*> Box a = Box (f a)

main :: IO ()
main = do
    print (pure 20 :: Box Int) -- Box 20
```

#### (<\*>) :: f (a -> b) -> f a -> f b

型変数 f の型コンストラクタに包まれた関数(a → b)を、同じ f に包まれた値 a に適用して、f に包まれた結果 b を返す演算子です。

##### 例

以下の例では、`Box`に包まれた関数`boxDouble`（Int の値を 2 倍にする関数）を、`Box`に包まれた値`box10`(10 を`Box`で包んだもの)に適用しています。
`<*>`演算子により、両方の`Box`から中身を取り出し、関数を値に適用してから、再び`Box`で包んだ結果を返しています。

```haskell

data Box a = Box a deriving Show

instance Functor Box where
  fmap function (Box value) = Box (function value)

instance Applicative Box where
  pure = Box
  Box f <*> Box a = Box (f a)

box10 :: Box Int
box10 = Box 10

boxDouble :: Box (Int -> Int)
boxDouble = Box (* 2)

main :: IO ()
main = do
    print (boxDouble <*> box10) -- Box 20
```

## 参考

### Functor

- https://www.nct9.ne.jp/m_hiroi/func/haskell14.html
- https://wiki.haskell.org/index.php?title=Functor
- https://qiita.com/suin/items/0255f0637921dcdfe83b

### Applicative

- https://www.nct9.ne.jp/m_hiroi/func/haskell14b.html
- https://scrapbox.io/haskell-shoen/Applicative
- https://haskell.jp/blog/posts/2019/regex-applicative.html
- https://qiita.com/masaki_shoji/items/930434432fc3764685ba
