---
title: "[Haskell]：代数的データ型"
emoji: "📓"
type: "tech"
topics: ["haskell", "初心者", "memo", "学習メモ"]
published: false
---

## はじめに

モナドを理解するための、ファンクタを理解するために、代数的データ型や型コンストラクタを知る必要があるため、調査した。

## 代数的データ型とは

以下の２つの型を組み合わせたものである。

- 複数の型を合わせて、新しく作成した型（直積型）
- 複数の型を合わせて、いずれか１つの型を持つ型（直和型）

## 代数的データ型の定義

代数的データ型の基本的な定義は、以下になる。

```haskell
data 型コンストラクタ（型名） 型変数 = 値コンストラクタ
```

## 型コンストラクタと値コンストラクタ

### 型コンストラクタ

型コンストラクタは、型を受け取り、新しい型を返す。
型コンストラクタは、0 個以上の引数を受け取ることができる。

#### 例

```haskell
-- 引数なし（具体型）
data Bool = False | True
--   ^^^^ 型コンストラクタ

data Color = Red | Green | Blue
--   ^^^^^ 型コンストラクタ

-- 引数1つ
data Maybe a = Nothing | Just a
--   ^^^^^ 型コンストラクタ（1引数）

data Box a = Box a
--   ^^^ 型コンストラクタ（1引数）

-- 引数2つ
data Either a b = Left a | Right b
--   ^^^^^^ 型コンストラクタ（2引数）

data Pair a b = Pair a b
--   ^^^^ 型コンストラクタ（2引数）

data First a b = First a
--   ^^^^^ 型コンストラクタ（2引数）
```

### 値コンストラクタ（データコンストラクタ）

値コンストラクタは、値を受け取り、新しい値を返す。
値コンストラクタは、0 個以上の引数を受け取ることができる。
また、値をグループ化して、代数的データ型にタグを付ける。
引数を受け取らないコンストラクタは、定数や空のコンストラクタと呼ばれる？

#### 例

```haskell
-- 引数：なし
-- Red、Blue、Greenが値コンストラクタに該当
data Color = Red | Blue | Green
--   ^^^^^ 型コンストラクタ
--           ^^^ ^^^^ ^^^^^ 値コンストラクタ（引数なし）

-- 引数：あり
-- 以下が値コンストラクタ
-- Circle Int
-- Rect Int Int
-- Triangle Int Int
data Shape = Circle Int | Square Int | Triangle Int Int
--   ^^^^^ 型コンストラクタ
--           ^^^^^^ 値コンストラクタ（Int -> Shape）
--                        ^^^^^^ 値コンストラクタ（Int -> Shape）
--                                      ^^^^^^^^ 値コンストラクタ（Int -> Int -> Shape）
```

#### 関数として扱う

値コンストラクタは、関数として扱うことができる。

```haskell
data Shape = Circle Int | Square Int | Triangle Int Int deriving Show

sides:: [Int]
sides = [2,4,6]
squares :: [Shape]
squares = map Square sides

main = do
    print squares -- [Square 2,Square 4,Square 6]

```

### 直積型

#### 例

```haskell
data Point = Point Int Int
```

### 直和型

#### 例

```haskell
data DayOfWeek = Mon | Tue | Wed | Thu | Fri | Sat | Sun
```

### 直積 + 直和

```haskell
data Shape = Rectangle Int Int
```

## カインド（種）

カインドとは、大雑把にいうと「**型の分類**」です。  
haskell のカインドには、以下の 2 つの規則がある。

- 1. （A）`*`は、全ての**データ型**（値を持つことのできる）のカインドである。
- 2. （B）`k1 -> k2` は、k1 のカインドを持つ型を受け取り、k2 のカインドの型を生成する。

:::message
GHC 8 以降は、以下の理由から、カインドを表す際に、`*`ではなく`Type`を使用することを推奨しています。

主な理由

- 型レベルでの乗算の演算子`*`と競合する
- 項と型を統一する際に支障がある
- 可読性の問題(`*`は、ワイルドカードや Markdown の箇条書きなどで使用されているため)など

参考
[GHC Proposal #143](https://ghc-proposals.readthedocs.io/en/latest/proposals/0143-remove-star-kind.html)
:::

kind は`:k`または`:kind`を使用することで確認できる。

### 具体例

#### (1) 型を受け取らない型（具体型）

`Int`や`String`など、型引数がない、値を持てる**具体型**は、カインドが `*` になる。

```haskell
ghci> :k Int
Int :: *

ghci> :k String
String :: *
```

#### (2)型引数を受け取る型

`Maybe`や`[]`など、型引数を受け取る型は、`* -> *`のようなカインドになる。

```haskell
ghci> :k Maybe
Maybe :: * -> * -- 1つの型を受け取る

ghci> :k []
[] :: * -> * -- 1つの型を受け取る

ghci> :k Either
Either :: * -> * -> *  -- 2つの型を受け取る
```

https://ja.wikipedia.org/wiki/%E3%82%AB%E3%82%A4%E3%83%B3%E3%83%89_(%E5%9E%8B%E7%90%86%E8%AB%96)

## 参考

https://wiki.haskell.org/Constructor#Type_constructor
https://haskell.jp/blog/posts/2020/how-to-use-type-newtype-data.html
http://walk.northcol.org/haskell/adts/
https://qiita.com/7shi/items/1ce76bde464b4a55c143
https://qiita.com/tobita_yoshiki/items/a2ffd2cefa53376338c6
https://zenn.dev/mod_poppo/books/haskell-type-level-programming/viewer/types-and-kinds
https://ja.wikipedia.org/wiki/%E3%82%AB%E3%82%A4%E3%83%B3%E3%83%89_(%E5%9E%8B%E7%90%86%E8%AB%96)

https://ghc.gitlab.haskell.org/ghc/doc/users_guide/exts/poly_kinds.html#overview-of-type-in-type
https://zenn.dev/mod_poppo/books/haskell-type-level-programming/viewer/types-and-kinds
https://ghc.gitlab.haskell.org/ghc/doc/users_guide/exts/poly_kinds.html#the-kind-type
