---
Up:
  - "[[Beginning C++ Programming - From Beginner to Beyond]]"
---
# Section Overview 
## Expressions, Statements and Operators
- Expressions
- Statements and block statements
- Operators
## Assignment
- Arithmetic
- Increment and decrement
- Equality
- Relational
- Logical
- Compound assignment
- Precedence
---
# 表達式(Expression)與陳述式(Statement)？

最好分辨表達式(Expression)及陳述式(Statement)的方法就是判斷_**有沒有回傳結果**_

### 陳述式 ( Statement )

- 命令，並且進行一系列操作，特徵為**不會回傳一個結果**
- ex : 流程控制 ( if else、switch case、while… )、宣告、函數 ( function ) 與類別（Class）、迭代（Iteration）  
    **ex :**  
    
    ![](https://i.imgur.com/FYxcEXz.png)

### 表達式 ( Expression )

- 也可以稱為表示式、運算式，經常透過一些符號 ( 運算子) 結合上下語句並運算及**回傳結果**
	**ex:**  
    
    ![](https://i.imgur.com/hgx5agc.png)

### 函式陳述式 ( 也叫具名函式 )

- 單純只有函式敘述，有函式名稱  
    **ex:**  
    
    ![](https://i.imgur.com/sPopQ13.png)

### 函式表達式 ( 也叫匿名函式 )

- 用變數指向沒有名稱的涵式  
    **ex:**  
    
    ![](https://i.imgur.com/6rj8fYN.png)
---
# Operator
## Assign Operator
```cpp
int num1 {10};
int num2 {20};

num1 = num2 = 1000;//assign由右往左
//result num1 = 1000,num2 = 1000
```
## Increment and Decrement Operator
```cpp
int num1 {10};
int num2 {20};
num1 = num2++ + 10;
//result num1 = 20 + 10 = 30, num2 = 21
```
```cpp

int num1 {10};
int num2 {20};
num1 = 10 + ++num2;
//result num1 = 10 + (20+1) = 31, num2 = 21 
```
---
# Mixed Type Expressions
## Conversions
Higher vs. Lower types are based on the size of the values the type can hold
- long double,double,float,unsigned long,long,unsigned int,int
- short and char types are always converted to int
## Type Coercion(強制): conversion of one operand to another data type
### Promotion: conversion to a higher type
Used in mathematical expressions
### Demotion: conversion to a lower type
Used with assignment to lower type
範例:
```cpp
//lower op higher the lower is promoted to a higher
2 * 5.2
//2 is promoted to 2.0
//lower = higher; the higher is demoted to a lower
int num {0};
num = 100.2;
//result num = 100
```
## Explicit Type Casting - static_cast`<type`>
```cpp
int total amount {100};
int total number {8};
double average {0.0};
average total amount / total number; =
cout << average << endl;
// displays 12
average static cast<double>(total amount) / total number; =
cout << average << endl;
// displays 12.5
```
# Logical Operators
## Precedence
**not** has higher precedence than **and**
**and** has higher precedence than **or**
**not** is a unary operator
**and** and **or** are binary operators