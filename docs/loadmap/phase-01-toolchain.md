# Phase 01: 環境構築とツールチェーン

**目安期間:** 1–2 日  
**前提:** ターミナル操作、他言語でのビルド経験  
**次フェーズ:** [Phase 02: 基本構文と型システム](./phase-02-syntax-and-types.md)

## ゴール

- Go をインストールし、`go` コマンドだけでプログラムをビルド・実行できる
- Go の「フォーマットは議論しない」文化と、標準ツールが言語と一体である点を理解する
- シングルバイナリ・クロスコンパイルの前提を把握する

## 学習内容

### 1.1 インストールと検証

```bash
go version
go env GOPATH GOROOT GOOS GOARCH
```

- [Install Go](https://go-tour-jp.appspot.com/doc/install) に従って最新安定版をインストール
- モジュールモードがデフォルト — `go mod init` でプロジェクトを開始（GOPATH ワークスペースは Phase 08 で補足）

### 1.2 `go` コマンドの基本

| コマンド | 用途 |
|---------|------|
| `go run` | ビルド＋実行（開発時） |
| `go build` | バイナリ生成 |
| `go install` | `$GOBIN`（未設定時は `$GOPATH/bin`）へインストール |
| `go fmt` / `gofmt` | コード整形（**必須習慣**） |
| `go vet` | 静的解析（疑わしいコードを検出） |
| `go doc` | ドキュメント閲覧 |
| `go mod init` | モジュール初期化 |

**公式:** [Tutorial: Getting started](https://go-tour-jp.appspot.com/doc/tutorial/getting-started)

### 1.3 フォーマット文化

- Go ではインデント・改行スタイルをチームで議論しない — `gofmt` が唯一の正解
- エディタの「保存時 gofmt」を有効にする
- これは可読性と大規模開発（Google 発の設計背景）に直結する

### 1.4 シングルバイナリとクロスコンパイル

```bash
GOOS=linux GOARCH=amd64 go build -o myapp .
```

- ランタイムがバイナリに同梱されるため、デプロイが単純
- `GOOS` / `GOARCH` でターゲットを切り替え可能 — インフラ設計の前提として意識する

## 演習

1. `learn-golang` リポジトリに `cmd/hello/main.go` を作成し、`go run` で Hello World
2. 意図的にインデントを崩し、`go fmt` で自動修正されることを確認
3. `go vet ./...` を実行し、警告が出たら内容を読む
4. macOS 上から Linux 向けバイナリを `GOOS=linux go build` で生成

## 完了チェックリスト

- [x] `go version` で最新安定版が表示される
- [x] `go run` / `go build` / `go mod init` を説明できる
- [x] なぜ `gofmt` を使うのか、他言語の formatter との違いを一言で言える
- [x] クロスコンパイルの概念を理解した

## 参照

- [Go Documentation — Getting Started](https://go-tour-jp.appspot.com/doc/)
- [Tutorial: Getting started](https://go-tour-jp.appspot.com/doc/tutorial/getting-started)
- [How to Write Go Code](https://go-tour-jp.appspot.com/doc/code.html) — モジュール内のパッケージ構成
- [Command go](https://pkg.go-tour-jp.appspot.com/cmd/go)
- [Editor plugins and IDEs](https://go-tour-jp.appspot.com/doc/editors.html) — gopls / 保存時 gofmt
