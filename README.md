# YYLibrary

YYシリーズとYYSortで共通利用するRustライブラリと開発ツールです。

- `crates/yy-encoding`: 日本語、Unicode、EBCDIC、外部対応表、文字コード検出と不正バイトの可逆変換。
- `tools/gen-tables`: 公開対応表から文字コード変換テーブルを生成するツール。

## 利用

CargoのGit依存で使用します。再現可能なビルドのため、`rev`には採用したコミットの完全SHAを指定します。

```toml
yy-encoding = { git = "https://github.com/ykwyuta/YYLibrary", rev = "<commit SHA>" }
```

利用側のCargo.lockも更新・保存してください。通常のCargoビルドがライブラリを取得するため、
利用者が別途cloneしたりサブモジュールを初期化したりする必要はありません。
ライブラリの更新は本リポジトリで行い、利用側のrevとCargo.lockを明示的に更新します。

## 開発と検証

```text
cargo fmt --all -- --check
cargo clippy --workspace --all-targets --locked -- -D warnings
cargo test --workspace --locked
```

WindowsとLinuxのCIで同じ検査を行います。文字コードテーブルは生成済みのものを同梱しており、
通常のビルド時に対応表の取得や再生成は不要です。
生成方法は[gen-tablesの説明](tools/gen-tables/src/main.rs)を参照してください。
GPL-3.0-or-later。GPL本文はCOPYING、追加許可はCOPYING.EXCEPTIONに記載しています。
第三者の対応表の著作権・ライセンス表記は生成ソースと各クレートのNOTICEに保持しています。
