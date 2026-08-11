# ファイル種別
## svh
- util 特に分類できない
- ram メインメモリに関する関数などを定義
- machine 機械語を生成する関数群を定義
- rom ROM関係の関数などを定義
- decoder デコーダー用の関数などを定義
- alu aluに関するデータ型宣言など

## sv
- mother_board_sv マザーボードのSystem Verilogソース
- ram_sv メインメモリ
- cpu_sv QurgeのCPUのラッパ．decoderとaluをインスタンス化．
- rom_sv ROM
- decoder_sv デコーダー．機械語を分解してレジスタ番号などを取得する
- alu_sv alu

## v
- mother_board マザーボード．PCの全体像．

# 識別子命名規則
- パッケージ: `xx_p`
- 列挙体: `xx_enum`
- インターフェース: `xx_if`
- 変数の型: `xx_t`
