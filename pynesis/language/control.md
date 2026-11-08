---
description: if・while・for・switchなどの制御構文
audience: Pynesisでプログラムを書く人
---

# 制御構文
```c
int classify(int n) {
    if (n < 0) {
        return -1;
    } else if (n == 0) {
        return 0;
    }
    switch (n) {
    case 1:
        return 1;
    default:
        break;
    }
    int count = 0;
    while (n > 1) {
        n = n / 2;
        count++;
    }
    return count;
}
```

| 構文 | 備考 |
|:-|:-|
| `if` / `else if` / `else` | |
| `for` | |
| `while` | |
| `do`-`while` | |
| `switch` / `case` / `default` | `case`の値は定数式で書く([pynesis/language/variable.md](variable.md)の「定数式」を参照) |
| `break` | |
| `continue` | |
| `return` | `return;`(値なし)と`return expr;`(値あり)の両方に対応する．どちらを書けるかは関数の戻り値型による([pynesis/language/function.md](function.md)の「戻り値」を参照) |

条件にポインタを書いた場合の扱いは，[pynesis/language/pointer.md](pointer.md)の「演算」を参照．  
`goto`とラベル文には対応しない([pynesis/language/unsupported.md](unsupported.md)を参照)．  
