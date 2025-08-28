# 📘 C++ `static` 關鍵字筆記

---

## 1. `static` 的兩種主要語境

### (1) 類別/結構外部的 `static`

- 限制變數或函數的**連結範圍 (linkage)** 為 **internal linkage**。
    
- 僅在當前的 **translation unit** 可見。
    
- 避免多個檔案間的名稱衝突。
    
- 類似於將符號「私有化」。
    

**範例：**

```cpp
// file1.cpp
static int counter = 0;  // 僅 file1.cpp 內可見

void increment() {
    counter++;
}

// file2.cpp
int counter = 100; // 與 file1.cpp 的 static counter 不衝突
```

---

### (2) 類別/結構內部的 `static`

- **成員變數**：所有該類別的實例共用一份記憶體。
    
- **成員函數**：不會接收 `this` 指標，因此無法存取非 `static` 成員。
    

**範例：**

```cpp
#include <iostream>
using namespace std;

class MyClass {
public:
    static int shared;  // 靜態成員變數
    int id;

    MyClass(int i) : id(i) { shared++; }

    static void printShared() {  // 靜態成員函數
        cout << "Shared count: " << shared << endl;
    }
};

int MyClass::shared = 0; // 靜態成員需額外定義

int main() {
    MyClass a(1), b(2);
    MyClass::printShared(); // 輸出 Shared count: 2
}
```

---

## 2. `translation unit` 的定義

- **定義**：C++ 中一個 **translation unit** = **一個來源檔案 (source file)** 加上 **它所包含的所有 header 檔案 (經過前處理展開後的完整內容)**。
    
- 編譯器會將每個 translation unit 分別編譯成一個 **目標檔 (.o 或 .obj)**，最後再由 **linker** 進行連結。
    

**範例：**

```cpp
// main.cpp
#include "util.h"
int main() {
    foo();
    return 0;
}

// util.h
void foo();

// util.cpp
#include <iostream>
#include "util.h"
void foo() {
    std::cout << "Hello" << std::endl;
}
```

➡️ 這裡共有兩個 translation unit：

1. `main.cpp` + `util.h`
    
2. `util.cpp` + `util.h`
    

每個 translation unit 會各自編譯，然後 linker 將它們連結在一起。若 `static` 符號存在於 `util.cpp`，則它不會被 `main.cpp` 看到。