```python
import pandas as pd
import numpy as np
```

# 案例1:學生成績分析

場景:某班級的學生成績資料如下,請完成以下任務:
1. 計算每位學生的總分和平均分。
2. 找出數學成績高於90分或英語成績高於85分的學生。
3. 按總分從高到低排序,並輸出前3名學生。

```python
import pandas as pd \
 data = {\
'姓名': ['張三','李四','王五','趙六','錢七'],\
'數學': [85,92, 78, 88, 95],\
'英語': [90, 88, 85, 92, 80],\
'物理': [75, 80, 88, 85, 90]\
}
```


```python
data = {
    '姓名': ['張三', '李四', '王五', '趙六', '錢七'],
    '數學': [85, 92, 78, 88, 95],
    '英語': [90, 88, 85, 92, 80],
    '物理': [75, 80, 88, 85, 90]
}
```


```python
scores = pd.DataFrame(data)
scores
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>姓名</th>
      <th>數學</th>
      <th>英語</th>
      <th>物理</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>張三</td>
      <td>85</td>
      <td>90</td>
      <td>75</td>
    </tr>
    <tr>
      <th>1</th>
      <td>李四</td>
      <td>92</td>
      <td>88</td>
      <td>80</td>
    </tr>
    <tr>
      <th>2</th>
      <td>王五</td>
      <td>78</td>
      <td>85</td>
      <td>88</td>
    </tr>
    <tr>
      <th>3</th>
      <td>趙六</td>
      <td>88</td>
      <td>92</td>
      <td>85</td>
    </tr>
    <tr>
      <th>4</th>
      <td>錢七</td>
      <td>95</td>
      <td>80</td>
      <td>90</td>
    </tr>
  </tbody>
</table>
</div>

```python
scores["Total"] = scores["數學", "英語", "物理"]("數學",%20"英語",%20"物理").sum(axis=1)
scores
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>姓名</th>
      <th>數學</th>
      <th>英語</th>
      <th>物理</th>
      <th>Total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>張三</td>
      <td>85</td>
      <td>90</td>
      <td>75</td>
      <td>250</td>
    </tr>
    <tr>
      <th>1</th>
      <td>李四</td>
      <td>92</td>
      <td>88</td>
      <td>80</td>
      <td>260</td>
    </tr>
    <tr>
      <th>2</th>
      <td>王五</td>
      <td>78</td>
      <td>85</td>
      <td>88</td>
      <td>251</td>
    </tr>
    <tr>
      <th>3</th>
      <td>趙六</td>
      <td>88</td>
      <td>92</td>
      <td>85</td>
      <td>265</td>
    </tr>
    <tr>
      <th>4</th>
      <td>錢七</td>
      <td>95</td>
      <td>80</td>
      <td>90</td>
      <td>265</td>
    </tr>
  </tbody>
</table>
</div>

```python
scores["Average"] = scores["數學", "英語", "物理"]("數學",%20"英語",%20"物理").mean(axis=1)
scores
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>姓名</th>
      <th>數學</th>
      <th>英語</th>
      <th>物理</th>
      <th>Total</th>
      <th>Average</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>張三</td>
      <td>85</td>
      <td>90</td>
      <td>75</td>
      <td>250</td>
      <td>83.333333</td>
    </tr>
    <tr>
      <th>1</th>
      <td>李四</td>
      <td>92</td>
      <td>88</td>
      <td>80</td>
      <td>260</td>
      <td>86.666667</td>
    </tr>
    <tr>
      <th>2</th>
      <td>王五</td>
      <td>78</td>
      <td>85</td>
      <td>88</td>
      <td>251</td>
      <td>83.666667</td>
    </tr>
    <tr>
      <th>3</th>
      <td>趙六</td>
      <td>88</td>
      <td>92</td>
      <td>85</td>
      <td>265</td>
      <td>88.333333</td>
    </tr>
    <tr>
      <th>4</th>
      <td>錢七</td>
      <td>95</td>
      <td>80</td>
      <td>90</td>
      <td>265</td>
      <td>88.333333</td>
    </tr>
  </tbody>
</table>
</div>

```python
scores[(scores["數學"] > 90) | (scores["英語"] > 85)]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>姓名</th>
      <th>數學</th>
      <th>英語</th>
      <th>物理</th>
      <th>Total</th>
      <th>Average</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>張三</td>
      <td>85</td>
      <td>90</td>
      <td>75</td>
      <td>250</td>
      <td>83.333333</td>
    </tr>
    <tr>
      <th>1</th>
      <td>李四</td>
      <td>92</td>
      <td>88</td>
      <td>80</td>
      <td>260</td>
      <td>86.666667</td>
    </tr>
    <tr>
      <th>3</th>
      <td>趙六</td>
      <td>88</td>
      <td>92</td>
      <td>85</td>
      <td>265</td>
      <td>88.333333</td>
    </tr>
    <tr>
      <th>4</th>
      <td>錢七</td>
      <td>95</td>
      <td>80</td>
      <td>90</td>
      <td>265</td>
      <td>88.333333</td>
    </tr>
  </tbody>
</table>
</div>

```python
scores.loc[(scores["數學"] > 90) | (scores["英語"] > 85), "姓名"]
```

   姓名  数学  英语  物理  Total    Average
0  张三  85  90  75    250  83.333333
1  李四  92  88  80    260  86.666667
3  赵六  88  92  85    265  88.333333
4  钱七  95  80  90    265  88.333333

```python
scores.sort_values("Total", ascending=False)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>姓名</th>
      <th>數學</th>
      <th>英語</th>
      <th>物理</th>
      <th>Total</th>
      <th>Average</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>錢七</td>
      <td>95</td>
      <td>80</td>
      <td>90</td>
      <td>265</td>
      <td>88.333333</td>
    </tr>
    <tr>
      <th>3</th>
      <td>趙六</td>
      <td>88</td>
      <td>92</td>
      <td>85</td>
      <td>265</td>
      <td>88.333333</td>
    </tr>
    <tr>
      <th>1</th>
      <td>李四</td>
      <td>92</td>
      <td>88</td>
      <td>80</td>
      <td>260</td>
      <td>86.666667</td>
    </tr>
    <tr>
      <th>2</th>
      <td>王五</td>
      <td>78</td>
      <td>85</td>
      <td>88</td>
      <td>251</td>
      <td>83.666667</td>
    </tr>
    <tr>
      <th>0</th>
      <td>張三</td>
      <td>85</td>
      <td>90</td>
      <td>75</td>
      <td>250</td>
      <td>83.333333</td>
    </tr>
  </tbody>
</table>
</div>

```python
scores.sort_values("Total", ascending=False).head(3)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>姓名</th>
      <th>數學</th>
      <th>英語</th>
      <th>物理</th>
      <th>Total</th>
      <th>Average</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>錢七</td>
      <td>95</td>
      <td>80</td>
      <td>90</td>
      <td>265</td>
      <td>88.333333</td>
    </tr>
    <tr>
      <th>3</th>
      <td>趙六</td>
      <td>88</td>
      <td>92</td>
      <td>85</td>
      <td>265</td>
      <td>88.333333</td>
    </tr>
    <tr>
      <th>1</th>
      <td>李四</td>
      <td>92</td>
      <td>88</td>
      <td>80</td>
      <td>260</td>
      <td>86.666667</td>
    </tr>
  </tbody>
</table>
</div>

```python
scores.nlargest(3, columns="Total")
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>姓名</th>
      <th>數學</th>
      <th>英語</th>
      <th>物理</th>
      <th>Total</th>
      <th>Average</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>趙六</td>
      <td>88</td>
      <td>92</td>
      <td>85</td>
      <td>265</td>
      <td>88.333333</td>
    </tr>
    <tr>
      <th>4</th>
      <td>錢七</td>
      <td>95</td>
      <td>80</td>
      <td>90</td>
      <td>265</td>
      <td>88.333333</td>
    </tr>
    <tr>
      <th>1</th>
      <td>李四</td>
      <td>92</td>
      <td>88</td>
      <td>80</td>
      <td>260</td>
      <td>86.666667</td>
    </tr>
  </tbody>
</table>
</div>
---
# Sales data analyzation

案例2:銷售資料分析
場景:某公司銷售資料如下,請完成以下任務:
1. 計算每種產品的總銷售額(銷售額單價×銷量)。
2. 找出銷售額最高的產品。
3. 按銷售額從高到低排序,並輸出所有產品資訊。
import pandas as pd

```python
data = {
'產品名稱': ['A', 'B', 'C', 'Dני,
'單價': [100, 150, 20,120],
'銷量': [50,30, 20, 40]
}
df = pd.DataFrame (data)
```


```python
data = {
    '產品名稱': ['A', 'B', 'C', 'D'],
    '單價': [100, 150, 20, 120],
    '銷量': [50, 30, 20, 40],
}
df = pd.DataFrame(data)
df
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>產品名稱</th>
      <th>單價</th>
      <th>銷量</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>100</td>
      <td>50</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>150</td>
      <td>30</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>20</td>
      <td>20</td>
    </tr>
    <tr>
      <th>3</th>
      <td>D</td>
      <td>120</td>
      <td>40</td>
    </tr>
  </tbody>
</table>
</div>

```python
df["Total sales price"] = df["單價"] * df["銷量"]
df
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>產品名稱</th>
      <th>單價</th>
      <th>銷量</th>
      <th>Total sales</th>
      <th>Total sales price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>100</td>
      <td>50</td>
      <td>5000</td>
      <td>5000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>150</td>
      <td>30</td>
      <td>4500</td>
      <td>4500</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>20</td>
      <td>20</td>
      <td>400</td>
      <td>400</td>
    </tr>
    <tr>
      <th>3</th>
      <td>D</td>
      <td>120</td>
      <td>40</td>
      <td>4800</td>
      <td>4800</td>
    </tr>
  </tbody>
</table>
</div>

```python
df["Total sales price"].max()
df.loc[df["Total sales price"] == df["Total sales price"].max(), "產品名稱"]
```

0    A
Name: 產品名稱, dtype: object




```python
df.sort_values("Total sales price", ascending=False)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>產品名稱</th>
      <th>單價</th>
      <th>銷量</th>
      <th>Total sales</th>
      <th>Total sales price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>100</td>
      <td>50</td>
      <td>5000</td>
      <td>5000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>D</td>
      <td>120</td>
      <td>40</td>
      <td>4800</td>
      <td>4800</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>150</td>
      <td>30</td>
      <td>4500</td>
      <td>4500</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>20</td>
      <td>20</td>
      <td>400</td>
      <td>400</td>
    </tr>
  </tbody>
</table>
</div>



## 案例3:電商使用者行為分析
場景:某電商平臺的使用者行為資料如下,請完成以下任務:
1. 計算每位使用者的總消費金額消費金額= 商品單價 購買數量)
2. 找出消費金額最高的使用者,並輸出其所有資訊
3. 計算所有使用者的平均消費金額(保留2位小數)
4. 統計電子產品的總購買數量
```
import pandas as pd
data {
    '使用者ID':【101, 102, 103, 104, 105),
    '使用者名稱':【'Alice', 'Bob', 'Charlie', 'David', 'Eve'],
    '商品類別':【電子產品”,“服飾','電子產品”,“家居”,“服飾'],
    '商品單價':【1200, 300, 889, 150, 200),
    '購買數量':【1, 3, 2, 5, 4]
df pd.DataFrame(data)
```


```python

```
