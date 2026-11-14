---
description: コンパイラのソースの構成・ビルドの仕方・実装の決まり
audience: コンパイラのソースを変更する人
---

# ソースの構成とビルド
コンパイラの2つのツール([pynesis/compiler/usage.md](../compiler/usage.md)を参照)は，どちらもリポジトリ直下でC++17でコンパイルして作る．  
`pn2mc.exe`はアセンブラのソースも使うため，`pynesis`リポジトリ(`compiler/`)と`pyntaxis`リポジトリ(`assembler/`)を同じディレクトリの下に並べて置く．  

## pn2asm.exe
Pynesisのソースをアセンブリへ変換する処理は，`lexer.cpp`・`parser.cpp`・`analyzer.cpp`・`generator.cpp`と，入口の`pn2asm.cpp`から成る．  

```
g++ -Wall -Wextra -std=c++17 -o pn2asm.exe lexer.cpp parser.cpp analyzer.cpp generator.cpp pn2asm.cpp
```

## pn2mc.exe
`pn2mc.exe`は，次の2つの本処理を1つのプログラムにリンクし，順に呼び出す．  
別のプロセスとして起動はしない．  

- `pn2asm.cpp`の`compile_pn_to_asm`: Pynesisのソースからアセンブリへの変換  
- アセンブラの`../assembler/asm2mc.cpp`の`assemble_asm_to_mc`: アセンブリからROMまたは実行ファイルへの変換  

ビルドでは，`pn2asm.cpp`と`asm2mc.cpp`のそれぞれの`main`を無効にするマクロ`PN2ASM_NO_MAIN`・`ASM2MC_NO_MAIN`を指定し，両者の本処理のソースを`pn2mc.cpp`と一緒にコンパイルする．  

```
g++ -std=c++17 -DPN2ASM_NO_MAIN -DASM2MC_NO_MAIN -o pn2mc.exe pn2mc.cpp pn2asm.cpp lexer.cpp parser.cpp analyzer.cpp generator.cpp ../assembler/asm2mc.cpp
```

# 実装の決まり
- コーディング規約: for-pynthesis-skillsプラグインのコーディング規約に従う  
- エラー: `throw std::string("compiler error: <メッセージ>")`で投げる  
- 参考にするコード: アセンブラ(`../assembler/`)のコードを，実装の参考にする  
