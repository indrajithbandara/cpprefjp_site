# enum class変数の初期値として整数を指定する際の規則を調整
* cpp17[meta cpp]

## 概要



## 仕様

文法仕様は下記の通り。

```
```


## 例
```cpp example
enum class byte : char { };

int main()
{
	// C++14
	byte aa { (byte)42 };
	byte ab = (byte)42;
	byte ac = static_cast<byte>(42);
	// C++17
	byte ba { 42 };         // OK in C++17, error in C++14
	byte bb = byte{ 42 };   // OK in C++17, error in C++14
	byte bc = { 42 };       // error, { 42 } is int

	// C++14
	byte ca { (byte)42 };
	byte cb { (byte)1142 }; // OK, but cannot detect narrowing
	// C++17
	byte da { 42 };         // OK in C++17, error in C++14
	byte db { 1142 };       // error, narrowing

	return 0;
}
```

### 出力
不適格。

### 警告例
clang++ 6.0.0 C++14モードにてコンパイルした場合。

```
construction_enum_class.cpp:13:12: error: cannot initialize a variable of type
      'byte' with an rvalue of type 'int'
        byte ba { 42 };         // OK in C++17, error in C++14
                  ^~
construction_enum_class.cpp:14:18: error: cannot initialize a value of type
      'byte' with an rvalue of type 'int'
        byte bb = byte{ 42 };   // OK in C++17, error in C++14
                        ^~
construction_enum_class.cpp:15:14: error: cannot initialize a variable of type
      'byte' with an rvalue of type 'int'
        byte bc = { 42 };       // error, { 42 } is int
                    ^~
construction_enum_class.cpp:21:12: error: cannot initialize a variable of type
      'byte' with an rvalue of type 'int'
        byte da { 42 };         // OK in C++17, error in C++14
                  ^~
construction_enum_class.cpp:22:12: error: cannot initialize a variable of type
      'byte' with an rvalue of type 'int'
        byte db { 1142 };       // error, narrowing
                  ^~~~
5 errors generated.
```

clang++ 6.0.0 C++17モードにてコンパイルした場合。

```
construction_enum_class.cpp:15:14: error: cannot initialize a variable of type
      'byte' with an rvalue of type 'int'
        byte bc = { 42 };       // error, { 42 } is int
                    ^~
construction_enum_class.cpp:22:12: error: constant expression evaluates to 1142
      which cannot be narrowed to type 'byte' [-Wc++11-narrowing]
        byte db { 1142 };       // error, narrowing
                  ^~~~
2 errors generated.
```


## 関連項目
- [C++11 スコープを持つ列挙型](/lang/cpp11/scoped_enum.md)

## 参照
- [P0138R2 Construction Rules for enum class Values.](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0138r2.pdf)
