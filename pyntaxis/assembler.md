# アセンブラについて
アセンブリ言語Pyntaxis([pyntaxis/language.md](language.md)を参照)で書かれたコードを，ROMとして埋め込むSystem Verilogのソース，またはQosmosの実行ファイルにする．

## 出力先
出力先によって，命令数の上限と`main`の`ret`の扱いが異なる．

| | ROM(`.sv`) | 実行ファイル |
|:-|:-|:-|
| 出力する内容 | 受け取ったPCに格納されている機械語を返すROMのモジュール．読み出しポートを2つ(`rom_read1`・`rom_read2`)持ち，それぞれが受け取った互いに独立なPCの機械語を，クロック同期で同時に返す．各命令は機械語を作る関数(`machine.svh`)の呼び出しとして書く | 各命令の機械語を先頭から8バイトずつ並べたもの(形式は[qosmos/program/executable.md](../qosmos/program/executable.md)を参照) |
| `main`の`ret` | 自分自身へ飛ぶ`jmp`に置き換える(プログラムの終了) | `ret`の命令にする([pyntaxis/language.md](language.md)の「ジャンプ系(J系)」を参照) |
| 命令数の上限 | 16384([qurge/rom.md](../qurge/rom.md)の「容量と命令数の上限」を参照) | 4064(シェルが割り振る32768バイトの領域のうち，末尾の引数の置き場(シェルがコマンド名と引数を置く256バイト)より手前に収まる命令数．[qosmos/program/executable.md](../qosmos/program/executable.md)の「領域」を参照) |

命令数が上限を超える場合はエラーになる．\
エラーになった場合は，出力ファイルを作らない．

## 起動引数

実行ファイル名は `asm2mc.exe`．mcは機械語(machine code)を表し，出力するファイルの種類によらず同じ名前を使う．

| フラグ | 内容 | 省略時 |
|:-|:-|:-|
| `-pt` | 入力アセンブリファイル名(`.pt`)．必須 | - |
| `-sv` | 出力SystemVerilog ROMファイル名(`.sv`) | `-pt`の拡張子を`.sv`に変えた名前を自動的に使う |
| `-bin` | 出力する実行ファイル名．ファイル名の部分(ディレクトリを除いた部分)は，Qosmosの名前の規則([qosmos/resource/filesystem.md](../qosmos/resource/filesystem.md)の「ファイル・ディレクトリの名前」を参照)に従い，かつ拡張子を持たない名前に限る(Qosmosは拡張子の無いファイルだけを実行ファイルとして探すため)．これに合わない名前はエラーになる | SystemVerilog ROMを出力する |

出力するファイルの種類は`-bin`の有無で決まる．出力ファイルを何も指定しない場合は，SystemVerilog ROMを出力する．実行ファイルを出力するには`-bin`の指定が必須である．`-sv`と`-bin`を両方指定した場合はエラーになる．

例: `asm2mc.exe -pt program.pt -sv program.sv`\
例: `asm2mc.exe -pt hello.pt -bin HELLO`

同じフラグ(`-pt`／`-sv`／`-bin`)や指定子なし引数を複数回指定した場合，エラーにはならず最後に指定した値で上書きされる(後勝ち)．
