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

## ファンクタ

モナドを理解するのに、まずはモナドに関わりのあるファンクタ(functor)という型クラスについても理解する必要があります。
ファンクタは、簡単に説明すると「箱に入っている値に対して関数を適用して別の値にする処理を提供する」型クラスになります。
ここでいう「箱」とは、`Maybe`、`[]`（リスト）、`Either e` のような、型引数を「1 つ」受け取る型コンストラクタのことを指します。
「箱に入った値」とは、`Just 5`、`[1,2,3]`、`Right "hello"` のような、型コンストラクタ（Maybe や []）によって定義された型の実際の値のことを指します。

### 定義

ファンクタは以下のように定義されています。

```haskell
class Functor f where
    fmap :: (a -> b) -> f a -> f b
```

この型クラスの定義は、以下になります。

- 型クラス Functor は、型変数 f を受け取る
- `fmap`関数が「a を引数にして、b を返す」関数を受け取り、「コンテナ f に包まれた a」を「コンテナ f に包まれた b」に変換する

### 例

TODO: 自分で作った型を使うのもいいかも

## アプリケイティブ

次に、重要になるのが、Applicative です。
Applicative は、ファンクタとモナドの間の処理を行う型クラスになります。

### 定義

```haskell
class Functor f => Applicative (f :: Type -> Type) where
```

## モナドとは

https://www.infoq.com/jp/articles/Understanding-Monads-guide-for-perplexed/
http://walk.northcol.org/haskell/overview/

## 参考

### ファンクタ

- https://www.nct9.ne.jp/m_hiroi/func/haskell14.html
- https://wiki.haskell.org/index.php?title=Functor
- https://qiita.com/suin/items/0255f0637921dcdfe83b

- https://www.infoq.com/jp/articles/Understanding-Monads-guide-for-perplexed/
- https://qiita.com/kerupani129/items/333155e5e2dee644d6dc
- http://walk.northcol.org/haskell/monads/
- https://www.tohoho-web.com/ex/haskell.html
