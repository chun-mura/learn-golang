# Go 学習ロードマップ

`docs/leaning_plan.md` の「Go らしさにフォーカスする」方針と、[Go 公式ドキュメント](https://go.dev/doc/) の推奨学習順序を統合したロードマップです。

## 前提

- Web/API 開発の経験はあるものとする（他言語で同等機能を書いたことがある）
- 他言語と共通する部分（変数、制御構文、HTTP の基本概念など）は最小限にとどめ、**Go 固有の設計・イディオム**を中心に学ぶ
- Go Team は公式の「ロードマップ」を公開していない。本ロードマップは [go.dev/learn](https://go.dev/learn/) のリソース群と [Effective Go](https://go.dev/doc/effective_go) を骨格にしている

## フェーズ一覧

| フェーズ | テーマ | 目安期間 | ファイル |
|---------|--------|---------|---------|
| 01 | 環境構築とツールチェーン | 1–2 日 | [phase-01-toolchain.md](./phase-01-toolchain.md) |
| 02 | 基本構文と型システム | 2–3 日 | [phase-02-syntax-and-types.md](./phase-02-syntax-and-types.md) |
| 03 | 構造体とコンポジション | 2–3 日 | [phase-03-structs-and-composition.md](./phase-03-structs-and-composition.md) |
| 04 | インターフェースと暗黙実装 | 3–4 日 | [phase-04-interfaces.md](./phase-04-interfaces.md) |
| 05 | エラーハンドリングと言語哲学 | 2–3 日 | [phase-05-error-handling.md](./phase-05-error-handling.md) |
| 06 | 並行処理（goroutine / channel） | 1–2 週 | [phase-06-concurrency.md](./phase-06-concurrency.md) |
| 07 | メモリモデルとランタイム | 3–5 日 | [phase-07-memory-and-runtime.md](./phase-07-memory-and-runtime.md) |
| 08 | モジュール・標準ライブラリ・実践 | 1 週 | [phase-08-modules-and-practice.md](./phase-08-modules-and-practice.md) |
| 09 | テストと品質保証 | 3–5 日 | [phase-09-testing.md](./phase-09-testing.md) |
| 10 | 設計思想と総合演習 | 1–2 週 | [phase-10-design-and-capstone.md](./phase-10-design-and-capstone.md) |

## 推奨学習順序（公式に沿った最小パス）

```
Install → Tour of Go（全4章）→ Effective Go → 小さな CLI / HTTP サービス → 並行処理 → テスト
```

各フェーズ完了時に「完了チェックリスト」を確認してから次へ進む。

## 信頼できる情報源（優先度順）

### 一次情報（最優先）

| リソース | URL | 用途 |
|---------|-----|------|
| Go Documentation | https://go.dev/doc/ | 公式ドキュメント索引 |
| A Tour of Go | https://go.dev/tour/ | 対話型入門（4 章構成） |
| Effective Go | https://go.dev/doc/effective_go | イディオムとベストプラクティス |
| Language Specification | https://go.dev/ref/spec | 言語仕様（参照用） |
| Go Modules Reference | https://go.dev/ref/mod | モジュール管理 |
| pkg.go.dev | https://pkg.go.dev/std | 標準ライブラリ API リファレンス |
| Go Memory Model | https://go.dev/ref/mem | メモリモデル |
| Go Wiki | https://go.dev/wiki/ | コミュニティ維持の補足記事 |

### 公式チュートリアル

| リソース | URL |
|---------|-----|
| Getting started | https://go.dev/doc/tutorial/getting-started |
| Create a Go module | https://go.dev/doc/tutorial/create-module |
| Developing a RESTful API (Gin) | https://go.dev/doc/tutorial/web-service-gin |
| Go by Example | https://gobyexample.com/ |
| Go Web Examples | https://gowebexamples.com/ |

### 書籍（中〜上級）

| 書籍 | 著者 | 用途 |
|------|------|------|
| The Go Programming Language | Donovan & Kernighan | 体系的な言語リファレンス |
| Concurrency in Go | Katherine Cox-Buday | 並行処理の深掘り |

### 補助資料（日本語・概念理解）

- [Go 言語特性まとめ（leaning_plan 参照元）](../leaning_plan.md) — 本リポジトリの学習方針
- [実用 Go 言語システム開発の現場で知っておきたいこと（O'Reilly）](https://www.oreilly.com/library/view/shi-yong-goyan-yu-sisutemukai-fa-noxian-chang-dezhi-tuteokitaiadobaisu/9784873119694/) — 並行処理・実務イディオム

## 学び方の原則

1. **差分で学ぶ** — 既知の Web/API 実装と Go 版を並べ、シンプルになる点・制約される点をメモする
2. **小さく書いてリファクタ** — ワーカーキュー、rate limiter、pipeline など題材を固定し、「Go らしい書き方」へ段階的に寄せる
3. **フレームワークは後** — 最初は `net/http` と標準ライブラリで書く（[Effective Go](https://go.dev/doc/effective_go) 推奨）
4. **テストと並行** — 各フェーズの演習には `go test` をセットで書く
