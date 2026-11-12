# Pynthesis 仕様書

自作パソコンのプロジェクトPynthesisの仕様をまとめたものであり，各リポジトリの実装が参照する唯一の一次情報源である．  

## 名称について

このプロジェクト全体および構成要素には，PYNQ-Z2ボードを使うことにちなみ「pynq」を共通の要素として組み込んだ，ギリシャ語由来の造語を付けている．  

| 名称 | 対象 | 由来 |
|:-|:-|:-|
| Pynthesis(ピンセシス) | 自作パソコンのプロジェクト全体 | pynq + 「組み上げる」を意味するギリシャ語(synthesis) |
| Qurge(クージ) | CPU・メモリ・ROM等のハードウェア | pynq + 「自らの技術で材料から物を作り上げる職人」を意味するDemiurge |
| Pynesis(ピネシス) | 自作プログラミング言語 | pynq + 「まだないものを生み出す行為」を意味するギリシャ語(poiesis) |
| Pyntaxis(ピンタクシス) | 自作アセンブリ言語 | pynq + 「順序立てて並べる」を意味するギリシャ語(syntaxis) |
| Qosmos(コスモス) | 自作OS．シェルやファイルシステムなど，ROM上で動くソフトウェアの一式 | pynq + 「秩序ある世界」を意味するギリシャ語(kosmos)．Demiurgeが混沌から秩序ある世界を形作ることにちなみ，Qurgeが動かす世界として同じくpynqのqを冠する |

各名称に対応するリポジトリは，次のとおりである．  

- Pynthesis: 自作パソコンのプロジェクト全体を表す用語のため対応するリポジトリなし  
- Pynesis: [pynesis](https://github.com/tt-and-tk/pynesis)  
- Pyntaxis: [pyntaxis](https://github.com/tt-and-tk/pyntaxis)  
- Qurge: [qurge](https://github.com/tt-and-tk/qurge)  
- Qosmos: 専用のリポジトリはなく，[qurge](https://github.com/tt-and-tk/qurge)の`mypc.srcs/pn/`に置く  

## 各仕様

仕様は，構成要素ごとのディレクトリに分けて置く．  
各ディレクトリの`index.md`に，そのディレクトリのファイルとディレクトリの一覧を置く．  
各文書の先頭のフロントマターには，その文書に書いてあること(`description`)を書く．  
読者を限る文書には，想定する読者(`audience`)も書く．  
番地の上下は，番地の大きい側を上，小さい側を下と呼ぶ([qurge/memory.md](qurge/memory.md)の「容量と番地」を参照)．  

```
specification/
├── operation.md       実機の準備・接続・起動のやり直し・実行ファイルを置く方法の手順
├── board.md           ボードとシールドの物理的な構成と，ピンの割り当て
├── qurge/             ハードウェアQurgeの仕様．メインメモリとROM
│   └── cpu/           CPUの命令・レジスタ・スタック・動作状態・例外・パイプライン
├── pyntaxis/          アセンブリ言語Pyntaxis
│   ├── language/      言語自体の仕様
│   └── assembler/     アセンブラの使い方と，命令を機械語へ変換する方法
├── pynesis/           プログラミング言語Pynesis
│   ├── language/      言語自体の仕様
│   └── compiler/      コンパイラの使い方と，コンパイラが生成する命令
└── qosmos/            OS Qosmos
    ├── user/          利用者から見たQosmosの動作．起動時の表示・コンソール・シェル・組み込みコマンド・ファイル
    ├── program/       実行ファイルから見たQosmos．実行ファイルの作り方と，システムコールの使い方
    └── kernel/        Qosmosの内部で，各機能を実現する仕組み
```
