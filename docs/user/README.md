# YYLibraryをアプリから利用する

[文書全体](../README.md)。ここでの「利用者」はYYLibraryを組み込むアプリケーション開発者です。
ライブラリ内部の変更手順は[開発者向け](../developer/README.md)へ分離しています。

## 依存設定

```toml
yy-encoding = { git = "https://github.com/ykwyuta/YYLibrary", rev = "57deca92c834227f705496fd7c5c4a226ec8dc97" }
```

このSHAは利用できる固定参照の例です。更新時は採用する完全SHAへ置き換え、利用側のCargo.lockも更新・保存します。
Cargoが取得するため別途cloneやsubmodule初期化は不要です。通常ビルドでは生成済みの対応表を使用します。

## 公開API

小さい入力の例です。デコード結果はUTF-8のバイト列です。

```rust
use yy_encoding::{decode_all, encode_all, Encoding, EscapeMode};

let bytes = b"hello";
let (text, stats) = decode_all(Encoding::Utf8, bytes, true);
assert_eq!(stats.invalid, 0);
let restored = encode_all(Encoding::Utf8, &text, EscapeMode::Restore).unwrap();
assert_eq!(restored, bytes);
```

- `Encoding::from_name`・`Encoding::all`・`Encoding::spec`: 対応名の解決・一覧・保存用指定。
- `decode_all`・`encode_all`: バイト列全体を扱うAPI。大容量では`new_decoder`・`new_encoder`でストリーム処理する。
- `DecodeStats`: 不正バイト、非正規符号、入力中のエスケープ文字、EBCDICの終端シフト状態を確認する。
- `EscapeMode::Restore`: 同じ文字コードに戻す用途。別文字コードへ変換するときは`Reject`で変換不能範囲を扱う。
- `load_mappings`・`parse_mapping`・`register_mapping`: 外部対応表の読み込み・登録。
- `detect`・`detect_ebcdic`: 検出は推定であり、既知の文字コードがある場合は明示指定する。

元バイトの保持には統計とシフト状態も含めた扱いが必要です。単純な全変換がすべての表で
常にバイト一致するという契約ではありません。詳細は[公開APIのRustdoc](../../crates/yy-encoding/src/lib.rs)、
[EBCDIC](../../crates/yy-encoding/src/ebcdic.rs)、[外部対応表](../../crates/yy-encoding/src/mapfile.rs)を参照してください。
