# Phase 07: メモリモデルとランタイム

**目安期間:** 3–5 日  
**前提:** [Phase 06](./phase-06-concurrency.md) 完了  
**次フェーズ:** [Phase 08: モジュール・標準ライブラリ・実践](./phase-08-modules-and-practice.md)

## ゴール

- Go メモリモデルの **happens-before** 関係の概要を理解する
- GC 付き言語としての特性と、エスケープ解析によるスタック/ヒープ配置の概念
- 値渡し vs ポインタ渡しを**設計レベル**で判断できる（マイクロ最適化は後回し）

## 学習内容

### 7.1 Go Memory Model

- goroutine 間でデータを共有する際、**どこまで言語が保証するか**を知る
- channel 操作、`sync` プリミティブ、`Once` 等が happens-before を確立
- **参照:** [The Go Memory Model](https://go.dev/ref/mem)

```go
// 悪い例: 共有変数への unsynchronized アクセス
var flag bool
go func() { flag = true }()
if flag { ... } // data race の可能性
```

### 7.2 ガベージコレクション

- マーク＆スイープ GC — 停止時間（STW）は世代ごとに改善されている
- 開発者が `free` しない — **割り当てを減らす**設計が有効
- `GOGC` 環境変数で GC チューニング（本番前のプロファイリングとセット）

### 7.3 エスケープ解析

```bash
go build -gcflags='-m' ./...
```

- コンパイラが「変数が関数外に逃げるか」を解析
- ヒープ escape → GC 圧力増加
- **最初は気にしすぎない** — ボトルネックが見えてから `pprof` で確認

### 7.4 値 vs ポインタ（設計判断）

| 値 | ポインタ |
|----|---------|
| 小さく immutable な struct | 大きな struct |
| map/slice の要素が小さい | 同一インスタンスを共有・変更 |
| コピーで独立させたい | nil を表現したい |

- Phase 03 のレシーバ選択と統合して復習

### 7.5 sync パッケージ（channel 以外の同期）

- `sync.Mutex` / `RWMutex` — 共有状態の保護
- `sync.Once` — 一度きり初期化
- `sync/atomic` — 低レベル atomic 操作（mutex で足りるなら mutex 優先）

**原則:** まず channel。どうしても共有状態が必要なら mutex + メモリモデル理解。

## 演習

1. [Go Memory Model](https://go.dev/ref/mem) を読み、channel による happens-before の例を自分の言葉で要約
2. 小さな struct を値/ポインタ両方で渡し、`-gcflags='-m'` で escape の有無を確認
3. `sync.Mutex` でカウンタを保護するコードと、channel で集約するコードを書き、トレードオフを比較
4. `go test -race` で意図的な data race を検出（Phase 09 と連動可）

## 完了チェックリスト

- [ ] happens-before の概念を説明できる
- [ ] data race と race condition の違いを説明できる
- [ ] escape 解析の出力を読んで「ヒープに載る理由」を推測できる
- [ ] channel vs mutex の選択基準を説明できる

## 参照

- [The Go Memory Model](https://go.dev/ref/mem)
- [Go blog — Go 1.5 GC](https://go.dev/blog/go15gc)（背景理解）
- [Go Wiki — Performance](https://go.dev/wiki/Performance)
- [pkg.go.dev — runtime](https://pkg.go.dev/runtime)
