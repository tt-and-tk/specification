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

- mother_board_sv マザーボードのSystem Verilogソース
- ram_sv メインメモリ
- cpu_sv QurgeのCPUのラッパ．decoderとaluをインスタンス化．
- rom_sv ROM
- decoder_sv デコーダー．機械語を分解してレジスタ番号などを取得する
- alu_sv alu

## v
ブロックデザイン上で使用するためのファイル群．モジュール定義そのもの，またはsvファイルで作成されたモジュールを内包するラッパー(VivadoのブロックデザインはSystemVerilogで作ったモジュールに対応していないため)として使用する．

- mother_board マザーボード．mother_board_sv(sv)をインスタンス化し，ブロックデザインへ組み込むためのラッパー．

# 階層構造
sv・vファイル群のインスタンス化の親子関係(svhはインスタンス化されないため対象外)．ブロックデザインに直接組み込まれるのはmother_board(v)のみで，他のsvモジュールはその内部で間接的にインスタンス化される．

```
mother_board (v)
└─ mother_board_sv (sv)
    ├─ ram_sv (sv)
    ├─ cpu_sv (sv)
    │   ├─ decoder_sv (sv)
    │   └─ alu_sv (sv)
    │       └─ decoder_sv (sv)  // 次段命令の先読みデコード用
    └─ rom_sv (sv)
```

# 識別子命名規則
- パッケージ: `xx_p`
- 列挙体: `xx_enum`
- インターフェース: `xx_if`
- 変数の型: `xx_t`

# ファイル名の命名規則
- svhファイル: 接尾辞なし(`xx.svh`)
- svファイル: `_sv`接尾辞(`xx_sv.sv`)
- vファイル: 接尾辞なし(`xx.v`)
