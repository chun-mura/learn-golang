# Phase 03: 構造体とコンポジション

**目安期間:** 2–3 日  
**前提:** [Phase 02](./phase-02-syntax-and-types.md) 完了  
**次フェーズ:** [Phase 04: インターフェースと暗黙実装](./phase-04-interfaces.md)

## ゴール

- 継承ではなく **struct + コンポジション（embedding）** で設計する文化に慣れる
- メソッド、レシーバ（値 vs ポインタ）の選択基準を理解する
- 「巨大クラス階層を避ける」Go の設計感覚を身につける

## 学習内容

### 3.1 struct 基本

```go
type User struct {
    ID   int
    Name string
}

func (u *User) Rename(name string) {
    u.Name = name
}
```

- フィールドはコンパイル時に固定 — 動的な属性追加はしない
- タグ（`` `json:"name"` ``）でシリアライズ等のメタデータを付与

### 3.2 コンポジション vs 継承

```go
type Admin struct {
    User          // 匿名 embedding — User のメソッドが昇格
    Permissions []string
}
```

- **継承はない** — 「is-a」より「has-a」＋小さな struct の組み合わせ
- embedding でフィールド・メソッドを昇格させるが、これは継承の代替ではなく合成

### 3.3 レシーバ: 値 vs ポインタ

| 値レシーバ | ポインタレシーバ |
|-----------|----------------|
| 小さな immutable な型 | フィールドを変更する |
| コピーコストが低い | 大きな struct |
| | インターフェース実装の一貫性 |

**Effective Go の指針:** 迷ったらポインタレシーバ。ただし Phase 07 でコスト感も学ぶ。

### 3.4 コンストラクタパターン

Go に `new Type()` 以外の公式コンストラクタはない。慣例:

```go
func NewUser(name string) (*User, error) {
    if name == "" {
        return nil, errors.New("name required")
    }
    return &User{Name: name}, nil
}
```

- `NewXxx` 関数 + バリデーション + エラー返却

## 演習

1. **[A Tour of Go — 第 2 章前半（Methods）](https://go-tour-jp.appspot.com/tour/methods/1)** を完了
2. ドメインモデルを設計:
   - `Account` struct
   - `PremiumAccount` — embedding で `Account` を拡張（継承ではなく合成）
   - 各 struct に 1–2 メソッド
3. 値レシーバ版とポインタレシーバ版を書き、挙動の差（変更が反映されるか）を確認
4. 他言語（Java/C#/TypeScript 等）の同モデルを思い浮かべ、**何行減ったか / 何が書けなくなったか** をメモ

## 完了チェックリスト

- [ ] embedding と継承の違いを説明できる
- [ ] ポインタレシーバが必要な典型ケースを 3 つ挙げられる
- [ ] `NewXxx` コンストラクタパターンで struct を生成できる
- [ ] struct タグで JSON マーシャリングができる（`encoding/json`）

## 参照

- [A Tour of Go — Methods](https://go-tour-jp.appspot.com/tour/methods/1)
- [Effective Go — Structs](https://go-tour-jp.appspot.com/doc/effective_go#structs)
- [Effective Go — Embedding](https://go-tour-jp.appspot.com/doc/effective_go#embedding)
- [FAQ — 型継承がないのはなぜですか？](https://go.dokyumento.jp/doc/faq) — コンポジションの背景
- [Go by Example — Structs, Methods, Embedding](https://gobyexample.com/structs)
