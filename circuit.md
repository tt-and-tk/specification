# ファイル種別
## svh
svファイルで使用する型・定数・enum・interface等の定義をまとめたファイル群であり，必要とするsvファイルがある場合にのみ存在する(複数のsvファイルから共有されることもある)．

- util 特に分類できない
- ram メインメモリに関する関数などを定義
- machine 機械語を生成する関数群を定義
- rom ROM関係の関数などを定義
- decoder デコーダー用の関数などを定義
- alu aluに関するデータ型宣言など

## sv
回路の実体を格納するファイル群．

- mother_board_sv マザーボードのSystem Verilogソース．ram_sv・cpu_sv・rom_svをインスタンス化し，各インターフェース(ram_read/ram_write/rom_read等)を介して接続する
- ram_sv メインメモリ．データをBlock RAMへ保持し，読み込み・書き込みインターフェース経由でアクセスを提供する
- cpu_sv QurgeのCPUのラッパ．decoderとaluをインスタンス化
- rom_sv ROM．プログラム(機械語列)を格納し，受け取ったpcに対応する機械語をクロック同期で返す
- decoder_sv デコーダー．機械語を分解してレジスタ番号などを取得する
- alu_sv ALU．レジスタファイルを保持し，命令のフェッチ・デコード結果に基づいて演算・分岐・メモリアクセス等を実行する

## v
ブロックデザイン上で使用する．svファイルのラッパとしても使用される(svファイルはブロックデザインに直接配置できないため)．

- mother_board マザーボード．CPUなど各ハードウェアを全て載せる
- seven_seg デバッグ用の7セグメントディスプレイ表示モジュール．自作PC本体とは無関係な開発時限定の使用

# 階層構造

```
mother_board (v)
└─ mother_board_sv (sv)
    ├─ ram_sv (sv)
    ├─ cpu_sv (sv)
    │   ├─ decoder_sv (sv)
    │   └─ alu_sv (sv)
    │       └─ decoder_sv (sv)  // 次段命令の先読みデコード用
    └─ rom_sv (sv)

seven_seg (v)  // デバッグ用．他モジュールとの親子関係なし
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
