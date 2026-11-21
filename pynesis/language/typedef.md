---
description: typedefによる型の別名の宣言・使い方・名前の規則
audience: Pynesisでプログラムを書く人
---

# 型の別名(typedef)
```c
typedef unsigned char byte_t;          // 整数型の別名
typedef struct {                       // 無名構造体の定義と同時に別名を付ける
    int value;
    node_t *next;                      // 自身の別名へのポインタもメンバにできる
} node_t;
typedef void (*handler_t)(int);        // 関数ポインタの別名

handler_t handlers[4];                 // 関数ポインタの配列
node_t nodes[8];

void main(void) {
    byte_t b = 200;
    node_t *head = &nodes[0];
    const byte_t LIMIT = 3;            // constは別名の前に付ける
}
```

`typedef`は，型に別名(typedef名)を付ける．typedef名は，元の型と同じ型の型名として使える．  

# 宣言
typedefは，グローバルスコープの直下にだけ書ける．関数の中(ブロックスコープ)に書くと，コンパイルエラーになる．  
1つのtypedefで付けられる名前は1つである．`typedef int A, *B;`は，カンマ区切りの宣言に対応しないため，コンパイルエラーになる．  

typedefは，次の形で宣言する．  

| 形 | 別名の型 |
|:-|:-|
| `typedef 型名 名前;` | 型名の型．型名には，整数型・列挙型(`enum Tag`)・構造体型(`struct Tag`)・別のtypedef名と，それらのポインタを書ける．ただし，「対応しない書き方」に挙げた型は書けない |
| `typedef 戻り値型 (*名前)(引数の型の並び);` | 関数ポインタ．宣言子の書き方は，関数ポインタの変数と同じである([pynesis/language/pointer.md](pointer.md)の「宣言」を参照) |
| `typedef struct [構造体名] { メンバ... } 名前;` | その場で定義した構造体．構造体名を省いた無名構造体にも付けられる |
| `typedef enum [列挙型名] { 列挙子... } 名前;` | その場で定義した列挙型(`int`と同じ型) |

構造体・列挙型の定義を伴う形では，閉じ波括弧の後ろに名前だけを書く．定義の規則は，[pynesis/language/struct.md](struct.md)・[pynesis/language/enum.md](enum.md)の「定義」と同じである．  

定義されていない構造体・列挙型に別名を付けると，typedef名を使わなくてもコンパイルエラーになる．  

# 使い方
typedef名は，元の型の型名を書ける場所に，元の型の型名と同じく書ける．ただし，次の`sizeof`と`const`の制限と，「対応しない書き方」に挙げた組み合わせを除く．  

- `T *p`のように`*`を付けると，別名の型を指すポインタになる  
- 整数型の別名に`const T x = 1;`のように`const`を前に付けると，const変数になる([pynesis/language/variable.md](variable.md)の「const変数」を参照)．const変数にできない型(ポインタ・構造体)の別名には，`const`を付けられない  
- `sizeof(T)`は，`sizeof`に書ける型名(整数型とそのポインタ)の別名にだけ書ける([pynesis/language/operator.md](operator.md)の「sizeof」を参照)  

別名の型と元の型は，区別しない．このため，typedef名の変数と元の型の変数の間で，代入・比較・構造体の代入ができる．エラーメッセージでは，型を元の型で表示する．ただし，typedef名を付けた無名構造体は，typedef名で表示する．  

typedef名は，グローバルスコープのほかの宣言と同じく，宣言の順序によらず参照できる．後で宣言したtypedef名も，`#include`で取り込んだファイルのtypedef名も使える．  

## 自身を参照する別名
構造体のメンバには，自身のtypedef名へのポインタを書ける．無名構造体でも書ける(この文書の冒頭の例の`node_t`)．  

一方，別名の型そのものが自身に依存するtypedefは，別名の型が決まらないため，コンパイルエラーになる．  

```c
typedef A B;
typedef B A;                           // AとBが互いに依存する
typedef void (*F)(F);                  // 引数の型に自身を書いている
```

# 名前の規則
typedef名は，構造体名と同じ規則に従う([pynesis/language/scope.md](scope.md)の「グローバルスコープ」を参照)．  

- 変数(const変数を含む)・関数・構造体名・列挙型名・列挙子・ハードウェア変数・ほかのtypedef名と，同じ名前にできない  
- 同じ名前のローカル変数・関数の引数は宣言できない(シャドーイングを認めない)  
- `static`は付けられない．typedef名はプログラム全体で共有する  
- 構造体のメンバ名とは，同じ名前にできる  

typedef名は型名として構文を読むため，同じ名前が変数を指すことがあると，`T * x;`が宣言か式か決まらなくなる．このため，シャドーイングを認めない．  

C言語と異なり，構造体名と同じtypedef名は付けられない(`typedef struct Node { ... } Node;`はエラー)．構造体名とtypedef名が同じ名前空間にあるためである．  

# 対応しない書き方
次のものは，コンパイルエラーになる．  

- 配列の別名(`typedef int A[4];`・`typedef int (*A[4])(void);`): 別名の配列が多次元配列になるため  
- `void`の別名: 値を持たない型であり，変数にも引数にも使えないため  
- `const`を含む型の別名(`typedef const int C;`): `const`は変数の宣言に直接書く  
- typedef名への符号修飾子(`unsigned T`)  
- 関数ポインタの別名へのポインタ(`F *p`)と，関数ポインタの別名を戻り値型にした関数・関数ポインタ: 関数ポインタへのポインタと関数ポインタを返す関数に対応しないため([pynesis/language/unsupported.md](unsupported.md)を参照)  
