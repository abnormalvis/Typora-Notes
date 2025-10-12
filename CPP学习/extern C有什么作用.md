`extern "C"` 是 C++ 中的一个重要关键字，主要用于解决 C++ 和 C 语言之间的名称修饰（name mangling）和链接兼容性问题。

## 主要作用

### 1. 禁止 C++ 的名称修饰
C++ 支持函数重载，编译器会对函数名进行修饰（mangling），而 C 语言不会。

**C++ 代码**：
```cpp
void func(int a);        // 可能被修饰为 _func_i
void func(double a);     // 可能被修饰为 _func_d
```

**C 代码**：
```c
void func(int a);        // 保持为 func
```

### 2. 在 C++ 中调用 C 函数

**C 头文件 (clib.h)**：
```c
#ifndef CLIB_H
#define CLIB_H

#ifdef __cplusplus
extern "C" {
#endif

void c_function(int param);
int c_calculate(int a, int b);

#ifdef __cplusplus
}
#endif

#endif
```

**C++ 调用代码**：
```cpp
#include "clib.h"

int main() {
    c_function(10);           // 正确调用 C 函数
    int result = c_calculate(5, 3);
    return 0;
}
```

### 3. 在 C 中调用 C++ 函数

**C++ 代码**：
```cpp
// cpplib.h
#ifdef __cplusplus
extern "C" {
#endif

// 这些函数可以被 C 代码调用
void cpp_function_for_c(int param);
int cpp_calculate_for_c(int a, int b);

#ifdef __cplusplus
}
#endif

// C++ 实现
#ifdef __cplusplus
void cpp_function_for_c(int param) {
    // C++ 代码实现
}

int cpp_calculate_for_c(int a, int b) {
    return a + b;
}
#endif
```

**C 调用代码**：
```c
// main.c
#include "cpplib.h"

int main() {
    cpp_function_for_c(10);    // 调用 C++ 函数
    int result = cpp_calculate_for_c(5, 3);
    return 0;
}
```

## 使用场景

### 1. 包含 C 标准库头文件
```cpp
extern "C" {
    #include <stdio.h>
    #include <string.h>
    #include <math.h>
}
```

### 2. 现代标准写法（推荐）
```cpp
#ifdef __cplusplus
extern "C" {
#endif

#include <stdio.h>
#include <stdlib.h>

#ifdef __cplusplus
}
#endif
```

### 3. 系统头文件通常已经处理
大多数系统头文件已经包含了 `extern "C"` 保护：
```cpp
// 直接包含即可
#include <stdio.h>
#include <string.h>
```

## 实际示例

### C 库的实现和调用

**C 库头文件 (mylib.h)**：
```c
#ifndef MYLIB_H
#define MYLIB_H

#ifdef __cplusplus
extern "C" {
#endif

void print_message(const char* msg);
int add_numbers(int a, int b);

#ifdef __cplusplus
}
#endif

#endif
```

**C 库实现 (mylib.c)**：
```c
#include <stdio.h>
#include "mylib.h"

void print_message(const char* msg) {
    printf("Message: %s\n", msg);
}

int add_numbers(int a, int b) {
    return a + b;
}
```

**C++ 调用程序**：
```cpp
#include "mylib.h"
#include <iostream>

int main() {
    print_message("Hello from C++");  // 调用 C 函数
    std::cout << "Sum: " << add_numbers(10, 20) << std::endl;
    
    return 0;
}
```

## 编译命令

```bash
# 编译 C 库
gcc -c mylib.c -o mylib.o

# 编译 C++ 程序并链接 C 库
g++ main.cpp mylib.o -o program

# 或者一步编译
g++ main.cpp mylib.c -o program
```

## 注意事项

1. **`__cplusplus` 宏**：只在 C++ 编译器中定义
2. **函数重载**：`extern "C"` 修饰的函数不能重载
3. **C++ 特性**：`extern "C"` 函数内部仍可使用 C++ 特性
4. **名称冲突**：确保 C 函数名在全局命名空间中唯一

`extern "C"` 是实现 C/C++ 混合编程的关键机制，确保了两种语言之间的二进制兼容性。