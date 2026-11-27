---
description: Pynthesisの構成要素の名称と，Pynthesis全体で共通に使う用語
---

# 用語
Pynthesisの構成要素の名称と，Pynthesis全体で共通に使う用語の意味を定める．仕様書と各リポジトリの文章は，これらの語をここで定めた意味で使う．  

# 構成要素の名称
Pynthesisの全体と構成要素には，PYNQ-Z2ボードを使うことにちなみ「pynq」を共通の要素として組み込んだ，ギリシャ語由来の造語を付けている．  

| 名称 | 対象 | 由来 |
|:-|:-|:-|
| Pynthesis(ピンセシス) | 自作パソコンと，それを作るプロジェクトの全体 | pynq + 「組み上げる」を意味するギリシャ語(synthesis) |
| Qurge(クージ) | CPU・メモリ・ROM等のハードウェア | pynq + 「自らの技術で材料から物を作り上げる職人」を意味するDemiurge |
| Pynesis(ピネシス) | 自作プログラミング言語 | pynq + 「まだないものを生み出す行為」を意味するギリシャ語(poiesis) |
| Pyntaxis(ピンタクシス) | 自作アセンブリ言語 | pynq + 「順序立てて並べる」を意味するギリシャ語(syntaxis) |
| Qosmos(コスモス) | 自作OS．シェルやファイルシステムなど，ROM上で動くソフトウェアの一式 | pynq + 「秩序ある世界」を意味するギリシャ語(kosmos)．Demiurgeが混沌から秩序ある世界を形作ることにちなみ，Qurgeが動かす世界として同じくpynqのqを冠する |

各名称に対応するリポジトリは，次のとおりである．  

- Pynthesis: 自作パソコンと，それを作るプロジェクトの全体を表す名称のため対応するリポジトリなし  
- Pynesis: [pynesis](https://github.com/tt-and-tk/pynesis)  
- Pyntaxis: [pyntaxis](https://github.com/tt-and-tk/pyntaxis)  
- Qurge: [qurge](https://github.com/tt-and-tk/qurge)  
- Qosmos: 専用のリポジトリはなく，[qurge](https://github.com/tt-and-tk/qurge)の`mypc.srcs/pn/`に置く  

# 共通の用語
## PC
「PC」は，プログラムカウンタだけを指す([qurge/cpu/register.md](qurge/cpu/register.md)を参照)．Pynthesisや一般のパソコンを指すのには使わない．  

## パソコン
修飾の付かない「パソコン」は，Windowsなどが動く一般のパソコンを指す．Pynthesisを指す場合は，「Pynthesis」と書く．「自作パソコン」のように修飾を付けた語は，一般のパソコンに限らず使える．  

## リセット
対象を書かない「リセット」は，CPUのリセットを指す．CPUのリセットは，CPUをプログラムの先頭から実行し直させる信号である([qurge/boot.md](qurge/boot.md)の「SRSTによる起動のやり直し」を参照)．  

次のリセットは，前後の文で対象が定まる場合を除き，対象を付けて書く．  

- SRSTのボタンによるPSのリセット  
- 周辺機器のリセット(Ethernetモジュールのリセットなど)  

## 実行ファイルとプロセス
「実行ファイル」と「プロセス」は，次のように使い分ける．  

| 語 | 指すもの | この語で書く事柄 |
|:-|:-|:-|
| 実行ファイル | Qosmosが実行するプログラムのファイルと，その中身(形式・ヘッダ・命令列)([qosmos/program/executable.md](qosmos/program/executable.md)を参照) | コンパイラ・アセンブラが作るもの，プログラムを書く人が守る取り決め |
| プロセス | Qosmosがメモリに置いた実行ファイル([qosmos/kernel/exec.md](qosmos/kernel/exec.md)を参照) | 動いている間の事柄(領域や，例外による終了など) |

## 実機
「実機」は，PYNQ-Z2ボードにシールドを重ねた，Pynthesisの実物を指す([board.md](board.md)を参照)．シミュレーションと区別して，実物での動作や操作を述べるときに使う．  
