# Pynesisの概要
Pynesisは，C言語に似た書き方の自作プログラミング言語である．  
ソースファイル(`.pn`)をコンパイラでアセンブリ言語Pyntaxisへ変換し，QurgeのCPUで動かす(コンパイラの使い方は[pynesis/compiler/usage.md](../compiler/usage.md)を参照)．  

```c
const int CAP = 4;                   // 定数
int table[CAP];                      // グローバル変数の配列

struct Pair {                        // 構造体
    int first;
    int second;
};

int sum(int *values, int n) {        // 関数．配列はポインタで受け取る
    int total = 0;
    for (int i = 0; i < n; i++) {
        total += values[i];
    }
    return total;
}

void main(void) {                    // プログラムの開始点
    for (int i = 0; i < CAP; i++) {
        table[i] = i;
    }
    struct Pair p;
    p.first = sum(table, CAP);       // 配列を渡すと先頭要素の番地が渡る
    LED = (unsigned int)p.first;     // ボードのLEDへ書き込む
}
```

# 文書の構成
言語の仕様は，話題ごとに次の文書に分ける．  

| 文書 | 内容 |
|:-|:-|
| [lexical.md](lexical.md) | 識別子・リテラル・コメント |
| [type.md](type.md) | データ型・型変換・キャスト |
| [variable.md](variable.md) | 変数・const変数・定数式・スコープ |
| [array.md](array.md) | 配列と文字列 |
| [struct.md](struct.md) | 構造体 |
| [pointer.md](pointer.md) | ポインタと関数ポインタ |
| [operator.md](operator.md) | 演算子と`sizeof` |
| [control.md](control.md) | 制御構文 |
| [function.md](function.md) | 関数と`main` |
| [include.md](include.md) | ソースファイルの取り込み |
| [builtin.md](builtin.md) | 組み込み関数 |
| [hardware.md](hardware.md) | ハードウェア変数 |
| [limit.md](limit.md) | 量的制限 |
| [unsupported.md](unsupported.md) | 未対応機能 |

# 出力先
プログラムは，次のどちらかの形に変換する．  
どちらにするかはコンパイラの起動引数で指定する([pynesis/compiler/usage.md](../compiler/usage.md)を参照)．  

- ROM: CPUに組み込むROMとして置き，リセット後に`main`から実行する．Qosmos自身がこの形で動く  
- 実行ファイル: Qosmosの実行ファイルとしてSDカードに置き，シェルから実行する([qosmos/program/executable.md](../../qosmos/program/executable.md)を参照)．ユーザーモードで動く([qurge/cpu/mode.md](../../qurge/cpu/mode.md)を参照)  

言語の仕様は出力先によらず同じであり，次の点だけが異なる．  

| | ROM | 実行ファイル |
|:-|:-|:-|
| `main`の形 | `void main(void)` | `int main(int argc, char **argv)`・`int main(void)` |
| `main`の終了 | その場で止まる | 戻り値を終了状態としてシェルへ伝える |
| ハードウェア変数 | 読み書きできる | 読み書きできない(コンパイルエラー) |
| `sysret()` | 使える | 使えない(コンパイルエラー) |
| 量的制限 | ROMの命令数とメモリの前半で決まる | 実行ファイルの領域で決まる |

それぞれの詳細は，[pynesis/language/function.md](function.md)の「main関数」，[pynesis/language/hardware.md](hardware.md)，[pynesis/language/builtin.md](builtin.md)の「ユーザーモードへ移る組み込み関数」，[pynesis/language/limit.md](limit.md)を参照．  
