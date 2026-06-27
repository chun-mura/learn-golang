# Go 学習ロードマップ

`docs/leaning_plan.md` の「Go らしさにフォーカスする」方針と、[Go 公式ドキュメント](https://go-tour-jp.appspot.com/doc/) の推奨学習順序を統合したロードマップです。

## 前提

- Web/API 開発の経験はあるものとする（他言語で同等機能を書いたことがある）
- 他言語と共通する部分（変数、制御構文、HTTP の基本概念など）は最小限にとどめ、**Go 固有の設計・イディオム**を中心に学ぶ
- Go Team は公式の「ロードマップ」を公開していない。本ロードマップは [go-tour-jp.appspot.com/learn](https://go-tour-jp.appspot.com/learn/) のリソース群と [Effective Go](https://go-tour-jp.appspot.com/doc/effective_go) を骨格にしている

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

**原則:** 一次情報（go-tour-jp.appspot.com）を最優先する。GOPATH 前提の古い日本語記事（Go 4 Beginners、とほほの Go 言語入門、golang.jp 等）は参照しない。

### 一次情報（最優先）

| リソース | URL | 用途 |
|---------|-----|------|
| Go Documentation | https://go-tour-jp.appspot.com/doc/ | 公式ドキュメント索引 |
| Go Documentation（日本語） | https://go.dokyumento.jp/doc/ | 上記の日本語訳 |
| A Tour of Go | https://go-tour-jp.appspot.com/tour/ | 対話型入門（Basics / Methods / Generics / Concurrency） |
| Effective Go | https://go-tour-jp.appspot.com/doc/effective_go | イディオムとベストプラクティス |
| FAQ | https://go-tour-jp.appspot.com/doc/faq | 言語設計の背景・設計原則 |
| Language Specification | https://go-tour-jp.appspot.com/ref/spec | 言語仕様（参照用） |
| Go Modules Reference | https://go-tour-jp.appspot.com/ref/mod | モジュール管理 |
| pkg.go-tour-jp.appspot.com | https://pkg.go-tour-jp.appspot.com/std | 標準ライブラリ API リファレンス |
| Go Memory Model | https://go-tour-jp.appspot.com/ref/mem | メモリモデル |
| Go GC Guide | https://go-tour-jp.appspot.com/doc/gc-guide | ガベージコレクション |
| Go Wiki | https://go-tour-jp.appspot.com/wiki/ | コミュニティ維持の補足記事 |
| Go blog | https://go-tour-jp.appspot.com/blog/ | 言語変更・新機能の公式解説 |
| Release Notes | https://go-tour-jp.appspot.com/doc/devel/release | バージョンごとの変更点 |

### 言語設計思想（背景理解）

| リソース | URL | 備考 |
|---------|-----|------|
| FAQ — 起源・設計原則 | https://go.dokyumento.jp/doc/faq | 日本語。Go 4 Beginners の代替 |
| Go at Google | https://go-tour-jp.appspot.com/talks/2012/splash.article | 英語。ソフトウェア工学としての設計 |
| Why Generics? | https://go-tour-jp.appspot.com/blog/why-generics | 英語。ジェネリクス慎重導入の経緯 |

### 公式チュートリアル

| リソース | URL |
|---------|-----|
| Getting started | https://go-tour-jp.appspot.com/doc/tutorial/getting-started |
| Create a Go module | https://go-tour-jp.appspot.com/doc/tutorial/create-module |
| Getting started with generics | https://go-tour-jp.appspot.com/doc/tutorial/generics |
| Multi-module workspaces | https://go-tour-jp.appspot.com/doc/tutorial/workspaces |
| Developing a RESTful API (Gin) | https://go-tour-jp.appspot.com/doc/tutorial/web-service-gin |
| Accessing a relational database | https://go-tour-jp.appspot.com/doc/tutorial/database-access |
| Getting started with fuzzing | https://go-tour-jp.appspot.com/doc/tutorial/fuzz |
| Go by Example | https://gobyexample.com/ |
| Go Web Examples | https://gowebexamples.com/ |

### 書籍（中〜上級・辞書代わり）

| 書籍 | 著者 | 用途 |
|------|------|------|
| The Go Programming Language | Donovan & Kernighan | 体系的な言語リファレンス |
| Concurrency in Go | Katherine Cox-Buday | 並行処理の深掘り |

### 補助資料

- [Go 言語特性まとめ（leaning_plan）](../leaning_plan.md) — 本リポジトリの学習方針
- [実用 Go 言語（O'Reilly 日本語版）](https://www.oreilly.com/library/view/shi-yong-goyan-yu-sisutemukai-fa-noxian-chang-dezhi-tuteokitaiadobaisu/9784873119694/) — 実務イディオム（Go 1.18+ 内容を含む）

## 学び方の原則

1. **差分で学ぶ** — 既知の Web/API 実装と Go 版を並べ、シンプルになる点・制約される点をメモする
2. **小さく書いてリファクタ** — ワーカーキュー、rate limiter、pipeline など題材を固定し、「Go らしい書き方」へ段階的に寄せる
3. **フレームワークは後** — 最初は `net/http` と標準ライブラリで書く（[Effective Go](https://go-tour-jp.appspot.com/doc/effective_go) 推奨）
4. **テストと並行** — 各フェーズの演習には `go test` をセットで書く
