---
description: コンパイラの各ツールの起動の仕方と起動引数，エラーの出力
audience: コンパイラを使う人
---

# コンパイラの使い方
コンパイラは，Pynesisのソースファイル(`.pn`．[pynesis/language/overview.md](../language/overview.md)を参照)を，ROMとして埋め込むSystemVerilogのソース(`.sv`)，またはQosmosの実行ファイルへ変換する．  
ふだんは`pn2mc.exe`を使う．  

```
pn2mc.exe -pn program.pn -pt program.pt -sv program.sv    # ROMを出力する
pn2mc.exe -pn hello.pn -pt hello.pt -bin HELLO            # 実行ファイルHELLOを出力する
```

# ツールの構成
| ツール | 変換する範囲 |
|:-|:-|
| `pn2mc.exe` | Pynesisのソース(`.pn`) → SystemVerilogのROM(`.sv`)またはQosmosの実行ファイル．途中でアセンブリ(`.pt`)も出力する |
| `pn2asm.exe` | Pynesisのソース(`.pn`) → アセンブリ(`.pt`) |
| `asm2mc.exe`(アセンブラ．pyntaxisリポジトリで作る) | アセンブリ(`.pt`) → SystemVerilogのROM(`.sv`)またはQosmosの実行ファイル．起動引数は[pyntaxis/assembler/usage.md](../../pyntaxis/assembler/usage.md)の「起動引数」を参照 |

- `pn2mc.exe`は，`pn2asm.exe`とアセンブラの本処理を，それぞれ`main`から分けた関数として直接リンクし，順に呼び出す．別のプロセスは起動しない  
- `pn2asm.exe`・`asm2mc.exe`は，単体のツールとしても動く  

# pn2mc.exeの起動引数
| フラグ | 内容 | 省略時 |
|:-|:-|:-|
| `-pn` | 入力するPynesisのソースファイル名(`.pn`) | 省略できない |
| `-pt` | 途中で出力するアセンブリのファイル名(`.pt`) | 省略できない |
| `-sv` | 出力するSystemVerilogのROMのファイル名(`.sv`) | 「出力先の指定」を参照 |
| `-bin` | 出力するQosmosの実行ファイルの名前．実行ファイル用のアセンブリを生成してから，実行ファイルに変換する | 「出力先の指定」を参照 |

## 出力先の指定
- 出力先は，`-sv`と`-bin`のどちらか一方で指定する．両方指定した場合はエラーになる  
- どちらも省略した場合は，`-pt`の拡張子を`.sv`に変えた名前の`.sv`ファイルを出力する．`asm2mc.exe`を単体で使う場合と同じ決め方である  
- `-bin`に指定する名前の規則は，`asm2mc.exe`の`-bin`と同じである([pyntaxis/assembler/usage.md](../../pyntaxis/assembler/usage.md)の「実行ファイルとして出力するファイル(-bin)」を参照)  

## 引数の検査
引数は，変換を始める前にまとめて検査する．  
誤りがあれば，途中のアセンブリのファイルを書き出さずにエラーにする．  

- 指定子を付けない引数は受け付けない．`pn2asm.exe`は入力のPynesisのソース名，`asm2mc.exe`はアセンブリ名と解釈が異なるためである  
- 表にない指定子と，値のない指定子は受け付けない．単体の各ツールでは黙って無視され，打ち間違いに気づけないためである  
- `-pn`・`-pt`・`-sv`・`-bin`を複数回指定した場合は，`pn2asm.exe`・`asm2mc.exe`と同じく，最後に指定した値を使う  

# pn2asm.exeの起動引数
```
pn2asm.exe -pn program.pn -pt program.pt               # ROM用のアセンブリを出力する
pn2asm.exe -pn program.pn -pt program.pt --bin-mode    # 実行ファイル用のアセンブリを出力する
```

| フラグ | 内容 | 省略時 |
|:-|:-|:-|
| `-pn` | 入力するPynesisのソースファイル名(`.pn`)．必須 | - |
| `-pt` | 出力するアセンブリのファイル名(`.pt`) | `-pn`の拡張子を`.pt`に変えた名前 |
| `--bin-mode` | 値を取らない．Qosmosの実行ファイル用のアセンブリを出力する | ROM用のアセンブリを出力する |

同じフラグ(`-pn`・`-pt`)や指定子なしの引数を複数回指定した場合は，エラーにはならず，最後に指定した値を使う．  

## pn2asm.exeとasm2mc.exeで実行ファイルを作る場合
`pn2asm.exe`に`--bin-mode`を指定してアセンブリを生成し，`asm2mc.exe`の`-bin`で変換する．  
アセンブリは出力先の情報を持たない．  
このため，`--bin-mode`を付けずに生成したアセンブリを`-bin`で変換しても検出されない．  
`--bin-mode`を必ず付ける．  

# 入力と出力
- 入力: ソースファイル(`.pn`)．`#include`で取り込んだファイル([pynesis/language/include.md](../language/include.md)を参照)と合わせて，1つのプログラムとしてコンパイルする  
- 出力: アセンブリのファイル(`.pt`)．コード生成より前の段階でコンパイルエラーになった場合は作らない  

# コンパイルエラー
ソース上の位置を特定できるコンパイルエラーは，次の形で出力する．  

```
compiler error: <内容> at <ファイル名>:<行番号>
```
