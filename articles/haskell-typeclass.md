---
title: "[Haskell] 型クラスについて"
emoji: "💠"
type: "tech"
topics: ["haskell", "初心者", "memo", "学習メモ"]
published: true
---

モナドについて調べていたところ、**型シグネチャと型変数**と**型クラス**についても理解する必要があることがわかり、調べてみた。

## 💠 型シグネチャと型変数

### 🔹 型シグネチャ

変数や関数に対して、型が何であるかを示すもので、基本的には以下の形式で型を定義します。

```haskell
名前（変数名 or 関数名） :: 型
```

変数と関数のそれぞれの型シグネチャは以下のようになります。

#### 変数

```haskell
x :: Int
x = 5

name :: String
name = "type"
```

#### 　関数

```haskell
add :: Int -> Int -> Int
add x y = x + y

result = add 1 2 -- 結果: 3
```

### 🔹 型変数

型変数について、例えば以下のような、単純に 2 つの引数を受け取って、最初の値を返す関数があるとします。

```haskell
first :: a -> a -> a
first x y = x

first 1 2 -- 結果: 1
first "first" "second" -- 結果: "first"
```

この例の「a」が**型変数**にあたります。
「a」については、Int や、String、など任意の型を表すことができます。

また、以下の様に、一度に**複数の型変数**を使用できます。

```haskell
first :: a -> b -> a
first x y = x

first 1 "second" -- 結果: 1
first "first" 2 -- 結果: "first"
```

## 💠 型クラス

型クラスとは、他の言語の`interface`に似たもので、共通の関数を持つデータ型の集まりを定義するものです。（個人的には、Rust の trait に似ていると思っています）

### 🔹 既存の型クラス（Num）

例えば、定義済みの型クラスとして、`Num`という型クラスがあります。
`:info`コマンドで`Num`の定義を出力すると以下のように表示されます。

```
:info Num
type Num :: * -> Constraint
class Num a where
  (+) :: a -> a -> a
  (-) :: a -> a -> a
  (*) :: a -> a -> a
  negate :: a -> a
  abs :: a -> a
  signum :: a -> a
  fromInteger :: Integer -> a
  {-# MINIMAL (+), (*), abs, signum, fromInteger, (negate | (-)) #-}
        -- Defined in ‘GHC.Num’
-- ==== ここまでが、Num型クラスが持つべき関数の定義(A) ====
-- ==== これ以降は、Num型クラスに所属するデータ型(B) ====
instance Num Double -- Defined in ‘GHC.Float’
instance Num Float -- Defined in ‘GHC.Float’
instance Num Int -- Defined in ‘GHC.Num’
instance Num Integer -- Defined in ‘GHC.Num’
instance Num Word -- Defined in ‘GHC.Num’
```

まず(B)の部分について、こちらは`Num`型クラスに所属しているデータ型になります。`Double`、`Float`、`Int`、`Integer`、`Word`が、`Num`型クラスに所属していることになります。

次に(A)の部分について、これは`Num`型クラスに所属するために必要な関数の一覧です。`Double`や`Int`などのデータ型は、これらの関数を実際に実装することで`Num`型クラスのメンバーになることができます。
そのため、以下の様に「+」や「-」で計算することができます。

```haskell
a = 1.0
b = 3.2
c = a + b -- 結果:4.2
```

### 🔹 型クラスの定義方法

型クラスは、以下の様にすることで定義することができます。

```Haskell
-- 定義
class 名前 型変数 where
  関数名 :: 型シグネチャ
  関数名 :: 型シグネチャ

-- 例
class Shape a where
  height :: a -> Int
  width :: a -> Int
```

また、上記型クラスにデータ型を所属させる場合、以下の様にすることで所属させることができます。

```haskell
-- データ型
data Rectangle = Rectangle Int Int

-- 実装
instance Shape Rectangle where
  height (Rectangle h w) = h
  width (Rectangle h w) = w
```

### 🔹 型クラスの利点

型クラスの利点は、以下があります。

- 既存のデータ型に対して、後から制約をつけることができる
- データ型に関係なく、同じ関数名で処理できる

それぞれの例について、記載します。

#### 既存のデータ型に対して、後から制約をつけることができる

`MyNum`型を作成して、`Int`に対して、`add`関数の制約を追加します。

```haskell
class MyNum a where
  add:: a -> a -> a

instance MyNum Int where
  add x y = x + y

a:: Int
a = 100
b:: Int
b = 200

main = do
  putStrLn $ show $ add a b -- 300
```

#### データ型に関係なく、同じ関数名で処理できる

```haskell
-- 三角形（底辺, 高さ）
data Triangle = Triangle Int Int deriving Show

-- 四角形（幅、高さ）
data Square = Square Int Int deriving Show

-- 図形クラス
class Shape a where
  calculateArea:: a -> Int

-- 実装
instance Shape Triangle where
  calculateArea (Triangle base height) = (base * height) `div` 2

instance Shape Square where
  calculateArea (Square width height) = width * height


-- 使用例
triangle::Triangle
triangle = Triangle 2 4

square::Square
square = Square 2 4

main = do
  putStrLn $ show $ calculateArea triangle -- 結果: 4
  putStrLn $ show $ calculateArea square -- 結果: 8
```

## 参考

### 型シグネチャと型変数

https://haskell-tech.nkhn37.net/haskell-type-signatures-variables/
https://wiki.haskell.org/Type_signature

### 型クラス

http://walk.northcol.org/haskell/type-classes/
https://zenn.dev/masahiro_toba/articles/fc7ea6c2419e97
https://www.tohoho-web.com/ex/haskell.html
