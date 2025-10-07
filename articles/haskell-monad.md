---
title: "[Haskell]：モナドについて"
emoji: "📓"
type: "tech"
topics: ["haskell", "初心者", "memo", "学習メモ"]
published: false
---

## 📖 はじめに

haskell のモナドについて、自分の理解がかなり曖昧だったため、理解を深めるために調査してみました。

## 純粋関数型言語

モナドについて、理解を深めるために、まずは`純粋関数型言語`について知る必要があります。
Haskell は、`純粋関数型言語`であり、以下のような特徴があります。

- 全ての処理を純粋関数のみ（同じ入力に対して、同じ出力がされる関数）で記述する
- 副作用を持たない

ただ、どうしても純粋関数のみで、処理を記述することができない場合があります。
例えば、`ユーザーがキーボード入力した値を出力する`という処理については、ユーザーが入力する値によって、結果が変わるため、純粋関数で表現することができません。

このように副作用を伴う計算を行うために、Hskell ではモナドを使用します。

## Functor

モナドを理解するのに、まずはモナドに関わりのある Functor(functor)という型クラスについても理解する必要があります。  
Functor は、簡単に説明すると「箱に入っている値に対して関数を適用して別の値にする処理を提供する」型クラスになります。  
ここでいう「箱」とは、`Maybe`、`[]`（リスト）、`Either e` のような、型引数を「1 つ」受け取る型コンストラクタのことを指します。  
「箱に入った値」とは、`Just 5`、`[1,2,3]`、`Right "hello"` のような、型コンストラクタ（Maybe や []）によって定義された型の実際の値のことを指します。

### 定義

Functor は以下のように定義されています。

```haskell
class Functor f where
    fmap :: (a -> b) -> f a -> f b
```

この型クラスの定義は、以下になります。

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
  --   ^^^^^^^^      ^^^^^         ^^^^^^^^ ^^^^^
  --   関数(a->b)    中身のa        関数適用  結果はb

box10 :: Box Int
box10 = Box 10

main :: IO ()
main = do
    print (fmap convertIntToString box10 ) -- Box "Number: 10"

```

## Funtor 則

TODO: 次ここかく

- https://wiki.haskell.org/index.php?title=Functor
- https://qiita.com/airtoxin/items/47327e9f8f5fa8d92e2d

### 法則１ 恒等射の保存を満たす

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

### 法則２

## Applicative

次に、重要になるのが、Applicative です。
Applicative は、Functor とモナドの間の処理を行う型クラスで、Functor のサブクラスに当たります。

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

```haskell
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

```haskell
instance Applicative Box where
  pure = Box
  Box f <*> Box a = Box (f a)

box10 :: Box Int
box10 = Box 10


boxDouble :: Box (Int -> Int)
boxDouble = Box (* 2)

someFunc :: IO ()
someFunc = do
    print (boxDouble <*> box10) -- Box 20
```

## モナドとは

## 参考

### Functor

- https://www.nct9.ne.jp/m_hiroi/func/haskell14.html
- https://wiki.haskell.org/index.php?title=Functor
- https://qiita.com/suin/items/0255f0637921dcdfe83b

- https://www.infoq.com/jp/articles/Understanding-Monads-guide-for-perplexed/
- https://qiita.com/kerupani129/items/333155e5e2dee644d6dc
- http://walk.northcol.org/haskell/monads/
- https://www.tohoho-web.com/ex/haskell.html
- https://qiita.com/airtoxin/items/47327e9f8f5fa8d92e2d

### アプリケイティブ

- https://www.nct9.ne.jp/m_hiroi/func/haskell14b.html
- https://scrapbox.io/haskell-shoen/Applicative
- https://haskell.jp/blog/posts/2019/regex-applicative.html
- https://qiita.com/masaki_shoji/items/930434432fc3764685ba

### モナド

- https://www.infoq.com/jp/articles/Understanding-Monads-guide-for-perplexed/

- http://walk.northcol.org/haskell/overview/
