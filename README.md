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

各名称に対応するリポジトリ:  

- Pynthesis: 自作パソコンのプロジェクト全体を表す用語のため対応するリポジトリなし  
- Pynesis: [pynesis](https://github.com/tt-and-tk/pynesis)  
- Pyntaxis: [pyntaxis](https://github.com/tt-and-tk/pyntaxis)  
- Qurge: [qurge](https://github.com/tt-and-tk/qurge)  
- Qosmos: 専用のリポジトリはなく，[qurge](https://github.com/tt-and-tk/qurge)の`mypc.srcs/pn/`に置く  

## 各仕様

仕様は，構成要素ごとのディレクトリに分けて置く．  
各ディレクトリの`index.md`に，そのディレクトリの文書の一覧と読む順番を置く．  
各文書の先頭のフロントマターには，その文書に書いてあること(`description`)と，想定する読者(`audience`)を書く．  

```
specification/
├── operation.md       実機の準備・接続・起動のやり直し・実行ファイルを置く方法．実機を動かす人が最初に読む
├── qurge/             ハードウェアQurgeの仕様．ボードの構成・メインメモリ・ROM．
│                      実機を組み立てる人と，回路を作る人・その上で動くプログラムを書く人向け
│   └── cpu/           CPUの命令・レジスタ・スタック・動作状態・例外・パイプライン．
│                      CPUで動くプログラムを書く人と，CPUの回路を作る人向け
├── pyntaxis/          アセンブリ言語Pyntaxis
│   ├── language/      言語の書き方．アセンブリでプログラムを書く人向け
│   └── assembler/     アセンブラの使い方と，機械語への変換の仕組み．
│                      アセンブラを使う人と，アセンブラを変える人向け
├── pynesis/           プログラミング言語Pynesis
│   ├── language/      言語仕様．Pynesisでプログラムを書く人向け
│   └── compiler/      コンパイラの使い方と，コンパイラが生成する命令．
│                      コンパイラを使う人と，コンパイラを変える人向け
└── qosmos/            OS Qosmos
    ├── user/          起動時の表示・コンソール・シェル・組み込みコマンド・ファイル．Qosmosを使う人向け
    ├── program/       実行ファイルとシステムコール．実行ファイルを作る人向け
    └── kernel/        起動・メモリ・コンソール・ファイルシステム・ネットワーク・実行ファイル・
                       システムコールの仕組み．Qosmosを変える人向け
```
