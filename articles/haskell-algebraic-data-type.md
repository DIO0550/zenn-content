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

カインドとは、**型の型**の様なものであり、型引数を受け取らない Int は`*`で表される。

## 参考

https://haskell.jp/blog/posts/2020/how-to-use-type-newtype-data.html
http://walk.northcol.org/haskell/adts/
https://qiita.com/7shi/items/1ce76bde464b4a55c143
https://qiita.com/tobita_yoshiki/items/a2ffd2cefa53376338c6
