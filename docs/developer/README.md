# YYLibraryの開発・保守

[文書全体](../README.md) / [内部仕様](encoding-internals.md)

## ビルドと試験

Rust stable、Rust 1.88以降を使用し、リポジトリルートから実行します。

```text
cargo fmt --all -- --check
cargo clippy --workspace --all-targets --locked -- -D warnings
cargo test --workspace --locked
cargo doc -p yy-encoding --no-deps --open
```

[CI](../../.github/workflows/ci.yml)はWindows/Linuxで同じ検査を実行します。
ランダム往復、全符号、結合文字、ストリーム境界、シフト状態、外部対応表の検査を保ちます。
APIを変更した場合は利用側のyyeditorと各独立ワークスペース、YYSortでコンパイル・回帰を確認します。

## 対応表の再生成

```text
cargo run -p gen-tables -- x0213 PATH_TO_sjis-0213-2004-std.txt > crates/yy-encoding/src/tables/jisx0213.rs
cargo run -p gen-tables -- ebcdic PATH_TO_ucm_directory > crates/yy-encoding/src/tables/ebcdic.rs
```

リダイレクト先をUTF-8で保存するシェルを使用してください。通常の利用・ビルドでは再生成不要です。
入力形式と取得元は[生成ツール](../../tools/gen-tables/src/main.rs)に記載します。
生成ソースにある著作権・ライセンス表記を維持し、変換結果の差分と往復試験を確認してください。

## 更新の反映

YYLibraryの検証済みコミットを公開し、利用側のCargo.tomlの`rev`とCargo.lockを更新します。
変更の要点と実行した検証は[履歴](../history/README.md)へ記録し、現行のAPI説明と混在させません。
