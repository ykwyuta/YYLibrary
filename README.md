# YYLibrary

YYシリーズとYYSortで共用するRustライブラリと開発ツールです。

- **yy-encoding**: Unicode、日本語、EBCDIC、外部対応表、文字コード検出、不正バイトの保持。
- **gen-tables**: 公開対応表から生成済みコーデックテーブルを再生成する保守用ツール。

## 文書

| 目的 | 入口 |
| --- | --- |
| アプリからライブラリを利用する | [利用者向け：依存設定と公開API](docs/user/README.md) |
| ライブラリ自体を実装・保守する | [開発者向け：内部仕様、生成、試験、CI](docs/developer/README.md) |
| 共有化の判断・作業結果を調べる | [検討経緯・作業記録](docs/history/README.md) |

Rust 1.88以降。利用側はCargo.tomlの`git`と完全な`rev`、Cargo.lockを固定します。
コードと対応表は本リポジトリで管理し、利用側へ実装の複製を置きません。

[GPL-3.0-or-later](COPYING)と[追加許可](COPYING.EXCEPTION)。
対応表の第三者表記は[yy-encodingのNOTICE](crates/yy-encoding/NOTICE.md)および生成ソースに保持します。
