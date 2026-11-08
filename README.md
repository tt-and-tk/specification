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
各文書は扱う対象を1つに定め，その対象を使う側の事情(どの番地に何を置くかなど)は，使う側の文書に書く．  
言語とOSの仕様は，使う人向けの文書と，仕組みを知りたい・変えたい人向けの文書を分けて置く．  
実機を使う手順は，[operation.md](operation.md)にまとめる．  

```
specification/
├── operation.md            実機の操作手順
├── qurge/                  ハードウェア
│   ├── board.md            実機の構成
│   ├── memory.md           メインメモリ
│   ├── rom.md              ROM
│   └── cpu/                CPU
│       ├── isa.md          命令の形式と各命令
│       ├── stack.md        スタック
│       ├── register.md     レジスタ
│       ├── mode.md         動作状態
│       ├── exception.md    例外
│       └── pipeline.md     パイプライン
├── pyntaxis/               アセンブリ言語Pyntaxis
│   ├── language/           言語の書き方
│   │   ├── program.md      プログラムの書き方
│   │   ├── instruction.md  命令
│   │   ├── label.md        局所ラベル
│   │   └── target.md       出力先による違い
│   └── assembler/          アセンブラ
│       ├── usage.md        使い方
│       └── encoding.md     機械語への変換
├── pynesis/                プログラミング言語Pynesis
│   ├── language/           言語の仕様
│   │   ├── overview.md     概要と文書の構成
│   │   ├── lexical.md      識別子・リテラル・コメント
│   │   ├── type.md         データ型・型変換・キャスト
│   │   ├── variable.md     変数・const変数・定数式
│   │   ├── array.md        配列と文字列
│   │   ├── struct.md       構造体
│   │   ├── pointer.md      ポインタと関数ポインタ
│   │   ├── operator.md     演算子とsizeof
│   │   ├── control.md      制御構文
│   │   ├── function.md     関数とmain
│   │   ├── include.md      ソースファイルの取り込み
│   │   ├── builtin.md      組み込み関数
│   │   ├── hardware.md     ハードウェア変数
│   │   ├── limit.md        量的制限
│   │   └── unsupported.md  未対応機能
│   └── compiler/           コンパイラ
│       ├── usage.md        使い方
│       ├── memory.md       変数の配置
│       ├── call.md         関数呼び出しの命令
│       └── check.md        量的制限の検査
└── qosmos/                 OS Qosmos
    ├── user/               利用者向け
    │   ├── boot.md         起動時の表示
    │   ├── console.md      コンソール
    │   ├── shell.md        シェル
    │   ├── command.md      組み込みコマンド
    │   └── file.md         ファイルとディレクトリ
    ├── program/            実行ファイルを作る人向け
    │   ├── executable.md   実行ファイル
    │   └── syscall.md      システムコール
    └── kernel/             仕組みを変える人向け
        ├── boot.md         起動の手順
        ├── console.md      コンソールの仕組み
        ├── memory.md       メモリの割り振り
        ├── exec.md         実行ファイルを動かす仕組み
        ├── syscall.md      システムコールの受け付け
        ├── filesystem.md   ファイルシステムの仕組み
        └── network.md      ネットワーク
```

### Qurge(`qurge/`)

ハードウェアの仕様．CPUの仕様は`qurge/cpu/`に置く．  

- [qurge/board.md](qurge/board.md) — PYNQ-Z2を中心とする実機の物理的な構成  
- [qurge/memory.md](qurge/memory.md) — メインメモリのインターフェース・アドレス空間・マスク  
- [qurge/rom.md](qurge/rom.md) — 命令を格納するROMと，命令数の上限  
- [qurge/cpu/isa.md](qurge/cpu/isa.md) — 機械語命令セット(命令の形式，各命令の動作)  
- [qurge/cpu/stack.md](qurge/cpu/stack.md) — CALL/RETが戻り先を積み下ろしするスタック  
- [qurge/cpu/register.md](qurge/cpu/register.md) — CPUレジスタ・I/Oマップされた周辺レジスタ・フラグと，動作状態ごとの読み書きの可否  
- [qurge/cpu/mode.md](qurge/cpu/mode.md) — カーネルモードとユーザーモードの違いと，その切り替え  
- [qurge/cpu/exception.md](qurge/cpu/exception.md) — 例外の種類と条件，停止と復帰  
- [qurge/cpu/pipeline.md](qurge/cpu/pipeline.md) — 命令を処理する段の構成，例外を判定する段と，2命令を同時に実行する条件  

### Pyntaxis(`pyntaxis/`)

- [pyntaxis/language/program.md](pyntaxis/language/program.md) — プログラムの書き方  
- [pyntaxis/language/instruction.md](pyntaxis/language/instruction.md) — 命令の書き方  
- [pyntaxis/language/label.md](pyntaxis/language/label.md) — 局所ラベル  
- [pyntaxis/language/target.md](pyntaxis/language/target.md) — 出力先(ROM・実行ファイル)による違い  
- [pyntaxis/assembler/usage.md](pyntaxis/assembler/usage.md) — アセンブラの使い方と起動引数  
- [pyntaxis/assembler/encoding.md](pyntaxis/assembler/encoding.md) — 機械語への変換と出力の形式  

### Pynesis(`pynesis/`)

- [pynesis/language/overview.md](pynesis/language/overview.md) — 言語の概要・文書の構成・出力先による違い(言語仕様の各文書はここから辿る)  
- [pynesis/compiler/usage.md](pynesis/compiler/usage.md) — コンパイラの使い方と起動引数  
- [pynesis/compiler/memory.md](pynesis/compiler/memory.md) — 変数の配置  
- [pynesis/compiler/call.md](pynesis/compiler/call.md) — 関数呼び出しの命令とスタックの使い方  
- [pynesis/compiler/check.md](pynesis/compiler/check.md) — 量的制限の検査  

### Qosmos(`qosmos/`)

Qosmosは，CPUのROM上で動き，起動時にハードウェアを初期化してストレージとネットワークを使える状態にした後，シェルとしてコンソールから1行ずつコマンドを受け取り，対応する処理を実行する．`qosmos/`配下の各文書がQosmosの仕様であり，実装(`qurge`の`mypc.srcs/pn/qosmos.pn`と，そこから取り込むファイル)はこれに従う．  

- [qosmos/user/boot.md](qosmos/user/boot.md) — 起動時の表示と，失敗したときの対処  
- [qosmos/user/console.md](qosmos/user/console.md) — コンソール(telnet・UARTで接続した端末)と1行の入力  
- [qosmos/user/shell.md](qosmos/user/shell.md) — 入力の解析・現在のディレクトリ・終了状態  
- [qosmos/user/command.md](qosmos/user/command.md) — 組み込みコマンド  
- [qosmos/user/file.md](qosmos/user/file.md) — 扱えるカード・ディレクトリ・名前の規則と表示・ファイルの内容・書き込みについて  
- [qosmos/program/executable.md](qosmos/program/executable.md) — 実行ファイルの作り方・置き場所・引数と終了状態・使えるメモリ・実行できない場合・形式  
- [qosmos/program/syscall.md](qosmos/program/syscall.md) — システムコールの使い方  
- [qosmos/kernel/boot.md](qosmos/kernel/boot.md) — 起動の手順と各段階  
- [qosmos/kernel/console.md](qosmos/kernel/console.md) — UARTの送受信・telnetの接続と送信・telnetのオプション  
- [qosmos/kernel/memory.md](qosmos/kernel/memory.md) — メモリの割り振りと，実行ファイルの領域  
- [qosmos/kernel/exec.md](qosmos/kernel/exec.md) — 実行ファイルを動かす手順と引数の置き方  
- [qosmos/kernel/syscall.md](qosmos/kernel/syscall.md) — システムコールの受け付けと番地の検査  
- [qosmos/kernel/filesystem.md](qosmos/kernel/filesystem.md) — マウントと書き込みの仕組み  
- [qosmos/kernel/network.md](qosmos/kernel/network.md) — ネットワークの設定とTCP  
