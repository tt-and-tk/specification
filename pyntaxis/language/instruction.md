# 命令
命令は，命令の名前に続けて引数を書く．  
引数の並びは命令の種類(系)ごとに決まっており，各節の例のとおりに書く．  
各命令がCPUで行う動作の詳細は，[qurge/cpu/isa.md](../../qurge/cpu/isa.md)の同じ名前の節を参照．  

引数に書くものは次のとおりである．  

- `rs1`・`rs2`: 演算に使うレジスタ  
- `rd`: 結果を格納するレジスタ  
- `mask`: 読み書きするバイトを表す4ビットの値([qurge/cpu/isa.md](../../qurge/cpu/isa.md)の「番地とmaskの詳細動作」を参照)  
- `imm`: イミディエイトデータ．数値の書き方は[pyntaxis/language/program.md](program.md)の「数値の書き方」を参照  

# 処理を実行しない(N系)
```
    nop                 ; 何もしない
```

# 演算系(P系)
rs1とrs2の演算結果をrdに格納する．  

```
    and  r1 r2 r3       ; r3 = r1 & r2(論理積)
    or   r1 r2 r3       ; r3 = r1 | r2(論理和)
    xor  r1 r2 r3       ; r3 = r1 ^ r2(排他的論理和)
    not  r1 r2          ; r2 = ~r1(反転．rs2は書かない)
    nand r1 r2 r3       ; r3 = ~(r1 & r2)(否定論理積)
    add  r1 r2 r3       ; r3 = r1 + r2
    sub  r1 r2 r3       ; r3 = r1 - r2
    mul  r1 r2 r3       ; r3 = r1 * r2
    div  r1 r2 r3       ; r3 = r1 / r2(符号あり．余りは捨てる)
    div  r1 r2 r3 4     ; r3 = r1 / r2，r4 = r1 % r2(符号あり)
    divu r1 r2 r3       ; r3 = r1 / r2(符号なし．余りは捨てる)
    divu r1 r2 r3 4     ; r3 = r1 / r2，r4 = r1 % r2(符号なし)
```

`div`・`divu`で余りも求める場合は，余りを格納するレジスタのレジスタ番地を4つ目の引数に書く．  
この引数はレジスタ番地を表す数値であり，`r`を付けずに書く．  

# シフト系(S系)
rs1を，rs2またはimmの値だけシフトしてrdに格納する．  

```
    sll r1 r2 r3        ; r3 = r1をr2だけ左シフト
    srl r1 r2 r3        ; r3 = r1をr2だけ右シフト(空いたビットを0で埋める)
    sla r1 r2 r3        ; r3 = r1をr2だけ算術左シフト(sllと同じ結果)
    sra r1 r2 r3        ; r3 = r1をr2だけ算術右シフト(空いたビットを符号ビットで埋める)
    sll r1 r0 r3 4      ; r3 = r1を4だけ左シフト(immを書くとrs2は使わない)
```

# 代入系(A系)
rs1またはimmの値をrdに格納する．  

```
    mov fh r0 r1 42     ; r1 = 42(immを書くとrs1は使わない)
    mov fh r2 r1        ; r1 = r2
```

maskは必ず書くが，値によらず常にrdの全バイトへ書き込む．  

# 分岐系(F系)
rs1とrs2を比較し，条件が成り立てば局所ラベルの位置へ飛ぶ．  
成り立たなければ次の命令へ進む．  
飛び先には局所ラベルだけを書ける([pyntaxis/language/label.md](label.md)を参照)．  

```
    eq   r1 r2 .L0      ; r1 == r2なら.L0へ
    ne   r1 r2 .L0      ; r1 != r2なら.L0へ
    lt   r1 r2 .L0      ; r1 < r2なら.L0へ(符号あり)
    gt   r1 r2 .L0      ; r1 > r2なら.L0へ(符号あり)
    elt  r1 r2 .L0      ; r1 <= r2なら.L0へ(符号あり)
    egt  r1 r2 .L0      ; r1 >= r2なら.L0へ(符号あり)
    ltu  r1 r2 .L0      ; r1 < r2なら.L0へ(符号なし)
    gtu  r1 r2 .L0      ; r1 > r2なら.L0へ(符号なし)
    eltu r1 r2 .L0      ; r1 <= r2なら.L0へ(符号なし)
    egtu r1 r2 .L0      ; r1 >= r2なら.L0へ(符号なし)
```

# ジャンプ系(J系)
```
main:
    jmp .L0             ; .L0へ飛ぶ
.L0:
    call func1          ; func1を呼び出す
    mov fh r0 r1 func1  ; r1 = func1の番地
    call r1             ; r1の番地の関数(func1)を呼び出す
    ret                 ; mainを終える

func1:
    ret                 ; 呼び出し元へ戻る
```

- `jmp <局所ラベル>`: 局所ラベルの位置へ飛ぶ．飛び先には局所ラベルだけを書ける．CPUの命令はレジスタの値の番地へも飛べるが，`jmp`では当面その書き方を設けない  
- `call <関数名>`: 関数を呼び出す．呼び出された関数の`ret`で，`call`の次の命令へ戻る．`main`は呼び出せない．`main`はプログラムの開始点であり，どの出力先でも`main`の`ret`は呼び出し元へ戻らないためである  
- `call <レジスタ>`: レジスタの値を番地とする関数を呼び出す．関数の番地は，イミディエイトデータの位置に関数名を書いて求める([pyntaxis/language/program.md](program.md)の「関数の番地」を参照)  
- `ret`: 呼び出し元へ戻る．`main`の`ret`の扱いは出力先によって異なる([pyntaxis/language/target.md](target.md)を参照)  

呼び出しを入れ子にできる段数に固定の上限はない．  
深さの上限は，戻り先を積むスタックの残り容量で決まる([qurge/cpu/stack.md](../../qurge/cpu/stack.md)を参照)．  

## 動作状態を切り替える命令
`syscall`・`sysret`は，動作状態(カーネルモード・ユーザーモード)を切り替えて飛ぶ．  
引数は取らない．  
動作と使う側の注意は，[qurge/cpu/mode.md](../../qurge/cpu/mode.md)の「動作状態の切り替え」を参照．  

- `syscall`: 次の命令の番地をKRET(`r12h`)へ書き込み，カーネルモードにして，KENTRY(`r11h`)の値の番地へ飛ぶ  
- `sysret`: ユーザーモードにして，KRETの値の番地へ飛ぶ．カーネルモードでだけ実行できる  

`sysret`は`ret`の代わりにならない．  
`sysret`で終わる関数にも，`ret`を置く．  

次の例の`setup`と`handler`はOSの関数，`user`はOSがユーザーモードで動かすプログラムの中の関数とする．  

```
setup:
    mov fh r0 r11h handler ; KENTRYへhandlerの番地を書き込んでおく
    ret

user:
    syscall             ; カーネルモードにしてhandlerへ飛ぶ
    ret                 ; handlerのsysretで，ここへ戻る

handler:
    sysret              ; ユーザーモードにして，userのsyscallの次の命令へ飛ぶ
    ret                 ; どの関数にも必要なret
```

# メモリ系(M系)
読み込みの命令は，メモリの番地の値をrdへ格納する．  
書き込みの命令は，rs2の値をメモリの番地へ書き込む．  
番地の指定方法は命令によって異なる．  

```
    rm  fh r1 r3        ; r3 = r1番地の値
    rm  fh r0 r3 100    ; r3 = 100番地の値(immを書くとrs1は使わない)
    wm  fh r1 r2        ; r1番地へr2を書き込む
    wm  fh r0 r2 100    ; 100番地へr2を書き込む(immを書くとrs1は使わない)
    rmr fh r16 r3 4     ; r3 = SP+4番地の値
    wmr fh r1 r2 -4     ; r1-4番地へr2を書き込む
```

- `rm`/`wm`: rs1またはimmで番地を指定する  
- `rmr`/`wmr`: rs1にimmを足した番地を指定する．immは省略できない．immには負の値も書ける  
- `brm`/`bwm`: バースト転送の命令．アセンブルはできるが，CPUが未実装のため実行すると例外になる  

SP(`r16`)をrs1にした`rmr`/`wmr`で，スタック上に確保した領域を読み書きできる([qurge/cpu/stack.md](../../qurge/cpu/stack.md)の「スタック上の領域の確保」を参照)．  
