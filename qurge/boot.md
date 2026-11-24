---
description: 電源を入れてからCPUが動き出すまでの流れ・動き出した後もPSが受け持つ役割・SRSTによる起動のやり直し
---

# 起動の仕組み
PYNQ-Z2のZynqは，ARMのプロセッサを持つPS(Processing System)と，回路を書き込めるPL(Programmable Logic)から成る．QurgeのCPUはPLに書き込んだ回路であり，PSはQurgeを起動するために使う．この文書でCPUと書く場合は，QurgeのCPUを指す．  

# 電源を入れたときの流れ
電源を入れると，ボードのSDカードスロットに挿したカードのBOOT.BINから起動する．BOOT.BINには，次の2つだけを入れる．  

- FSBL(First Stage Boot Loader): PSが最初に実行するプログラム  
- ビットストリーム: PLに書き込む回路の情報  

起動は，次の順に進む．  

1. PSが，BOOT.BINのFSBLを実行する  
2. FSBLが，PSを初期化する．通信速度などのUART0(USBのシリアル通信を行うPSの回路)の設定も，このときに行う  
3. FSBLが，BOOT.BINのビットストリームをPLへ書き込む  
4. CPUがクロックとリセットを受け取り，ROMの先頭から実行を始める  

BOOT.BINは，PSで動かすプログラムを持たない．このため，FSBLはビットストリームを書き込んだ後，PSを待機させて終わる．FSBLが終わった後にプログラムを実行するのは，CPUだけである．  

CPUが動き出してからFSBLが終わるまでの間も，FSBLはUART0へ出力しうる．このため，UARTで接続した端末では，FSBLの出力がQosmosの起動の行と混ざることがある([qosmos/user/boot.md](../qosmos/user/boot.md)の「起動の行が届く端末」を参照)．  

# 動き出した後もPSが受け持つ役割
PLへ書き込んだ後もPSが受け持つのは，次の2つである．  

- CPUへのクロックとリセットの供給: PSを初期化して初めて供給される  
- UART0によるUSBのシリアル通信  

## UARTの橋渡し回路
ボードの電源用のUSBは，PSのUART0にだけつながっている．このため，PL上のUARTの橋渡し回路が，PSのS_AXI_GP0ポートを通してUART0のレジスタを読み書きする．S_AXI_GP0ポートは，PLの回路がPS内の周辺回路のレジスタを読み書きするための入口である．CPUは，橋渡し回路をレジスタ`6'h31`で操作する([qurge/cpu/register.md](cpu/register.md)を参照)．  

# SRSTによる起動のやり直し
ボードのSRSTのボタン([board.md](../board.md)の「SRSTのボタン」を参照)は，PSをリセットするボタンである．CPUのリセットは，CPUをプログラムの先頭から実行し直させる信号であり，PSから入る．  

SRSTを押すと，PSが起動からやり直し，「電源を入れたときの流れ」と同じ手順で，PLを書き込み直した後にCPUがリセットを受け取ってROMの先頭から実行し直す．PLを書き込み直す間は，PLの書き込みの完了を示すDONEのLEDが消える．  

SRSTを押すと，次のものが起動時の状態に戻る．  

- CPUのレジスタ: CPUのリセットにより，初期値に戻る(各レジスタの初期値は[qurge/cpu/register.md](cpu/register.md)を参照)  
- メモリの内容: PLを書き込み直すため，ビットストリームが持つ初期値に戻る([qurge/memory.md](memory.md)の「リセット後の内容」を参照)  
- TCPの接続: ROMの先頭から実行し直したQosmosがEthernetモジュールをリセットするため，切れる([qosmos/kernel/boot.md](../qosmos/kernel/boot.md)の「ネットワークの項目」を参照)  
