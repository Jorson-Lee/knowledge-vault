好的，這份 PDF 檔案中關於「新模型最佳化」的數學式整理如下，已使用 LaTeX 語法呈現：

### 1. 模型 (Model)

- **線性模型 (Linear Model)**
    
    - y=b+wx1​ 1
        
    - 其中，
        
        y 為觀看次數，x1​ 為前一天的觀看次數，b 和 w 為未知參數（權重和偏差） 2。
        
- **新模型 (New Model)**
    
    - 基於 Sigmoid 函數的模型：
        
        y=b+∑i​ci​sigmoid(bi​+∑j​wij​xj​) 3
        
    - 基於 ReLU (Rectified Linear Unit) 函數的模型：
        
        y=b+∑i​ci​max(0,bi​+∑j​wij​xj​) 4
        

### 2. 損失函數 (Loss Function)

- **一般形式**
    
    - 損失是參數的函數：
        
        L(b,w) 5
        
    - 誤差：
        
        e=y−y^​ 6
        
- **均方誤差 (Mean Square Error, MSE)**
    
    - L=N1​∑n​(yn−y^​n)2 7
        
- **平均絕對誤差 (Mean Absolute Error, MAE)**
    
    - L=N1​∑n​∣yn−y^​n∣ 8
        

### 3. 最佳化 (Optimization)

- **目標**
    
    - 找到能使損失函數最小化的參數：
        
        w∗,b∗=argminw,b​L 9
        
- **梯度下降法 (Gradient Descent)**
    
    - **更新規則 (單一參數)**：w1←w0−η∂w∂L​∣w=w0​ 10
        
    - **更新規則 (多個參數)**：
        
        - w1←w0−η∂w∂L​∣w=w0,b=b0​ 11
            
        - b1←b0−η∂b∂L​∣w=w0,b=b0​ 12
            
    - **向量化形式**
        
        - θ1←θ0−ηg 13
            
        - 其中，
            
            g 為梯度向量：g=∇L(θ0) 14
