---
description: ROMと実行ファイルの出力先で異なる点(mainの終わり方・大きさの宣言・命令数の上限)
audience: Pyntaxisでプログラムを書く人
---

# 出力先による違い
アセンブリは，次のどちらかの出力先へ変換される．どちらにするかはアセンブラの起動引数で指定する([pyntaxis/assembler/usage.md](../assembler/usage.md)を参照)．  

- ROM: CPUに組み込むROMとして置き，リセット後に`main`から実行する  
- 実行ファイル: Qosmosの実行ファイルとしてSDカードに置き，シェルから実行する([qosmos/program/executable.md](../../qosmos/program/executable.md)を参照)  

書き方は出力先によらず同じであり，次の3つだけが異なる．  

| 項目 | ROM | 実行ファイル |
|:-|:-|:-|
| `main`の終わり方 | `ret`で終える | 終了のシステムコールで終える |
| 大きさの宣言 | 書けない | `.reserve`を書き，`.stack`を書ける |
| 命令数の上限 | 16384命令 | 4064命令 |

# mainの終わり方
## ROM
`main`の`ret`は，プログラムの終了を表す．`main`の`ret`を実行すると，その命令自身へ飛ぶことを繰り返し，以降の命令へ進まない．CPUは動き続けており，CPUの停止([qurge/cpu/exception.md](../../qurge/cpu/exception.md)の「停止と復帰」を参照)とは異なる．  

## 実行ファイル
実行ファイルは，Qosmosの終了のシステムコールで終える([qosmos/program/syscall.md](../../qosmos/program/syscall.md)を参照)．シェルは`main`を呼び出さずに実行し始めるため，`main`の`ret`では終えられない．  

# 大きさの宣言
## ROM
大きさの宣言は書けない．書くとアセンブルがエラーになる．ROMのプログラムは，Qosmosが割り振る領域を使わないためである．  

## 実行ファイル
シェルが割り振る領域に必要な大きさを，`.global`宣言と最初の関数ラベルの間に，次の順で宣言する．アセンブラは，宣言した値を実行ファイルのヘッダに書く([qosmos/program/executable.md](../../qosmos/program/executable.md)の「形式」を参照)．  

```
.global main
.reserve 16     ; 命令列の直後に確保する範囲のバイト数
.stack 64       ; スタックの量のバイト数(省略できる)
```

| 宣言 | 書く値 | 省略した場合 |
|:-|:-|:-|
| `.reserve <数値>` | 命令列(命令の機械語の並び．[qosmos/program/executable.md](../../qosmos/program/executable.md)の「形式」を参照)の直後から，固定の番地で読み書きする範囲のバイト数．使わなければ0 | アセンブルがエラーになる |
| `.stack <数値>` | スタックを最も深く積んだときのバイト数 | スタックの量は未定になる．シェルとアセンブラは，4096バイトとみなして大きさを確かめる |

各宣言は1回ずつ書き，`.stack`は`.reserve`より後に書く．数値の書き方は，イミディエイトデータと同じである([pyntaxis/language/program.md](program.md)の「数値の書き方」を参照)．ただし，負の値は書けず，32ビットで表せる値に限る．  

命令列(1命令8バイト)・`.reserve`の値・`.stack`の値の合計が，32512バイトを超える場合は，アセンブルがエラーになる．`.stack`を省略した場合は，4096バイトとして合計する．32512バイトは，シェルが割り振る領域の大きさから決まる([qosmos/kernel/memory.md](../../qosmos/kernel/memory.md)の「実行ファイルの領域」を参照)．  

# 命令数の上限
命令数が次の上限を超える場合は，アセンブルがエラーになる．  

- ROM: 16384命令．ROMの容量で決まる([qurge/rom.md](../../qurge/rom.md)の「容量と命令数の上限」を参照)  
- 実行ファイル: 4064命令．シェルが実行ファイルに割り振る領域の大きさで決まる([qosmos/kernel/memory.md](../../qosmos/kernel/memory.md)の「実行ファイルの領域」を参照)  
