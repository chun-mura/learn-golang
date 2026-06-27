# Phase 08: モジュール・標準ライブラリ・実践

**目安期間:** 1 週  
**前提:** [Phase 07](./phase-07-memory-and-runtime.md) 完了  
**次フェーズ:** [Phase 09: テストと品質保証](./phase-09-testing.md)

## ゴール

- Go modules で依存管理とバージョニングを行える
- **フレームワークなし**で CLI と HTTP サービスを標準ライブラリ中心に構築する
- Web/API 経験を活かしつつ「Go で書くとどこがシンプル・制約的か」を体感する

## 学習内容

### 8.1 Go Modules

```bash
go mod init example.com/myapp
go get example.com/some/module@v1.2.3
go mod tidy
```

- `go.mod` / `go.sum` — 再現可能ビルド
- semantic import versioning（v2+ は `/v2` サフィックス）
- **参照:** [Go Modules Reference](https://go-tour-jp.appspot.com/ref/mod)

### 8.2 プロジェクトレイアウト

```
myapp/
├── go.mod
├── cmd/myapp/main.go    # エントリポイント
├── internal/            # 外部公開しないパッケージ
└── pkg/                 # 公開可能なライブラリ（任意）
```

- 公式の厳密な標準はない — [golang-standards/project-layout](https://github.com/golang-standards/project-layout) は**非公式**の参考例（README も「必須ではない」と明記）
- `internal/` はコンパイラが強制するカプセル化

### 8.3 標準ライブラリの要点

| パッケージ | 用途 |
|-----------|------|
| `net/http` | HTTP サーバ・クライアント |
| `encoding/json` | JSON |
| `context` | キャンセル・期限 |
| `log/slog` | 構造化ログ（Go 1.21+） |
| `os`, `io`, `bufio` | ファイル・ストリーム |
| `time` | 時刻・タイマー |

**方針:** [Effective Go](https://go-tour-jp.appspot.com/doc/effective_go) — 標準ライブラリを読む。サードパーティは必要になってから。

### 8.4 HTTP サービス（net/http）

```go
http.HandleFunc("/health", func(w http.ResponseWriter, r *http.Request) {
    w.WriteHeader(http.StatusOK)
})
http.ListenAndServe(":8080", nil)
```

- `http.Handler` interface — Phase 04 の応用
- middleware パターン — 関数をラップ
- 本番では `http.Server` + `context` + graceful shutdown

**公式:** 最初は stdlib。Gin/Echo は Phase 10 で判断。

### 8.5 CLI

- `flag` パッケージ — 標準の CLI 引数（[pkg.go-tour-jp.appspot.com — flag](https://pkg.go-tour-jp.appspot.com/flag)）
- 規模が大きくなったら [Cobra](https://github.com/spf13/cobra) 等を検討 — 最初は stdlib で十分

## 演習

1. [Tutorial: Create a Go module](https://go-tour-jp.appspot.com/doc/tutorial/create-module) を完了
2. **CLI ツール** — ファイルを読み JSON 行数を出力（`flag` + `encoding/json`）
3. **HTTP サービス** — `net/http` のみで REST API（GET/POST 2 エンドポイント）
   - 他言語版と比較: ルーティング、middleware、JSON、エラー応答
4. [Go Web Examples](https://gowebexamples.com/) から 3 スニペットを再現

## 完了チェックリスト

- [ ] `go mod init` / `go get` / `go mod tidy` を説明できる
- [ ] `cmd/` + `internal/` 構成でプロジェクトを作れる
- [ ] `net/http` で CRUD の一部を実装できる
- [ ] 他言語の Web フレームワークと比べ、Go stdlib の長所・短所をメモした

## 参照

- [Go Modules Reference](https://go-tour-jp.appspot.com/ref/mod)
- [Modules — Developing and publishing](https://go-tour-jp.appspot.com/doc/modules/developing)
- [Tutorial: Create a Go module](https://go-tour-jp.appspot.com/doc/tutorial/create-module)
- [Tutorial: Multi-module workspaces](https://go-tour-jp.appspot.com/doc/tutorial/workspaces) — ローカル複数モジュール
- [Tutorial: Developing a RESTful API with Gin](https://go-tour-jp.appspot.com/doc/tutorial/web-service-gin)（stdlib 習得後の参考）
- [Go Web Examples](https://gowebexamples.com/)
- [pkg.go-tour-jp.appspot.com/std](https://pkg.go-tour-jp.appspot.com/std)
- [pkg.go-tour-jp.appspot.com — net/http](https://pkg.go-tour-jp.appspot.com/net/http)
