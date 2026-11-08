# 未対応機能
小さく始める方針により，次の機能には当面対応しない．  

| 機能 | 備考 |
|:-|:-|
| `#define`・`#ifdef`などのプリプロセッサ指令 | [pynesis/language/include.md](include.md)に書いた指令以外には対応しない．定数の定義にはconst変数を使う([pynesis/language/variable.md](variable.md)の「const変数」を参照) |
| `sizeof(計算式)` | `sizeof(1 + 2)`のような任意の式には対応せず，型名・変数名だけに対応する．将来対応する予定である([pynesis/language/operator.md](operator.md)の「sizeof」を参照) |
| `void *`・配列へのポインタ(`int (*p)[4]`)・関数ポインタの配列・関数ポインタへのポインタ・関数ポインタを返す関数・`const`とポインタの組み合わせ | ポインタ([pynesis/language/pointer.md](pointer.md)を参照)のうち，これらの型は宣言できない |
| `long` | 将来対応する予定である |
| `float`・`double` | CPUに浮動小数点の命令がないため |
| 整数リテラルの接尾辞(`10u`など) | 型はキャストで指定する(`(unsigned int)10`．[pynesis/language/type.md](type.md)の「キャスト」を参照) |
| `union` | |
| カンマ区切りの複数変数宣言 | 個別に対応を検討する予定である(pynesisリポジトリ issue #15) |
| `typedef` | 個別に対応を検討する予定である(pynesisリポジトリ issue #16) |
| 構造体を関数の引数・戻り値にすること | 構造体へのポインタを引数・戻り値にして受け渡せる([pynesis/language/struct.md](struct.md)の「構造体そのものの値」を参照)．個別に対応を検討する予定である(pynesisリポジトリ issue #17) |
| `static`変数 | |
| `goto`・ラベル文 | |
| カンマ演算子 | 対応する予定はない．カンマは，関数の引数の宣言と，関数呼び出しで渡す引数(組み込み関数を含む)の区切りとしてだけ使う |
| 動的なメモリ管理(`malloc`など) | |
| 標準ライブラリ全般(`printf`，`scanf`など) | 代わりに組み込み関数を提供する([pynesis/language/builtin.md](builtin.md)を参照) |
| 配列の添字の範囲外の検査 | コンパイル時・実行時とも検査しない．C言語と同じく，未定義動作として扱う |
| 配列の初期化子(初期値リスト) | 文字列リテラルによる`char`の配列の初期化だけに対応する([pynesis/language/array.md](array.md)を参照) |
| 入れ子の構造体・構造体のメンバの初期化子 | 将来対応する予定もない([pynesis/language/struct.md](struct.md)の「対応しない書き方」を参照) |
