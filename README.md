# Pynthesis 仕様書

自作パソコンのプロジェクトPynthesisの仕様をまとめたものであり，各リポジトリの実装が参照する唯一の一次情報源である．

## 名称について

このプロジェクト全体および構成要素には，PYNQ-Z2ボードを使うことにちなみ「pynq」を共通の要素として組み込んだ，ギリシャ語由来の造語を付けている．

| 名称 | 対象 | 由来 |
|:-|:-|:-|
| Pynthesis(ピンセシス) | 自作パソコンのプロジェクト全体 | pynq + 「組み上げる」を意味するギリシャ語(synthesis) |
| Qurge(クージ) | CPU・メモリ・ROM等のハードウェア | pynq + 「自らの技術で材料から物を作り上げる職人」を意味するDemiurge |
| Pynesis(ピネシス) | 自作プログラミング言語 | pynq + 「まだないものを生み出す行為」を意味するギリシャ語(poiesis) |
| Pyntaxis(ピンタクシス) | 自作アセンブリ言語 | pynq + 「順序立てて並べる」を意味するギリシャ語(syntaxis) |
| Qosmos(コスモス) | 自作OS．シェルやファイルシステムなど，ROM上で動くソフトウェアの一式 | pynq + 「秩序ある世界」を意味するギリシャ語(kosmos)．Demiurgeが混沌から秩序ある世界を形作ることにちなみ，Qurgeが動かす世界として同じくpynqのqを冠する |

各名称に対応するリポジトリ:

- Pynthesis: 自作パソコンのプロジェクト全体を表す用語のため対応するリポジトリなし
- Pynesis: [pynesis](https://github.com/tt-and-tk/pynesis)
- Pyntaxis: [pyntaxis](https://github.com/tt-and-tk/pyntaxis)
- Qurge: [qurge](https://github.com/tt-and-tk/qurge)
- Qosmos: 専用のリポジトリはなく，[qurge](https://github.com/tt-and-tk/qurge)の`mypc.srcs/pn/`に置く

## 各仕様

仕様は，構成要素ごとのディレクトリに分けて置く．

### Qurge(`qurge/`)

- [qurge/cpu/isa.md](qurge/cpu/isa.md) — 機械語命令セットの仕様
- [qurge/cpu/register.md](qurge/cpu/register.md) — CPUレジスタ・I/Oマップされた周辺レジスタ・フラグの仕様
- [qurge/cpu/mode.md](qurge/cpu/mode.md) — カーネルモードとユーザーモードの違いと，その切り替え
- [qurge/cpu/exception.md](qurge/cpu/exception.md) — CPUが例外として検出する条件と，例外の種類と番号，停止・例外ハンドラへ飛ぶ処理への振り分け，停止後・リセット後の挙動
- [qurge/cpu/pipeline.md](qurge/cpu/pipeline.md) — 命令を処理する段の構成と，2命令を同時に実行する条件(実行結果ではなくサイクル数に関わる仕様)
- [qurge/memory.md](qurge/memory.md) — メインメモリのインターフェース・アドレス空間・マスク・領域の配置等の仕様
- [qurge/rom.md](qurge/rom.md) — 命令を格納するROMの仕様
- [qurge/board.md](qurge/board.md) — PYNQ-Z2を中心とする実機の物理的な構成と，そこから生じる操作上の制約

### Pyntaxis(`pyntaxis/`)

- [pyntaxis/language.md](pyntaxis/language.md) — アセンブリ言語Pyntaxisの言語仕様
- [pyntaxis/assembler.md](pyntaxis/assembler.md) — アセンブラの出力先と起動引数

### Pynesis(`pynesis/`)

- [pynesis/language.md](pynesis/language.md) — 独自言語Pynesisの言語仕様
- [pynesis/compiler.md](pynesis/compiler.md) — コンパイラの出力先・メモリ配置・規約・起動引数

### Qosmos(`qosmos/`)

- [qosmos/boot.md](qosmos/boot.md) — 起動時の動作
- [qosmos/user/console.md](qosmos/user/console.md) — コンソール(telnet・UARTで接続した端末)と1行の入力
- [qosmos/user/command.md](qosmos/user/command.md) — 入力の解析・コマンド一覧・終了状態
- [qosmos/program/executable.md](qosmos/program/executable.md) — 実行ファイルの探し方・形式・実行の仕方
- [qosmos/program/syscall.md](qosmos/program/syscall.md) — システムコール
- [qosmos/resource/filesystem.md](qosmos/resource/filesystem.md) — ファイルシステム
- [qosmos/resource/network.md](qosmos/resource/network.md) — ネットワーク

### その他

- [limitations.md](limitations.md) — 一般的なCPUとの差分・非対応事項一覧
