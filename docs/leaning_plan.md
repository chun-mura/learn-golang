Goの言語特性にフォーカスするなら、「他言語と共通な部分」は一旦飛ばして、Goらしさが出るポイントに絞って学ぶのが効率良いです。 [qiita](https://qiita.com/melty_go/items/41885a4c7388d7a7b377)

## シンプル志向の型・構文

- 静的型付け＋シンプルな構文で「読めること優先」の設計になっており、予約語も少なく言語仕様が小さいことを理解する。 [zenn](https://zenn.dev/urakawa_jinsei/articles/6a4444d78c1300)
- 継承ではなく構造体とコンポジションが基本で、「小さなstructとinterfaceを組む」スタイルに慣れる（巨大クラスや継承ツリーを避ける文化）。 [qiita](https://qiita.com/imkaoru/items/1ca17fd4c353dab59707)

## インターフェースと暗黙実装

- Goのinterfaceは「暗黙実装」で、ある型がメソッドセットを満たした瞬間にinterfaceを満たすというduck typing的仕様を理解する。 [af-e](https://af-e.net/go-features/)
- `io.Reader`や`io.Writer`のような最小インターフェースを多用する文化と、「利用側パッケージがinterfaceを定義する」アンチ典型的OOPな設計方針。 [qiita](https://qiita.com/imkaoru/items/1ca17fd4c353dab59707)

## エラーハンドリングと言語哲学

- 例外ではなく「エラーは値」として扱う設計（`result, err := f()`→`if err != nil { ... }`）が、制御フローやAPI設計にどう影響するかを意識して読む。 [af-e](https://af-e.net/go-features/)
- `panic`/`recover`は例外ではなく「本当に異常なケース用」であり、通常ロジックには使わないという哲学（例外駆動設計からのマインドシフト）。 [qiita](https://qiita.com/melty_go/items/41885a4c7388d7a7b377)

## goroutineとchannelによる並行処理モデル

- goroutineが「軽量スレッド＋ランタイムスケジューラ」であり、数万単位で投げてもよい前提のモデルであること。 [oreilly](https://www.oreilly.com/library/view/shi-yong-goyan-yu-sisutemukai-fa-noxian-chang-dezhi-tuteokitaiadobaisu/9784873119694/concurrent/index.xhtml)
- channelによる通信・同期（共有メモリではなくメッセージパッシング）と、「共有メモリによる通信ではなく、通信によってメモリを共有せよ」というGoの合言葉に沿った設計感覚。 [dev-dojo](https://dev-dojo.jp/go/21/)
- `select`による多重待ち・タイムアウト・キャンセルと、`context.Context`との組み合わせで「キャンセル可能でリークしない並行処理」を書くイディオム。 [pathy](https://pathy.jp/posts/go-concurrent)

## メモリモデルとランタイム

- Goのメモリモデル（goroutine間でデータを共有する際の可視性やhappens-before関係）の概要を押さえ、「どこまでが言語が保証する範囲か」を理解する。 [computer-science-notes](https://www.computer-science-notes.com/programming_and_languages/programming_languages/go)
- GC付き言語としての性質と、エスケープ解析を通じて「スタック／ヒープ配置」「値／ポインタ渡し」のコスト感を掴む（マイクロ最適化ではなく設計レベルで意識）。 [qiita](https://qiita.com/melty_go/items/41885a4c7388d7a7b377)

## ツールチェーンとフォーマット文化

- `gofmt`による「フォーマットは議論しない」文化と、標準ツール（`go vet` `go test` `go doc`など）が言語仕様と強く結びついている点。 [harmonic-society.co](https://harmonic-society.co.jp/go-beginner-guide/)
- シングルバイナリ・クロスコンパイル前提のビルド体験（`GOOS`/`GOARCH`で別環境向けバイナリを簡単に出せる）と、そのためにランタイムがどう設計されているか。 [harmonic-society.co](https://harmonic-society.co.jp/go-beginner-guide/)

## 言語設計思想と制約

- 「機能を足すのではなく削る」方向で進化している言語であること（ジェネリクス導入も慎重、予約語は25個前後に制限）と、それが大規模開発・読みやすさにどう効いているか。 [zenn](https://zenn.dev/r_casio/articles/8abe1b19b6c6ba)
- Googleの大規模開発での不満（ビルド時間・複雑な言語仕様）から出発し、「シンプルで速く、安全」な実装・ビルドを目指した背景を俯瞰して、自分のアーキテクチャ判断軸に取り込む。 [FAQ — 起源・設計原則](https://go.dokyumento.jp/doc/faq)、[Go at Google（英語）](https://go-tour-jp.appspot.com/talks/2012/splash.article)

## 学び方の軸（言語特性フォーカス）

- Web/API実装は既に分かる前提で、「同じ機能をGoで書くとどこがシンプル・制約的になるか」を他言語実装と比較しながら差分で学ぶ。 [qiita](https://qiita.com/imkaoru/items/1ca17fd4c353dab59707)
- 並行処理・エラーハンドリング・インターフェース設計あたりを題材に、小さめのサンプル（ワーカーキュー、rate limiter、pipeline処理）を実装し、「Goらしい書き方」と他言語の癖のギャップを意識してリファクタする。 [oreilly](https://www.oreilly.com/library/view/shi-yong-goyan-yu-sisutemukai-fa-noxian-chang-dezhi-tuteokitaiadobaisu/9784873119694/concurrent/index.xhtml)

***

言語特性にフォーカスしたいとのことなので、例えば「エラーは値」「goroutine＋channel」「暗黙interface」あたりを題材に、具体的なサンプルコード付きの学習ロードマップも組めますが、まず重点的に深掘りしたいのはどのあたり（エラーハンドリング・並行処理・インターフェース／設計など）でしょうか？
