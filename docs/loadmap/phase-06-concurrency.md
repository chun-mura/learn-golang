# Phase 06: 並行処理（goroutine / channel）

**目安期間:** 1–2 週  
**前提:** [Phase 05](./phase-05-error-handling.md) 完了  
**次フェーズ:** [Phase 07: メモリモデルとランタイム](./phase-07-memory-and-runtime.md)

## ゴール

- goroutine を「数万単位で投げてよい軽量スレッド」として使える
- **「共有メモリによる通信ではなく、通信によってメモリを共有せよ」**（CSP モデル）
- `select`, `context.Context` でタイムアウト・キャンセル可能な並行処理を書く

## 学習内容

### 6.1 goroutine

```go
go func() {
    // 非同期実行
}()
```

- OS スレッドより軽量 — Go ランタイムが M:N スケジューリング
- main が終了すると goroutine も終了 — **必ず完了を待つ設計**（WaitGroup, channel 等）

### 6.2 channel

| 種類 | 用途 |
|------|------|
| 非バッファ | 送信・受信が同期（handoff） |
| バッファ | 非同期キュー（容量に注意） |

```go
ch := make(chan int)      // 非バッファ
ch := make(chan int, 10)  // バッファ 10
```

- **close** — 送信側のみ。受信側は `for v := range ch` で drain
- nil channel は永久ブロック — 意図的に使う advanced パターンあり

### 6.3 代表的パターン

- **Worker pool** — 固定数 goroutine + ジョブ channel
- **Pipeline** — stage ごとに channel で接続
- **Fan-out / Fan-in** — 並列化と集約
- **Rate limiter** — ticker + channel または `golang.org/x/time/rate`

### 6.4 select

```go
select {
case v := <-ch:
    ...
case <-time.After(5 * time.Second):
    ...
case <-ctx.Done():
    return ctx.Err()
}
```

- 多重待ち、タイムアウト、デフォルト（`default` でノンブロッキング）

### 6.5 context.Context

```go
ctx, cancel := context.WithTimeout(parent, 3*time.Second)
defer cancel()
```

- リクエストスコープのキャンセル・期限・値（値は乱用しない）
- HTTP サーバ、DB クエリ、gRPC 等で必須
- **参照:** [Go blog — context](https://go-tour-jp.appspot.com/blog/context)

### 6.6 落とし穴

- goroutine リーク — channel 送信側がブロックしたまま
- deadlock — `sync` パッケージの誤用
- data race — 共有変数への concurrent アクセス（Phase 09 の `-race` で検出）

## 演習

1. **[A Tour of Go — Concurrency](https://go-tour-jp.appspot.com/tour/concurrency/1)** 全章 + 演習
2. 必須ミニプロジェクト（いずれか 2 つ以上）:
   - **ワーカーキュー** — N 個の worker が jobs channel から取得
   - **Rate limiter** — 秒あたり K リクエスト制限
   - **Pipeline** — 読み取り → 変換 → 書き出しを channel で接続
3. 各プロジェクトに `context` によるキャンセルを組み込む
4. 他言語（async/await, thread pool）版と比較し、**デッドロック・リークの起き方** をメモ

## 完了チェックリスト

- [ ] `sync.WaitGroup` で goroutine 完了を待てる
- [ ] バッファ有無による挙動差を説明できる
- [ ] `select` + `context` でタイムアウト処理が書ける
- [ ] worker pool または pipeline を一から実装できる
- [ ] goroutine リークの典型原因を 2 つ挙げられる

## 参照

- [A Tour of Go — Concurrency](https://go-tour-jp.appspot.com/tour/concurrency/1)
- [Effective Go — Concurrency](https://go-tour-jp.appspot.com/doc/effective_go#concurrency)
- [Go blog — Share Memory By Communicating](https://go-tour-jp.appspot.com/blog/codelab-share)
- [Go blog — context](https://go-tour-jp.appspot.com/blog/context)
- [Go blog — Concurrency is not Parallelism](https://go-tour-jp.appspot.com/blog/waza-talk) — goroutine の使いどころ
- [pkg.go-tour-jp.appspot.com — context](https://pkg.go-tour-jp.appspot.com/context)
- [Concurrency in Go（書籍）](https://www.oreilly.com/library/view/concurrency-in-go/9781491941294/)
- [Go by Example — Goroutines, Channels, Worker Pools](https://gobyexample.com/goroutines)
