全体像

入力(独自言語Pynesis, .pn) → コンパイラ → アセンブリ言語Pyntaxis(.pt) → アセンブラ → SystemVerilog ROM(.sv) → Vivado → PYNQ-Z2上のハードウェア(Qurge)で実行，という流れでプログラムが動作する．

1. [名称について](./naming.md) — プロジェクト・構成要素の名称とその由来
1. [メモリ](./memory.md) — メインメモリのインターフェース・アドレス空間・マスク等の仕様
1. [レジスタ](./register.md) — CPUレジスタ・I/Oマップされた周辺レジスタ・フラグの仕様
1. [ROM](./rom.md) — 命令を格納するROMの仕様
1. [ISA](./isa.md) — 機械語命令セットの仕様
1. [アセンブラ](./assembler.md) — アセンブリ言語Pyntaxisの言語仕様とアセンブラツールの仕様
1. [コンパイラ](./compiler.md) — 独自言語Pynesisの言語仕様とコンパイラツールの仕様
1. [通常のCPUとの差分・非対応事項](./limitations.md) — 一般的なCPUとの差分・非対応事項一覧
1. [回路](./circuit.md) — ハードウェア実装(SystemVerilog)のファイル構成・階層構造・命名規則
