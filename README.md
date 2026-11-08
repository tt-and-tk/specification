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

仕様は，構成要素ごとのディレクトリに分けて置く．各文書は扱う対象を1つに定めており，その対象を使う側の事情(どの番地に何を置くかなど)は，使う側の文書に書く．

実機を使う手順は，[operation.md](operation.md)にまとめる．

### Qurge(`qurge/`)

ハードウェアの仕様．CPUの仕様は`qurge/cpu/`に置く．

- [qurge/cpu/isa.md](qurge/cpu/isa.md) — 機械語命令セット(命令の形式，各命令の動作)
- [qurge/cpu/stack.md](qurge/cpu/stack.md) — CALL/RETが戻り先を積み下ろしするスタック
- [qurge/cpu/register.md](qurge/cpu/register.md) — CPUレジスタ・I/Oマップされた周辺レジスタ・フラグと，動作状態ごとの読み書きの可否
- [qurge/cpu/mode.md](qurge/cpu/mode.md) — カーネルモードとユーザーモードの違い(命令を取り出す場所，メモリを読み書きできる範囲，番地の変換)と，その切り替え
- [qurge/cpu/exception.md](qurge/cpu/exception.md) — CPUが例外として検出する条件と，例外の種類と番号，停止・例外ハンドラへ飛ぶ処理への振り分け，停止後・リセット後の挙動
- [qurge/cpu/pipeline.md](qurge/cpu/pipeline.md) — 命令を処理する段の構成，例外を判定する段と，2命令を同時に実行する条件(実行結果ではなくサイクル数に関わる仕様)
- [qurge/memory.md](qurge/memory.md) — メインメモリ(記憶装置)のインターフェース・アドレス空間・マスク
- [qurge/rom.md](qurge/rom.md) — 命令を格納するROMと，命令数の上限
- [qurge/board.md](qurge/board.md) — PYNQ-Z2を中心とする実機の物理的な構成

### Pyntaxis(`pyntaxis/`)

- [pyntaxis/language/program.md](pyntaxis/language/program.md) — プログラムの書き方
- [pyntaxis/language/instruction.md](pyntaxis/language/instruction.md) — 命令の書き方
- [pyntaxis/language/label.md](pyntaxis/language/label.md) — 局所ラベル
- [pyntaxis/language/target.md](pyntaxis/language/target.md) — 出力先(ROM・実行ファイル)による違い
- [pyntaxis/assembler/usage.md](pyntaxis/assembler/usage.md) — アセンブラの使い方と起動引数
- [pyntaxis/assembler/encoding.md](pyntaxis/assembler/encoding.md) — 機械語への変換と出力の形式

### Pynesis(`pynesis/`)

- [pynesis/language.md](pynesis/language.md) — 独自言語Pynesisの言語仕様
- [pynesis/compiler.md](pynesis/compiler.md) — コンパイラの出力先・メモリ配置・規約・量的制限・起動引数

### Qosmos(`qosmos/`)

Qosmosは，CPUのROM上で動き，起動時にハードウェアを初期化してストレージとネットワークを使える状態にした後，シェルとしてコンソールから1行ずつコマンドを受け取り，対応する処理を実行する．`qosmos/`配下の各文書がQosmosの仕様であり，実装(`qurge`の`mypc.srcs/pn/qosmos.pn`と，そこから取り込むファイル)はこれに従う．

文書は，起動時の動作と，次の3つの分類に分けて置く．

- `user/`: 利用者がコンソールから使う機能
- `program/`: 実行ファイルが使う機能
- `resource/`: 両方が使う，OSが管理する基盤

| 文書 | 内容 |
|:-|:-|
| [qosmos/boot.md](qosmos/boot.md) | 起動時の動作 |
| [qosmos/user/console.md](qosmos/user/console.md) | コンソール(telnet・UARTで接続した端末)と1行の入力 |
| [qosmos/user/command.md](qosmos/user/command.md) | 入力の解析・コマンド一覧・終了状態 |
| [qosmos/program/executable.md](qosmos/program/executable.md) | 実行ファイルの探し方・形式・実行の仕方 |
| [qosmos/program/syscall.md](qosmos/program/syscall.md) | システムコール |
| [qosmos/resource/memory.md](qosmos/resource/memory.md) | メモリの割り振りと，実行ファイルの領域 |
| [qosmos/resource/filesystem.md](qosmos/resource/filesystem.md) | ファイルシステム |
| [qosmos/resource/network.md](qosmos/resource/network.md) | ネットワーク |

