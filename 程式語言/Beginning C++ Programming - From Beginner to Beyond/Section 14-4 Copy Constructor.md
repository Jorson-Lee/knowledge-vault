---
Up:
  - "[[Beginning C++ Programming - From Beginner to Beyond]]"
---
# 🧠什麼是 Copy Constructor（複製建構子）？

Copy Constructor 是用來用一個已經存在的物件來初始化另一個同類型的新物件的特殊建構子。

## 🔹 1. 語法格式
```cpp
ClassName(const ClassName& other);
```

- 參數是對同類型物件的**常數參考**（`const &`），避免不必要的複製並保護原物件不被修改。
    
- 主要目的是：**用已有物件複製一個新物件**。
    
## 🔹 2. 什麼時候會自動呼叫？
1. 用已存在的物件建立新物件時：
```cpp
Student s1("Tom", 20);
Student s2 = s1;  // 呼叫 copy constructor
```

2. 當物件以值傳遞（pass-by-value）做函式參數時：
```cpp
void foo(Student s);  // 這裡會呼叫 copy constructor 複製參數

foo(s1);
```

3. 函式回傳值時（回傳類別物件）也可能觸發複製建構：
```cpp
Student createStudent() {
    Student s("Jerry", 18);
    return s;  // 回傳時會呼叫 copy constructor（視編譯器優化而定）
}
```

## 🔹 3. C++ 預設提供的複製建構子
若你沒有自己寫複製建構子，編譯器會自動產生一個**淺拷貝**（shallow copy）的複製建構子：

- 逐成員複製：每個成員使用其 own copy constructor（或直接複製數值）複製。
    
- 如果成員是指標，則只會複製指標的地址，**不會複製指標所指向的物件** → 可能造成多個物件共用同一塊記憶體。
    
## 🔹 4. 自己寫複製建構子的時機？
當你的類別有**指標成員**（管理動態記憶體）或**其他需要深拷貝的情況**時，建議自己寫複製建構子來避免問題。

## 🔹 5. 示範：預設淺拷貝（淺複製）問題

```cpp
#include <iostream>
#include <cstring>
using namespace std;

class Person {
private:
    char* name;

public:
    Person(const char* n) {
        name = new char[strlen(n) + 1];
        strcpy(name, n);
    }

    // 編譯器自動產生淺拷貝複製建構子

    void print() {
        cout << "Name: " << name << endl;
    }

    ~Person() {
        delete[] name;
    }
};

int main() {
    Person p1("Alice");
    Person p2 = p1;  // 使用編譯器自動淺拷貝的複製建構子

    p1.print();
    p2.print();

    return 0;
}
```

## 🤔問題：當 `p1` 與 `p2` 被析構時，兩者的 `name` 指標都是指向同一個記憶體區塊，會造成**重複刪除（double free）錯誤**。

# 🔹 6. 示範：自己寫深拷貝的複製建構子

```cpp
class Person {
private:
    char* name;

public:
    Person(const char* n) {
        name = new char[strlen(n) + 1];
        strcpy(name, n);
    }

    // 深拷貝複製建構子
    Person(const Person& other) {
        name = new char[strlen(other.name) + 1];
        strcpy(name, other.name);
    }

    void print() {
        cout << "Name: " << name << endl;
    }

    ~Person() {
        delete[] name;
    }
};
```

# 🔹 7. 重點總結

| 事項                   | 說明                        |
| -------------------- | ------------------------- |
| Copy Constructor 是什麼 | 用一個物件複製並初始化另一個物件          |
| 預設複製建構子              | 編譯器會產生淺拷貝版本               |
| 淺拷貝問題                | 指標成員只複製指標位址，可能造成重複刪除或資源競爭 |
| 自己寫複製建構子時機           | 類別有動態配置資源（指標等）時，需深拷貝      |
| 複製建構子參數              | 一定是 `const ClassName&`    |
# 🧠複製建構子（Copy Constructor）**只能有一個**，因為它的定義是固定的
```cpp
ClassName(const ClassName& other);
```

## 原因：

- 複製建構子的目的就是用「同一個類別的物件」來複製一個新物件，參數型態就是該類別的 const 引用。
    
- 如果有多個複製建構子，編譯器就無法判斷你到底要呼叫哪一個，造成模糊不清（ambiguous）。
    
## 小提醒：

- 你可以**重載建構子（Constructor overloading）**，讓不同參數組合呼叫不同建構子，但複製建構子本身就是「用同一類別的 const 引用參數」來定義的，**不會有多個版本**。
    
- 不過你可以搭配**移動建構子（Move Constructor）**，兩者合起來是類別的「特殊建構子」：
    
    - 複製建構子：`ClassName(const ClassName& other);`
        
    - 移動建構子：`ClassName(ClassName&& other);`
        

這兩個有不同參數型態，算是不同的建構子，但都是「特殊建構子」，其中複製建構子只能有一個。

## 總結

| 事項      | 狀況說明                          |
| ------- | ----------------------------- |
| 複製建構子數量 | 只能有一個                         |
| 為什麼？    | 參數型態固定且唯一（`const ClassName&`） |
| 其他相似建構子 | 可搭配移動建構子，有不同參數型態，非複製建構子       |

---
# 🧠淺拷貝vs深拷貝
## 🌊 什麼是淺層複製 (shallow copy)？
當你的類別成員是「指標」時，**預設的複製建構子只會複製這個指標的值（也就是記憶體位置的位址）**，不會去複製「那塊指標所指向的資料」。

### 🔧 結果：

- 你會有兩個物件指向**同一塊記憶體**。
    
- 其中一個物件刪除或改變內容，另一個也會受到影響。
    
- 一不小心會造成 **double free** 或 **dangling pointer** 問題。
    
## 🧪 舉個簡單例子來看：
```cpp
#include <iostream>
#include <cstring>
using namespace std;

class Person {
private:
    char* name;
public:
    Person(const char* inputName) {
        name = new char[strlen(inputName) + 1];
        strcpy(name, inputName);
    }

    // 預設的複製建構子 (shallow copy)
    Person(const Person& other) {
        name = other.name;  // 只複製指標的位址
    }

    void print() {
        cout << "Name: " << name << endl;
    }

    ~Person() {
        delete[] name;
    }
};
```
### 使用情境：
```cpp
int main() {
    Person p1("Alice");
    Person p2 = p1;  // 呼叫複製建構子，p2.name = p1.name

    p1.print();  // Name: Alice
    p2.print();  // Name: Alice

    // 程式結束，p1 和 p2 都會呼叫 destructor，兩次 delete 同一塊 name 記憶體 → 錯誤！！
}
```
## ✅ 正確的作法：用 **深層複製 deep copy**

在複製建構子裡，自己動態配置新記憶體，然後把資料複製過去。

```cpp
Person(const Person& other) {     
	name = new char[strlen(other.name) + 1];     
	strcpy(name, other.name);
}
```

### 這樣的好處：
- 每個 `Person` 都有自己的 `name` 字串空間
    
- 不會互相干擾
    
- 不會出現重複刪除記憶體的問題
---
## 🌱 什麼是物件的「生命週期」？

生命週期指的是一個物件從被建立到被銷毀的整段期間。

- 在 `main()` 中定義的 `p1`、`p2` 是 **區域變數（local objects）**
    
- 它們的生命週期從宣告那一行開始，到 `main()` 結束為止
    
- 當生命週期結束，會自動呼叫 destructor 去清理資源
    
### 🧪 回到我們的程式碼（加上說明）：
```cpp
#include <iostream>
#include <cstring>
using namespace std;

class Person {
private:
    char* name;

public:
    // 建構子
    Person(const char* inputName) {
        name = new char[strlen(inputName) + 1];
        strcpy(name, inputName);
        cout << "Constructor: " << name << endl;
    }

    // 複製建構子（淺層複製）
    Person(const Person& other) {
        name = other.name;  // 指向同一個記憶體！
        cout << "Copy Constructor (shallow): " << name << endl;
    }

    void print() {
        cout << "Name: " << name << endl;
    }

    // 解構子
    ~Person() {
        cout << "Destructor: " << name << endl;
        delete[] name;
    }
};

int main() {
    Person p1("Alice");   // 呼叫建構子，開一塊 name = "Alice" 的記憶體
    Person p2 = p1;       // 呼叫複製建構子，p2.name 指向同一塊記憶體
    p1.print();           // Name: Alice
    p2.print();           // Name: Alice
}  // 離開 main()，p2 和 p1 會相繼被解構，兩次 delete 同一塊 name → 錯誤！

```

## 🔍 p1 / p2 的生命週期圖解
```sql
main()
│
├── 建立 p1 → 呼叫 Constructor，配置 name = "Alice"
│         p1.name 指向記憶體 A
│
├── 建立 p2 = p1 → 呼叫 Copy Constructor（淺層複製）
│         p2.name 也指向記憶體 A（同一塊）
│
├── p1.print() / p2.print() → 都輸出 Alice
│
└── main() 結束：
         ↓
        p2 被解構 → delete[] name (A)
         ↓
        p1 被解構 → delete[] name (A) ❌ → 對已釋放記憶體做 delete → **崩潰！**

```
---
## ✅ 如果改用深層複製（Deep Copy）
```cpp
// 正確的複製建構子
Person(const Person& other) {
    name = new char[strlen(other.name) + 1];
    strcpy(name, other.name);
    cout << "Copy Constructor (deep): " << name << endl;
}
```
就變成：
```sql
main()
│
├── 建立 p1 → name 指向記憶體 A
├── 建立 p2 → name 指向記憶體 B（複製內容）
│
└── 結束 → p2 解構 → 釋放 B
          p1 解構 → 釋放 A ✅ 完全沒衝突

```
# 🧠 一句話精華版：

> **深層複製**就是：「我重新開一塊自己的記憶體，把 source 指向的內容『值』複製過來，而不是共用指標本身。」

## 🔧 深層複製的標準步驟：

以 `name` 為指標來說，假設你有這樣的 class：
```cpp
class Person {
private:
    char* name;
public:
    Person(const char* n);               // 一般建構子
    Person(const Person& source);        // 複製建構子（重點）
    ~Person();                           // 解構子
};
```
## ✅ 複製建構子（Deep Copy）內容如下：
```cpp
Person::Person(const Person& source) {
    // 1. 配置自己的記憶體空間
    name = new char[strlen(source.name) + 1];

    // 2. 把來源內容 copy 過來
    strcpy(name, source.name);
}
```
## 🖼️ 圖解記憶體配置（淺層 vs 深層）

### 情境：`Person p1("Alice"); Person p2 = p1;`

### 淺層複製（錯誤）：
```sql
p1.name ──► 🧠"Alice"
p2.name ──┘   （共用同一塊）
```

### 深層複製（正確）：

```sql
p1.name ──► 🧠"Alice" p2.name ──► 🧠"Alice"（不同區塊，內容一樣）
```

## 💣 為什麼深層複製比較安全？

- 每個物件都有自己的記憶體空間。
    
- 彼此修改不會影響對方。
    
- 解構時 `delete[] name;` 不會出現 double free 的錯誤。
    
## 🧪 對照程式片段：
```cpp
Person p1("Alice"); 
Person p2 = p1;    // 呼叫深層複製建構子  
p1.print();        // Alice 
p2.print();        // Alice  
strcpy(p2.name, "Bob");  // 改 p2.name 的內容  
p1.print();        // Alice ✅ 沒被影響 
p2.print();        // Bob   ✅ 自己有自己的記憶體
```
---
# 🧪 範例：`Player` 類別，含複製建構子

```cpp
#include <iostream>
#include <string>
using namespace std;

class Player {
private:
    string name;
    int health;
    int xp;

public:
    // 普通建構子
    Player(string name_val, int health_val, int xp_val)
        : name{name_val}, health{health_val}, xp{xp_val} {
        cout << "Three-arg constructor called for " << name << endl;
    }

    // 複製建構子
    Player(const Player& source)
        : name{source.name}, health{source.health}, xp{source.xp} {
        cout << "Copy constructor - made copy of: " << source.name << endl;
    }

    // 顯示資訊
    void display() const {
        cout << "Player: " << name << ", Health: " << health << ", XP: " << xp << endl;
    }
};

int main() {
    Player p1{"Link", 100, 50};    // 呼叫普通建構子
    Player p2{p1};                 // 呼叫複製建構子
    Player p3 = p1;                // 同樣呼叫複製建構子

    p1.display();
    p2.display();
    p3.display();

    return 0;
}
```

---

### 🧠 執行結果分析（呼叫順序）：

```sql
Three-arg constructor called for Link
Copy constructor - made copy of: Link
Copy constructor - made copy of: Link
Player: Link, Health: 100, XP: 50
Player: Link, Health: 100, XP: 50
Player: Link, Health: 100, XP: 50
```

- 第一句：p1 用普通建構子建立
    
- 第二句：p2 使用 p1 複製 → 呼叫複製建構子
    
- 第三句：p3 使用 `=` 也會呼叫複製建構子（不是賦值運算子）
    

---
### 📌 重點回顧

|建構方式|呼叫哪個建構子|
|---|---|
|`Player p1{"Link", 100, 50}`|普通建構子|
|`Player p2{p1}`|✅ 複製建構子|
|`Player p3 = p1`|✅ 複製建構子（不是賦值）|
# 🤔 問題：為什麼複製建構子的參數**不能用傳值（by value）**？

### ✅ 正確的寫法（避免遞迴）
```cpp
class MyClass { public:     MyClass(const MyClass& other);  // ✔️ 傳參考，沒有複製發生 };
```
### ❌ 錯誤的寫法（會無限遞迴）

```cpp
class MyClass { public:     MyClass(MyClass other);  // ❌ 傳值（會觸發複製建構子） };
```

### 為什麼錯？
我們來分析這句話：
```cpp
MyClass obj2 = obj1;
```

你以為你只會呼叫一次 `MyClass(MyClass other)`，但其實...
### 🧨 呼叫順序拆解（無限遞迴）

1. **你寫了這行：**
    `MyClass obj2 = obj1;`
    
2. **這會呼叫複製建構子：**
    `MyClass(MyClass other)  // 注意：參數是傳值`
    
3. **為了傳值給 `other`，你必須「複製 obj1」** 👉 所以又會呼叫複製建構子來建立參數 `other`
    
4. **但複製建構子又是用傳值！又要複製！**
    
5. 💥 這個「為了傳值 → 又呼叫複製建構子 → 又要傳值 → 又呼叫...」會**無限遞迴**
    
## 🔁 示意流程圖（遞迴發生）
```
你呼叫 MyClass obj2 = obj1;

↓ 呼叫複製建構子
MyClass(MyClass other)

↓ 要複製 obj1 傳進來
又呼叫 MyClass(MyClass other)

↓ 要複製 obj1 傳進來
又呼叫 MyClass(MyClass other)

... 無限循環 💥
```

## ✅ 那為什麼「用參考」就沒事？

```cpp
MyClass(const MyClass& other);
```
- 傳進來的是「記憶體位置」（不是複製一份新的物件）
    
- 所以就不需要再呼叫複製建構子來產生這個參數
    
- 🔁 遞迴中斷，✅ 程式能正常編譯與執行
    
## 🔍 範例程式（會錯的版本）
```cpp
#include <iostream>
using namespace std;

class MyClass {
public:
    MyClass(MyClass other) {  // ❌ 傳值 → 觸發遞迴
        cout << "複製建構子\n";
    }
};

int main() {
    MyClass a;     // 錯：你沒寫預設建構子會先報錯
    MyClass b = a; // 🚨 如果前面修好，這行會導致無限遞迴
    return 0;
}
```
你會看到編譯器報錯，可能像這樣（GCC）：
```cpp
error: invalid initialization of reference of type ‘MyClass&’ from expression of type ‘MyClass’ note:   passing ‘MyClass’ as ‘this’ argument discards qualifiers
```

## ✅ 正確版本
```cpp
class MyClass {
public:
    MyClass() {}  // 預設建構子
    MyClass(const MyClass& other) {
        cout << "複製建構子\n";
    }
};
```
