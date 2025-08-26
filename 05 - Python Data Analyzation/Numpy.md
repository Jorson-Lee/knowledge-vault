# 📌 01 Numpy介紹

NumPy（Numerical Python）是 Python 中進行科學計算的基礎套件，它提供：

- 高效能的多維陣列（ndarray）物件
    
- 各種數學運算（如線性代數、傅立葉變換、隨機數生成）
    
- 廣泛的向量化操作與廣播功能（Broadcasting）
    

NumPy 的核心是 `ndarray`，它比 Python 原生的 list 在資料儲存與運算上更高效，適合用於大規模資料處理與矩陣操作。

---

# 📌 02 ndarray

## 📎 `numpy.ndarray`

`ndarray` 是 NumPy 中的核心資料結構，用於儲存同質資料的多維陣列。

### 建立 ndarray

```python
import numpy as np

# 一維陣列
arr1 = np.array([1, 2, 3])

# 二維陣列
arr2 = np.array([1, 2](1,%202))
```

## 屬性（Properties）

- `ndarray.ndim`：陣列的維度
    
- `ndarray.shape`：每個維度的大小（row, column, ...）
    
- `ndarray.size`：元素總數
    
- `ndarray.dtype`：陣列中元素的資料型別
    

```python
arr = np.array([1, 2, 3](1,%202,%203))
print(arr.ndim)   # 2
print(arr.shape)  # (2, 3)
print(arr.size)   # 6
print(arr.dtype)  # int64 (依平臺不同略有不同)
```

## 索引與切片

```python
print(arr[0, 1])    # 2
print(arr[:, 1])    # [2 5]
print(arr[1, :2])   # [4 5]
```

---

# 📌 03 Numpy常用函式

## ✅ `np.max()`

回傳陣列中的最大值。

```python
arr = np.array([1, 3, 7, 2])
print(np.max(arr))  # 7
```

## ✅ `np.argmax()`

回傳最大值所在的 index。

```python
print(np.argmax(arr))  # 2
```

## ✅ `np.where(condition)`

回傳符合條件的 index。

```python
arr = np.array([10, 20, 30, 40])
print(np.where(arr > 25))  # (array([2, 3]),)
```

## ✅ `np.count_nonzero()`

計算非零元素數量。

```python
arr = np.array([0, 1, 2, 0, 3])
print(np.count_nonzero(arr))  # 3
```

## ✅ `np.unique()`

取得唯一值，並可加上 `return_counts=True` 得到每個值的次數。

```python
arr = np.array([1, 2, 2, 3, 3, 3])
values, counts = np.unique(arr, return_counts=True)
print(values)  # [1 2 3]
print(counts)  # [1 2 3]
```

## ✅ `np.cumsum()`

累加總和。

```python
arr = np.array([1, 2, 3])
print(np.cumsum(arr))  # [1 3 6]
```

## ✅ `np.sort()`

排序陣列（不改變原陣列）。

```python
arr = np.array([3, 1, 2])
print(np.sort(arr))  # [1 2 3]
```

## ✅ `np.argsort()`

回傳排序後的 index。

```python
arr = np.array([30, 10, 20])
print(np.argsort(arr))  # [1 2 0]
```

## ✅ `np.amin()` / `np.min()`

回傳最小值。

```python
arr = np.array([5, 3, 8])
print(np.min(arr))  # 3
```

## ✅ `np.amax()` / `np.max()`

回傳最大值。

```python
print(np.max(arr))  # 8
```

## ✅ `np.unique(..., return_index=True)`

找出唯一值及其第一次出現的 index。

```python
arr = np.array([3, 1, 3, 2, 2])
values, index = np.unique(arr, return_index=True)
print(values)  # [1 2 3]
print(index)   # [1 3 0]
```

# 📌 04 案例練習

## 🧪 範例：找出總銷售額最高的產品

```python
import numpy as np

產品 = np.array(['A', 'B', 'C', 'D'])
單價 = np.array([100, 150, 20, 120])
銷量 = np.array([50, 30, 20, 40])

總銷售額 = 單價 * 銷量

# 找最大值的 index
index = np.argmax(總銷售額)

# 取得產品名稱
print(產品[index])  # A
```

## 🧪 範例：過濾條件篩選（語文成績 > 85 或 數學成績 > 90）

```python
姓名 = np.array(['張三', '李四', '王五', '趙六', '錢七'])
數學 = np.array([85, 92, 78, 88, 95])
英語 = np.array([90, 88, 85, 92, 80])

條件 = (數學 > 90) | (英語 > 85)
print(姓名[條件])  # ['李四' '趙六' '錢七']
```
