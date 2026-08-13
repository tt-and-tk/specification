# ファイル種別
## svh
- util 特に分類できない
- ram メインメモリに関する関数などを定義
- machine 機械語を生成する関数群を定義
- rom ROM関係の関数などを定義
- decoder デコーダー用の関数などを定義
- alu aluに関するデータ型宣言など

## sv
回路の実体を格納するファイル群．

- mother_board_sv マザーボードのSystem Verilogソース．ram_sv・cpu_sv・rom_svをインスタンス化．
- ram_sv メインメモリ
- cpu_sv QurgeのCPUのラッパ．decoderとaluをインスタンス化．
- rom_sv ROM
- decoder_sv デコーダー．機械語を分解してレジスタ番号などを取得する
- alu_sv alu．次段命令の先読みデコード用にdecoderをもう一つインスタンス化．

## v
VivadoのブロックデザインはSystemVerilogで作ったモジュールに対応していないため，svファイルのモジュールをブロックデザインへ組み込むために必要なファイル群．内部でsvファイルのモジュールをインスタンス化するのみで，独自の回路ロジックは持たない．

- mother_board マザーボード．mother_board_sv(sv)をインスタンス化し，ブロックデザインへ組み込むためのラッパー．

# 階層構造
上記ファイル群のインスタンス化の親子関係．ブロックデザインに直接組み込まれるのはmother_board(v)のみで，他のsvモジュールはその内部で間接的にインスタンス化される．

```
mother_board (v)
└─ mother_board_sv (sv)
    ├─ ram_sv (sv)
    ├─ cpu_sv (sv)
    │   ├─ decoder_sv (sv)
    │   └─ alu_sv (sv)
    │       └─ decoder_sv (sv)
    └─ rom_sv (sv)
```

# 識別子命名規則
- パッケージ: `xx_p`
- 列挙体: `xx_enum`
- インターフェース: `xx_if`
- 変数の型: `xx_t`
