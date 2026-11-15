---
description: QosmosのPynesisのソースの構成と，ソースからROMの内容を生成する方法
audience: Qosmosのソースを変更する人
---

# ソースの構成とROMの生成
QosmosはPynesisで書き，`qurge`リポジトリの`mypc.srcs/pn/`に置く．  
QosmosはROMに入れて動かすため，ソースを変更したら，ROMの内容を生成し直してから合成する．  

# ソースの構成
- 入口: `qosmos.pn`  
- `lib/`: Qosmosの実行ファイルのソースが取り込むライブラリ．このうち，番号などの取り決めを置くファイルはシェルも取り込み，ROMに入る．それ以外のファイルはROMに入らない  

# ROMの生成
ROMの内容(`mypc.srcs/sources_1/new/rom_sv.sv`)は，Qosmosのソースから生成したものを使う．  
Qosmosの変更は，`rom_sv.sv`ではなく`.pn`に対して行う．  
検証などのため一時的に，Qosmosのソース以外から作ったROMの内容を置いてもよい．  
ただし，`.pn`から生成し直すと上書きされるため，コミットしない．  

Vivadoはこの変換を行わず，生成済みの`rom_sv.sv`を，他のHDLのソースと同じく読み込むだけである．  
このため，`rom_sv.sv`を生成し直してから合成する必要があるのは，次のときである．  

- `mypc.srcs/pn/`の下を変更したとき  
- コンパイラ・アセンブラを更新して，生成結果が変わりうるとき  

`rom_sv.sv`は，リポジトリ直下で次のコマンドを実行して生成する．  

```
pn2mc.exe -pn mypc.srcs/pn/qosmos.pn -pt mypc.srcs/pn/qosmos.pt -sv mypc.srcs/sources_1/new/rom_sv.sv
```

`pn2mc.exe`は`pynesis`リポジトリでビルドし([pynesis/develop/source.md](../../pynesis/develop/source.md)を参照)，そのディレクトリにPATHを通しておく．  
起動引数は，[pynesis/compiler/usage.md](../../pynesis/compiler/usage.md)を参照．  
`-sv`は必ず指定する．  
省略すると`.pt`と同じ場所へ出力され，Vivadoが読む`rom_sv.sv`が古いまま残るためである．  

生成し直した`rom_sv.sv`に差分が出たら，コミットする．  
`.pn`の変更によるものは，その変更と同じコミットに含める．  
コンパイラ・アセンブラの更新によるものは，単独でコミットする．  
チェックアウトした直後に変換ツールなしで合成できるよう，`rom_sv.sv`もGit管理の対象としているためである．  
