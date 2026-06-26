# Phase 10: 設計思想と総合演習

**目安期間:** 1–2 週  
**前提:** [Phase 01](./phase-01-toolchain.md) 〜 [Phase 09](./phase-09-testing.md) 完了  
**位置づけ:** ロードマップの総仕上げ — 学んだ Go 固有概念を統合する

## ゴール

- Go の言語設計思想（シンプルさ、読みやすさ、明示性）をアーキテクチャ判断に活かす
- 総合演習で **エラーは値 / 暗黙 interface / goroutine+channel** を一つのプロジェクトに統合
- フレームワーク・周辺エコシステムを「必要になったら」選べる

## 学習内容

### 10.1 言語設計思想の俯瞰

| 原則 | 実践への影響 |
|------|-------------|
| 小さな言語仕様 | 覚えることが少ない、読み手優先 |
| 明示的エラー | API が `(T, error)` を強制、制御フローが見える |
| コンポジション | 深い継承階層を作らない |
| 並行処理が言語機能 | goroutine/channel が第一級 |
| 標準ツール一体 | fmt, test, vet, doc が文化を形成 |

**参照:** [leaning_plan](../leaning_plan.md)、[Effective Go 全体](https://go.dev/doc/effective_go)

### 10.2 コードレビュー文化

- [Go Code Review Comments](https://go.dev/wiki/CodeReviewComments) — 実務で参照される de facto 標準
- 命名、エラーメッセージ、interface サイズ、context の渡し方

### 10.3 フレームワークとライブラリ（必要時）

| 領域 | 代表例 | 学ぶタイミング |
|------|--------|---------------|
| HTTP router | chi, gin, echo | stdlib で足りなければ |
| ORM | sqlc, ent, GORM | SQL 明示性 vs 生産性 |
| DI | wire, fx | 過度な DI は Go では非推奨なことが多い |
| Config | env, viper | 12-factor |
| Observability | OpenTelemetry, zap/slog | 本番投入前 |

**Go Team / 実務の傾向:** フレームワークより stdlib + 小さな composable ライブラリ。

### 10.4 総合演習（Capstone）— いずれか 1 つ

#### A. 並行 HTTP プロキシ / Rate-limited API

- `net/http` + middleware
- `context` によるタイムアウト
- goroutine + channel または `x/time/rate` でレート制限
- 全エンドポイントに table-driven test + `-race`

#### B. パイプライン型ログ処理 CLI

- ファイル読み取り → パース → 集計 → 出力
- pipeline パターン（Phase 06）
- `errors` wrap でエラー経路を明示
- benchmark でボトルネック確認

#### C. 小さなサービス + インターフェース境界

- `internal/` に domain + repository interface（consumer 定義）
- fake repository で unit test
- 本番 repository は DB またはインメモリ

### 10.5 他言語との最終比較（リフレクション）

以下について各 200 字程度でまとめる:

1. エラーハンドリング — 例外 vs 値
2. 並行処理 — async/await vs goroutine/channel
3. 抽象化 — 継承/implements vs 暗黙 interface + コンポジション
4. Web 開発 — フルスタック FW vs stdlib 中心

### 10.6 継続学習

- [Go blog](https://go.dev/blog/) — 言語変更の公式説明
- [Go Release Notes](https://go.dev/doc/devel/release) — バージョンごとの変更
- [The Go Programming Language（書籍）](https://www.gopl.io/) — 辞書代わり
- [Concurrency in Go（書籍）](https://www.oreilly.com/library/view/concurrency-in-go/9781491941294/) — 並行処理の深掘り

## 完了チェックリスト

- [ ] Capstone を `go test -race ./...` でパスさせた
- [ ] Code Review Comments を一読し、3 項目以上を自分のコードに適用した
- [ ] フレームワークを使う/使わないの判断基準を説明できる
- [ ] 他言語経験者として「Go を選ぶ理由」を具体例付きで説明できる

## 参照

- [Effective Go](https://go.dev/doc/effective_go)
- [Go Code Review Comments](https://go.dev/wiki/CodeReviewComments)
- [Go Developer Survey](https://go.dev/blog/survey) — エコシステムの実態
- [leaning_plan](../leaning_plan.md)

---

**おめでとうございます。** Phase 01–10 を完了した時点で、Go の言語特性にフォーカスした基礎〜実践レベルは達成しています。本番運用（デプロイ、可観測性、DB 最適化）は別途、業務ドメインに合わせて深掘りしてください。
