# Wild Card
Wildcard（萬用字元）是一種在文字比對中常用的特殊字元，常見於 shell（例如 Bash）、搜尋功能、正則表達式、SQL 查詢等。它能幫助你快速比對一組字串，而不必完全列出每一個選項。

## 🧩 常見的 Wildcard 種類與用法：
以下依使用場景分別說明：

### 🔧 1. Shell 中的 Wildcard（以 Bash 為例）
| Wildcard | 意義            | 範例          | 說明                                         |
| -------- | ------------- | ----------- | ------------------------------------------ |
| `*`      | 任意數量（包含0）任意字元 | `*.txt`     | 比對所有副檔名為 `.txt` 的檔案，如 `a.txt`、`report.txt` |
| `?`      | 任意一個字元        | `?.txt`     | 比對只有一個字母開頭的 `.txt` 檔案，如 `a.txt`            |
| `[abc]`  | 任一個括號內的字元     | `[ab]*.txt` | 比對以 `a` 或 `b` 開頭的 `.txt` 檔案                |
| `[a-z]`  | 字元範圍          | `[a-c]*`    | 比對以 `a` 到 `c` 開頭的所有檔案                      |

### 📌 實際範例：

```bash
ls *.jpg          # 顯示所有 jpg 檔
ls file?.txt      # 比對 file1.txt、file2.txt 但不含 file10.txt
ls [0-9]*.sh      # 比對以數字開頭的 .sh 檔案
```
## 🔍 2. SQL 中的 Wildcard
SQL 中使用 LIKE 搭配萬用字元。

| Wildcard | 意義       | 範例                                       |
| -------- | -------- | ---------------------------------------- |
| `%`      | 任意數量任意字元 | `WHERE name LIKE 'A%'` → 開頭是 A 的名稱       |
| `_`      | 單一任意字元   | `WHERE id LIKE 'A_1'` → 比對 `A11`、`AB1` 等 |

### 📌 實際範例：

```sql
SELECT * FROM users WHERE email LIKE '%.edu';
SELECT * FROM products WHERE name LIKE '_ike';  -- 可能是 "bike", "like"
```
## 🧠 3. 正則表達式（Regular Expression）與 Wildcard 類似但更強大
雖然不是傳統意義上的 wildcard，但很多人會混用觀念。以下是正規表示法的一些對應用法：

| 正則語法 | 功能       | 範例                        |
| ---- | -------- | ------------------------- |
| `.`  | 任意單一字元   | `a.c` 可配對 `abc`, `axc`    |
| `.*` | 任意字元任意次數 | `a.*c` 可配對 `abc`, `axyzc` |
## 🧭 常見用途
檔案搜尋：
```bash
find . -name "*.cpp"
```
批次處理檔案：
```bash
mv *.jpg images/
```
SQL 查詢模糊名稱：
```sql
SELECT * FROM users WHERE username LIKE 'john%';
```
程式碼比對/過濾（如 grep）：
```bash
grep "error.*code" log.txt
```
---
# Hard Link & Soft Link
## 🔧 一、Hard Link（硬連結）

### 📌 定義

Hard link 是將**同一個檔案的 inode 編號賦予另一個檔名**，也就是說這兩個檔案名稱指向的是**相同的檔案內容（inode）**。
## ✅ 特性

- 多個硬連結共用同一個 inode（同一個實體檔案）。
    
- 刪除原始檔案不會影響其他 hard link。
    
- 不可以跨不同的檔案系統（filesystem）。
    
- 不可以對目錄建立 hard link（除非使用特殊方式如 `ln -d`，但一般會被禁止）。
    
## 📂 範例

```bash
$ echo "Hello" > fileA 
$ ln fileA fileB 
$ ls -li 
123456 -rw-r--r-- 2 user user 6 May 20 10:00 fileA 
123456 -rw-r--r-- 2 user user 6 May 20 10:00 fileB
```

- `fileA` 和 `fileB` 有相同的 inode（123456），代表它們是同一個實體檔案。
    
- 修改 `fileA` 的內容，`fileB` 的內容也會跟著變。
---

## 🔗 二、Soft Link（符號連結 / Symbolic Link）

### 📌 定義

Symbolic link 是一個**特殊的檔案，它只包含另一個檔案路徑的文字資訊**。可以把它想成是「捷徑」。

### ✅ 特性

- 是一個獨立的檔案，內容是原始檔案的路徑。
    
- 可以跨檔案系統。
    
- 可以對目錄建立符號連結。
    
- 如果原始檔案被刪除，symbolic link 會變成「壞掉的連結」（broken link）。
    

### 📂 範例
```bash
$ echo "Hello" > fileA 
$ ln -s fileA fileC 
$ ls -li 
123456 -rw-r--r-- 1 user user 6 May 20 10:00 fileA 
234567 lrwxrwxrwx 1 user user 5 May 20 10:00 fileC -> fileA
```
- `fileC` 是指向 `fileA` 的 symbolic link。
    
- 若刪除 `fileA`，`fileC` 將變成無效的捷徑
---
## 📋 差異總整理表

| 特性         | Hard Link       | Soft Link        |
| ---------- | --------------- | ---------------- |
| 指向目標       | 檔案的內容（inode）    | 檔案的名稱（路徑）        |
| 是否使用 inode | ✅ 共用 inode      | ❌ 自己有 inode      |
| 原始檔案刪除後    | 不影響其他 hard link | Link 會壞掉（Broken） |
| 是否跨檔案系統    | ❌ 不可            | ✅ 可跨檔案系統         |
| 是否可指向目錄    | ❌（限制）           | ✅ 可              |
| ls 標示方式    | 正常檔案            | `lrwxrwxrwx` 類型  |
## 🖼️ 形象比喻（ASCII Art）
### Hard Link（像一個房子有兩個門牌）：
```cpp
    [inode:1234]
        |
   --------------
   |            |
fileA        fileB
```
→ 無論用 fileA 還是 fileB，都進入同一間「房子」。

### Soft Link（像一個指標指向門牌）：
```cpp
fileC -> fileA -> [inode:1234]
```
→ fileC 是捷徑，只是指向 fileA；如果 fileA 被拆除，fileC 就指不到了。