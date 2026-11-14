---
description: アセンブラのソースの構成とビルドの仕方
audience: アセンブラのソースを変更する人
---

# ソースの構成とビルド
アセンブラのソースは，入口の`asm2mc.cpp`と，`asm2mc.cpp`が取り込むヘッダから成る．  
`asm2mc_main.hpp`は，コンパイラの`pn2mc.exe`から呼び出す関数を宣言する．  

コンパイラの`pn2mc.exe`は，`asm2mc.cpp`を一緒にコンパイルして使う([pynesis/develop/source.md](../../pynesis/develop/source.md)の「pn2mc.exe」を参照)．  
このため，アセンブラのソースを変えると，`pn2mc.exe`のビルドと動作も変わりうる．  

アセンブラ`asm2mc.exe`は，リポジトリ直下で次を実行して作る．  

```
g++ -Wall -Wextra -std=c++17 -o asm2mc.exe asm2mc.cpp
```

起動引数は，[pyntaxis/assembler/usage.md](../assembler/usage.md)を参照．  
