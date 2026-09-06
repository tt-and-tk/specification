全体像

## 名称について

このプロジェクト全体および構成要素には，PYNQ-Z2ボードを使うことにちなみ「pynq」を共通の要素として組み込んだ，ギリシャ語由来の造語を付けている．

| 名称 | 対象 | 由来 |
|:-|:-|:-|
| Pynthesis(ピンセシス) | 自作パソコン全体 | pynq + 「組み上げる」を意味するギリシャ語(synthesis) |
| Qurge(クージ) | CPU・メモリ・ROM等のハードウェア | pynq + 「自らの技術で材料から物を作り上げる職人」を意味するDemiurge |
| Pynesis(ピネシス) | 自作プログラミング言語 | pynq + 「まだないものを生み出す行為」を意味するギリシャ語(poiesis) |
| Pyntaxis(ピンタクシス) | 自作アセンブリ言語 | pynq + 「順序立てて並べる」を意味するギリシャ語(syntaxis) |

各名称に対応するリポジトリ:

- Pynthesis: 自作パソコン全体を表す用語のため対応するリポジトリなし
- Pynesis: [pynesis](https://github.com/tt-and-tk/pynesis)
- Pyntaxis: [pyntaxis](https://github.com/tt-and-tk/pyntaxis)
- Qurge: [qurge](https://github.com/tt-and-tk/qurge)

## 各仕様

1. [メモリ](./memory.md) — メインメモリのインターフェース・アドレス空間・マスク等の仕様
1. [レジスタ](./register.md) — CPUレジスタ・I/Oマップされた周辺レジスタ・フラグの仕様
1. [ROM](./rom.md) — 命令を格納するROMの仕様
1. [ISA](./isa.md) — 機械語命令セットの仕様
1. [アセンブラ](./assembler.md) — アセンブリ言語Pyntaxisの言語仕様とアセンブラツールの仕様
1. [コンパイラ](./compiler.md) — 独自言語Pynesisの言語仕様とコンパイラツールの仕様
1. [一般的なCPUとの差分・非対応事項](./limitations.md) — 一般的なCPUとの差分・非対応事項一覧
1. [回路](./circuit.md) — ハードウェア実装(SystemVerilog)のファイル構成・階層構造・命名規則
