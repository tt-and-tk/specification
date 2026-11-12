---
description: ROMと実行ファイルの出力先で異なる点(mainの終わり方と命令数の上限)
audience: Pyntaxisでプログラムを書く人
---

# 出力先による違い
アセンブリは，次のどちらかの出力先へ変換される．  
どちらにするかはアセンブラの起動引数で指定する([pyntaxis/assembler/usage.md](../assembler/usage.md)を参照)．  

- ROM: CPUに組み込むROMとして置き，リセット後に`main`から実行する  
- 実行ファイル: Qosmosの実行ファイルとしてSDカードに置き，シェルから実行する([qosmos/program/executable.md](../../qosmos/program/executable.md)を参照)  

書き方は出力先によらず同じであり，`main`の終わり方と命令数の上限だけが異なる．  

# mainの終わり方
## ROM
`main`の`ret`は，プログラムの終了を表す．  
`main`の`ret`を実行すると，その命令自身へ飛ぶことを繰り返し，以降の命令へ進まない．  
CPUは動き続けており，CPUの停止([qurge/cpu/exception.md](../../qurge/cpu/exception.md)の「停止と復帰」を参照)とは異なる．  

## 実行ファイル
実行ファイルは，Qosmosの終了のシステムコールで終える([qosmos/program/syscall.md](../../qosmos/program/syscall.md)を参照)．  
シェルは`main`を呼び出さずに実行し始めるため，`main`の`ret`では終えられない．  

# 命令数の上限
命令数が次の上限を超える場合は，アセンブルがエラーになる．  

- ROM: 16384命令．ROMの容量で決まる([qurge/rom.md](../../qurge/rom.md)の「容量と命令数の上限」を参照)  
- 実行ファイル: 4064命令．シェルが実行ファイルに割り振る領域の大きさで決まる([qosmos/kernel/memory.md](../../qosmos/kernel/memory.md)の「実行ファイルの領域」を参照)  
