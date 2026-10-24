# 共有ライブラリ化の記録（2026-10-24）

YYSortとyyeditorの文字コード実装の複製を避けるため、`yy-encoding`と`gen-tables`をYYLibraryへ集約した。
利用側は完全SHAを指定したCargo Git依存とし、yymergeの直接依存と独立アプリのlockfileも更新した。
GPLと第三者の対応表の権利表記を維持した。

対象コミットは`57deca92c834227f705496fd7c5c4a226ec8dc97`。
Windows（Rust 1.92.0）とLinux（Rust 1.99.0）でfmt/Clippy、文字コードの31試験を確認した。
[当該コミットのCI](https://github.com/ykwyuta/YYLibrary/actions/runs/40075088308)も両OSで成功した。
これは当該時点の記録で、現在のCI結果・公開APIは各案内を参照する。
