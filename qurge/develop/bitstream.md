---
description: 合成したビットストリームからBOOT.BINを作る方法と，開発中にJTAGで書き込む方法
audience: Qurgeのソースを変更する人
---

# ビットストリームの書き込み
合成したビットストリームをボードへ書き込む方法は，次の2つである．  

- BOOT.BIN: SDカードに置き，電源を入れると書き込まれる．BOOT.BINの役割は[qurge/boot.md](../boot.md)を参照  
- JTAG: 動作を確かめる段階で，BOOT.BINを作り直さずに書き込む  

# BOOT.BINの作成
VivadoとVitis(版はVivadoと揃える)のbinディレクトリ(`vivado`・`xsct`・`bootgen`)にPATHを通し，コマンドプロンプトから次を行う．出力は，リポジトリ直下の`mypc.boot/`(Git管理の対象外)に置かれる．  

1. Vivadoで`Generate Bitstream`を実行する  
2. 最初の1回と，ブロックダイアグラムでPS(`processing_system7_0`)の設定を変えたときは，`mypc.srcs\boot\make_fsbl.bat`を実行してFSBLを作る．FSBLはPSの設定だけに依存するため，自作のHDLやROMの内容を変えただけなら作り直さなくてよい  
3. `mypc.srcs\boot\make_boot.bat`を実行すると，`mypc.boot\BOOT.BIN`ができる  

BOOT.BINを置くSDカードの用意の仕方は，[operation.md](../../operation.md)の「SDカード」を参照．  

# 開発中の書き込み
BOOT.BINを置いたカードで一度起動した後に，VivadoのHardware ManagerからJTAG(USB)でビットストリームを書き込んで上書きしてよい．ビットストリームを作り直すたびにBOOT.BINを作り直さずに済む．  

先にBOOT.BINで起動しておくのは，JTAGではPSを初期化しないためである．CPUのクロックとリセットは，FSBLがPSを初期化して初めて供給される．  

JTAGで書き込んだ回路は，SRSTのボタンを押すか電源を入れ直すと，BOOT.BINの回路に戻る．どちらもFSBLがBOOT.BINからPLを書き込み直すためである([qurge/boot.md](../boot.md)の「SRSTによる起動のやり直し」を参照)．Qosmosの`reboot`コマンド([qosmos/user/command.md](../../qosmos/user/command.md)の「reboot」を参照)はPSをリセットしないため，PLは書き込み直されない．このため，JTAGで書き込んだ回路のままQosmosを起動し直すには，`reboot`コマンドを使う．  
