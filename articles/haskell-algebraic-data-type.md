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

カインドとは、大雑把にいうと「**型を分類**」を表す。  
haskell のカインドには、以下の 2 つの規則がある。

- 1. `*`は、全ての**データ型**のカインドである。
- 2. `k1 -> k2` は、k1 のカインドを持つ型を受け取り、k2 のカインドの型を生成する。

https://ja.wikipedia.org/wiki/%E3%82%AB%E3%82%A4%E3%83%B3%E3%83%89_(%E5%9E%8B%E7%90%86%E8%AB%96)

## 参考

https://haskell.jp/blog/posts/2020/how-to-use-type-newtype-data.html
http://walk.northcol.org/haskell/adts/
https://qiita.com/7shi/items/1ce76bde464b4a55c143
https://qiita.com/tobita_yoshiki/items/a2ffd2cefa53376338c6
https://zenn.dev/mod_poppo/books/haskell-type-level-programming/viewer/types-and-kinds
https://ja.wikipedia.org/wiki/%E3%82%AB%E3%82%A4%E3%83%B3%E3%83%89_(%E5%9E%8B%E7%90%86%E8%AB%96)
