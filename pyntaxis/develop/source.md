---
description: アセンブラのソースの構成とビルドの仕方
audience: アセンブラのソースを変更する人
---

# ソースの構成とビルド
アセンブラ`asm2mc.exe`は，`asm2mc.cpp`をC++17でコンパイルして作る．  
`asm2mc.cpp`の本処理は，コンパイラ`pn2mc.exe`も取り込んで使う([pynesis/develop/source.md](../../pynesis/develop/source.md)の「pn2mc.exe」を参照)．  
このため，本処理を変えると，`pn2mc.exe`の動作も変わる．  

リポジトリ直下で次を実行する．  

```
g++ -Wall -Wextra -std=c++17 -o asm2mc.exe asm2mc.cpp
```

起動引数は，[pyntaxis/assembler/usage.md](../assembler/usage.md)を参照．  
