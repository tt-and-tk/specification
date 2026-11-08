# 出力先による違い
アセンブリは，次のどちらかの出力先へ変換する．  
どちらにするかはアセンブラの起動引数で指定する([pyntaxis/assembler/usage.md](../assembler/usage.md)を参照)．  

- ROM: CPUに組み込むROMとして置き，リセット後に`main`から実行する  
- 実行ファイル: Qosmosの実行ファイルとしてSDカードに置き，シェルから実行する([qosmos/program/executable.md](../../qosmos/program/executable.md)を参照)  

書き方は出力先によらず同じであり，次の2点だけが異なる．  

| | ROM | 実行ファイル |
|:-|:-|:-|
| `main`の終わり方 | `main`の`ret`で終える | Qosmosの終了のシステムコールで終える |
| 命令数の上限 | 16384 | 4064 |

命令数が上限を超える場合は，アセンブルがエラーになる．  

# mainの終わり方
## ROM
`main`の`ret`は，プログラムの終了を表す．  
`main`の`ret`を実行すると，その命令自身へ飛ぶことを繰り返し，以降の命令へ進まない．  
CPUは動き続けており，CPUの停止([qurge/cpu/exception.md](../../qurge/cpu/exception.md)の「停止と復帰」を参照)とは異なる．  

## 実行ファイル
`main`の`ret`で実行を終えることはできない．  
シェルは`main`を`call`せずに実行し始めるため，`main`には戻り先が無い．  

実行ファイルは，Qosmosの終了のシステムコールで終える([qosmos/program/syscall.md](../../qosmos/program/syscall.md)を参照)．  
OSはこのシステムコールから実行ファイルへ戻らず，シェルへ戻る．  
どの関数にも`ret`が要るため，`main`にも`ret`を置く．  
Pynesisのコンパイラは，`main`の終わりで終了のシステムコールを呼び，その後に`ret`を置く．この`ret`は実行されない．  

# 命令数の上限
- ROM: ROMの容量で決まる([qurge/rom.md](../../qurge/rom.md)の「容量と命令数の上限」を参照)  
- 実行ファイル: 命令を置ける32512バイトに収まる命令数である．1命令は8バイトを占める  
    - シェルは，実行ファイルに32768バイトの領域を割り振る([qosmos/kernel/memory.md](../../qosmos/kernel/memory.md)の「実行ファイルの領域」を参照)  
    - 領域の末尾の256バイトは，シェルがコマンド名と引数を置く引数の置き場である  
    - 命令を置けるのは，引数の置き場より手前の32512バイトである  
