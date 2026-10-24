# yy-encodingの内部仕様

| モジュール | 責務 |
| --- | --- |
| [lib.rs](../../crates/yy-encoding/src/lib.rs) | Encoding、Decoder/Encoder、統計、エスケープ、全体変換API |
| [unicode.rs](../../crates/yy-encoding/src/unicode.rs) | UTF系コーデック |
| [web.rs](../../crates/yy-encoding/src/web.rs) | encoding_rsとの接続 |
| [dbcs.rs](../../crates/yy-encoding/src/dbcs.rs) | 日本語の多バイト符号処理 |
| [ebcdic.rs](../../crates/yy-encoding/src/ebcdic.rs) | CCSID、SO/SI、レコード区切りとシフト状態 |
| [mapfile.rs](../../crates/yy-encoding/src/mapfile.rs) | 外部対応表の解析・登録 |
| [detect.rs](../../crates/yy-encoding/src/detect.rs) | 検出候補と評価 |
| [tables](../../crates/yy-encoding/src/tables/) | 生成済みJIS/EBCDIC変換表 |

内部の文書表現はUTF-8。不正バイトは`U+10FE00 + バイト値`で保持します。
入力に同じ私用領域文字がある場合は`literal_escapes`を確認し、誤って元バイトに復元しない扱いが必要です。
非正規符号は意味を保っても再符号化のバイトが変わる場合があるため、`noncanonical`で区別します。
EBCDICの末尾SO状態は`open_shift_at_end`として扱い、再符号化で余分なSIを付けないためのAPIを提供します。

[往復試験](../../crates/yy-encoding/tests/roundtrip.rs)、[EBCDIC試験](../../crates/yy-encoding/tests/ebcdic.rs)、
[外部表試験](../../crates/yy-encoding/tests/mapfile.rs)を内部仕様の回帰条件として扱います。
公開APIの使い方は[利用者向け](../user/README.md)へ分離します。
