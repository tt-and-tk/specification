# 起動時の表示
Qosmosは，電源を入れたとき(SRSTのボタンを押したときを含む．[qurge/board.md](../../qurge/board.md)の「SRSTのボタン」を参照)と，`reboot`を実行したときに起動する．  
起動すると，SDカードとEthernetモジュールを使える状態にし，その成否を1行ずつコンソールへ出力する．  
起動の手順の詳細は，[qosmos/kernel/boot.md](../kernel/boot.md)を参照．  

# すべて成功した場合
```
[ OK ] SD card: reset (CMD0)
[ OK ] SD card: voltage check (CMD8)
[ OK ] SD card: initialize (ACMD41)
[ OK ] SD card: card type SDHC/SDXC (CMD58)
[ OK ] FAT32: mount
[ OK ] Ethernet: reset
[ OK ] Ethernet: network settings (IP 192.168.10.200)
  ___
 / _ \  ___  ___ _ __ ___   ___  ___
| | | |/ _ \/ __| '_ ` _ \ / _ \/ __|
| |_| | (_) \__ \ | | | | | (_) \__ \
 \__\_\\___/|___/_| |_| |_|\___/|___/

   order out of chaos, on Qurge
> 
```

- `[ OK ]`の行は，その段階が成功したことを表す  
- 最後にOS名Qosmosを大きな文字で出力し，プロンプト`> `を表示してコマンドを受け付け始める  
- プロンプトを表示した後，telnetでの接続を受け付ける([qosmos/user/console.md](console.md)を参照)  

# 失敗した場合
失敗した段階は，`[FAIL]`で始まる行に理由を添えて出力する．  
失敗した段階より後の，同じ機器の段階は実行しない．  
SDカードとEthernetモジュールは互いに依存しないため，片方が失敗しても，もう片方の段階は実行する．  
どちらが失敗しても，OS名を出力してコマンドを受け付ける．  

```
[FAIL] SD card: reset (CMD0): no response (0xff)
[ OK ] Ethernet: reset
[ OK ] Ethernet: network settings (IP 192.168.10.200)
  (OS名)
Storage is not available. Insert a card and run reboot.
> 
```

- 理由の後の`(0x??)`は，判定に使った機器の応答の値である  
- SDカードまたはファイルシステムの段階が失敗した場合は，OS名の後に`Storage is not available. Insert a card and run reboot.`を出力する．このとき，ファイルを扱うコマンドと実行ファイルは使えない  
- Ethernetモジュールの段階が失敗した場合は，Qosmosがネットワークの設定を自動でやり直す．やり直しが成功すれば，telnetで接続できるようになる([qosmos/kernel/console.md](../kernel/console.md)の「接続の待ち受け」を参照)  

## SDカードの失敗
| 表示 | 原因 | 対処 |
|:-|:-|:-|
| `SD card: reset (CMD0): no response` | SDカードが差さっていない | SDカードを差して`reboot`する |
| `SD card: reset (CMD0): busy`・`unexpected response` | カードがコマンドを受け付けない | カードを差し直して`reboot`する．直らなければ，シールドの配線を確かめる([qurge/board.md](../../qurge/board.md)の「SDカードのシールド」を参照) |
| `SD card: voltage check (CMD8): SD v1, unsupported` | 対応しない古い規格のカード(SD v1)である | SDHC/SDXCのカードに替える |
| `SD card: voltage check (CMD8): no response`・`failed`・`check pattern mismatch` | 電圧の確認に応答しない．配線か電圧に問題がある | カードを差し直して`reboot`する．直らなければ，シールドの配線を確かめる |
| `SD card: initialize (ACMD41): CMD55 rejected`・`failed`・`not ready` | カードの初期化が終わらない | カードを差し直して`reboot`する．直らなければ，別のカードを試す |
| `SD card: card type (CMD58): SDSC, unsupported` | 容量2GB以下のカード(SDSC)である | SDHC/SDXCのカードに替える |
| `SD card: card type (CMD58): failed` | カードの種別を読み出せない | カードを差し直して`reboot`する |

扱えるカードは，[qosmos/user/file.md](file.md)の「扱えるカード」を参照．  

## ファイルシステムの失敗
| 表示 | 原因 | 対処 |
|:-|:-|:-|
| `FAT32: mount: cannot read card` | カードから読み出せない | カードを差し直して`reboot`する |
| `FAT32: mount: no FAT32 file system` | カードがFAT32でフォーマットされていないか，扱えない形式である(「扱えるカード」は[qosmos/user/file.md](file.md)を参照) | パソコンでFAT32にフォーマットし直してから`reboot`する |

## Ethernetモジュールの失敗
| 表示 | 原因 | 対処 |
|:-|:-|:-|
| `Ethernet: reset: no response` | モジュールが応答しない．モジュールが接続されていない場合を含む | モジュールのシールドの接続を確かめる([qurge/board.md](../../qurge/board.md)の「Ethernetモジュールのシールド」を参照)．Qosmosがやり直すため，接続し直せば`reboot`しなくてもよい |
| `Ethernet: reset: unexpected version` | 応答したモジュールがW5100Sではない | W5100Sを載せたモジュールを接続する |
| `Ethernet: network settings: mismatch` | 書き込んだ設定を読み戻した値が異なる | Qosmosがやり直すのを待つ．直らなければ，シールドの配線を確かめる |

`[ OK ] Ethernet: network settings`は，設定を書き込めたことを表す．  
LANのケーブルの接続やリンクの確立は確かめない．  

# 起動の行が届く端末
- 電源を入れたとき: telnetで接続した端末はまだ無いため，起動の行はUARTで接続した端末にだけ届く  
- `reboot`したとき: SDカードとファイルシステムの行は，両方の端末に届く．Ethernetモジュールの行とその後のOS名は，UARTで接続した端末にだけ届く．Ethernetモジュールをリセットする前に，telnetでの接続を閉じるためである  

UARTで接続した端末には，PSが最初に実行するプログラム(FSBL)の出力も届く．  
FSBLはCPUが動き出した後も出力しうるため，その出力が起動の行と混ざることがある．  

SDカードとファイルシステムの段階の成否は，終了状態として保持される．  
このため，telnetで接続した端末からも`status`で確かめられる([qosmos/user/shell.md](shell.md)の「終了状態」を参照)．  

起動のやり直し方の使い分けは，[operation.md](../../operation.md)の「起動とやり直し」を参照．  
