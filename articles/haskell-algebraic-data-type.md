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
Maybe :: * -> *

ghci> :k []
[] :: * -> *
```

https://ja.wikipedia.org/wiki/%E3%82%AB%E3%82%A4%E3%83%B3%E3%83%89_(%E5%9E%8B%E7%90%86%E8%AB%96)

## 参考

https://haskell.jp/blog/posts/2020/how-to-use-type-newtype-data.html
http://walk.northcol.org/haskell/adts/
https://qiita.com/7shi/items/1ce76bde464b4a55c143
https://qiita.com/tobita_yoshiki/items/a2ffd2cefa53376338c6
https://zenn.dev/mod_poppo/books/haskell-type-level-programming/viewer/types-and-kinds
https://ja.wikipedia.org/wiki/%E3%82%AB%E3%82%A4%E3%83%B3%E3%83%89_(%E5%9E%8B%E7%90%86%E8%AB%96)

https://ghc.gitlab.haskell.org/ghc/doc/users_guide/exts/poly_kinds.html#overview-of-type-in-type
https://zenn.dev/mod_poppo/books/haskell-type-level-programming/viewer/types-and-kinds
https://ghc.gitlab.haskell.org/ghc/doc/users_guide/exts/poly_kinds.html#the-kind-type
