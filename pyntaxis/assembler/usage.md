# アセンブラの使い方
アセンブラ`asm2mc.exe`は，アセンブリ言語Pyntaxis([pyntaxis/language/program.md](../language/program.md)を参照)で書いたファイル(`.pt`)を，次のどちらかへ変換する．  
名前のmcは機械語(machine code)を表し，出力するファイルの種類によらず同じ名前を使う．  

- ROMとして埋め込むSystemVerilogのソース(`.sv`)  
- Qosmosの実行ファイル  

```
asm2mc.exe -pt program.pt -sv program.sv    # ROMを出力する
asm2mc.exe -pt program.pt                   # ROMを出力する(program.svになる)
asm2mc.exe -pt hello.pt -bin HELLO          # 実行ファイルHELLOを出力する
```

# 起動引数
| フラグ | 内容 | 省略時 |
|:-|:-|:-|
| `-pt` | 入力するアセンブリのファイル名(`.pt`) | 指定子を付けずに置いた引数を，入力のファイル名とする．どちらも無い場合はエラーになる |
| `-sv` | 出力するSystemVerilogのROMのファイル名(`.sv`) | `-pt`の拡張子を`.sv`に変えた名前 |
| `-bin` | 出力する実行ファイルの名前(「実行ファイルの名前」を参照) | SystemVerilogのROMを出力する |

- 入力のファイル名は`.pt`で，SystemVerilogのROMのファイル名は`.sv`で終わる必要がある．終わらない場合はエラーになる  
- 出力するファイルの種類は，`-bin`の有無で決まる．実行ファイルを出力するには`-bin`を指定する  
- `-sv`と`-bin`を両方指定した場合はエラーになる  
- 同じフラグや指定子なしの引数を複数回指定した場合は，エラーにはならず，最後に指定した値を使う  

## 実行ファイルの名前
`-bin`に指定する名前のうち，ディレクトリを除いたファイル名の部分は，次の両方を満たす必要がある．  
満たさない名前はエラーになる．  

- Qosmosの名前の規則([qosmos/user/file.md](../../qosmos/user/file.md)の「名前の規則」を参照)に従う  
- 拡張子を持たない．Qosmosは拡張子の無いファイルだけを実行ファイルとして探すため  

# エラー
- アセンブリの誤りや，命令数が上限([pyntaxis/language/target.md](../language/target.md)の「命令数の上限」を参照)を超える場合は，エラーになる  
- エラーになった場合は，出力ファイルを作らない  
