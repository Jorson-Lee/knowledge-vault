# [判斷連結串列是否包含環](https://labuladong.online/algo/essential-technique/linked-list-skills-summary-2/#%E5%88%A4%E6%96%AD%E9%93%BE%E8%A1%A8%E6%98%AF%E5%90%A6%E5%8C%85%E5%90%AB%E7%8E%AF)

判斷連結串列是否包含環屬於經典問題了，解決方案也是用快慢指標：

每當慢指標 `slow` 前進一步，快指標 `fast` 就前進兩步。

如果 `fast` 最終能正常走到連結串列末尾，說明連結串列中沒有環；如果 `fast` 走著走著竟然和 `slow` 相遇了，那肯定是 `fast` 在連結串列中轉圈了，說明連結串列中含有環。
```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {
        // 快慢指標初始化指向 head
        ListNode *slow = head, *fast = head;
        // 快指標走到末尾時停止
        while (fast != nullptr && fast->next != nullptr) {
            // 慢指標走一步，快指標走兩步
            slow = slow->next;
            fast = fast->next->next;
            // 快慢指標相遇，說明含有環
            if (slow == fast) {
                return true;
            }
        }
        // 不包含環
        return false;
    }
};
```
當然，這個問題還有進階版，也是力扣第 142 題「[環形連結串列 II](https://leetcode.cn/problems/linked-list-cycle-ii/)」：如果連結串列中含有環，如何計算這個環的起點？

舉個例子，環的起點是指下面這幅圖中的節點 2：
![Pasted image 20250825121629](06%20-%20Machine%20Learning/attachments/Pasted%20image%2020250825121629.png)

可以看到，當快慢指標相遇時，讓其中任一個指標指向頭節點，然後讓它倆以相同速度前進，再次相遇時所在的節點位置就是環開始的位置。

為什麼要這樣呢？這裡簡單說一下其中的原理。

我們假設快慢指標相遇時，慢指標 `slow` 走了 `k` 步，那麼快指標 `fast` 一定走了 `2k` 步：
![400](06%20-%20Machine%20Learning/attachments/Pasted%20image%2020250825121217.png)

`fast` 一定比 `slow` 多走了 `k` 步，這多走的 `k` 步其實就是 `fast` 指標在環裡轉圈圈，所以 `k` 的值就是環長度的「整數倍」。

假設相遇點距環的起點的距離為 `m`，那麼結合上圖的 `slow` 指標，環的起點距頭結點 `head` 的距離為 `k - m`，也就是說如果從 `head` 前進 `k - m` 步就能到達環起點。

巧的是，如果從相遇點繼續前進 `k - m` 步，也恰好到達環起點。因為結合上圖的 `fast` 指標，從相遇點開始走k步可以轉回到相遇點，那走 `k - m` 步肯定就走到環起點了：
![400](06%20-%20Machine%20Learning/attachments/Pasted%20image%2020250825121236.png)

所以，只要我們把快慢指標中的任一個重新指向 `head`，然後兩個指標同速前進，`k - m` 步後一定會相遇，相遇之處就是環的起點了。

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode *fast, *slow;
        fast = slow = head;
        while (fast != nullptr && fast->next != nullptr) {
            fast = fast->next->next;
            slow = slow->next;
            if (fast == slow) break;
        }
        // 上面的程式碼類似 hasCycle 函式
        if (fast == nullptr || fast->next == nullptr) {
            // fast 遇到空指標說明沒有環
            return nullptr;
        }

        // 重新指向頭結點
        slow = head;
        // 快慢指標同步前進(k-m步)，相交點就是環起點
        while (slow != fast) {
            fast = fast->next;
            slow = slow->next;
        }
        return slow;
    }
};
```
# [兩個連結串列是否相交](https://labuladong.online/algo/essential-technique/linked-list-skills-summary-2/#%E4%B8%A4%E4%B8%AA%E9%93%BE%E8%A1%A8%E6%98%AF%E5%90%A6%E7%9B%B8%E4%BA%A4)
給你輸入兩個連結串列的頭結點 `headA` 和 `headB`，這兩個連結串列可能存在相交。

如果相交，你的演算法應該返回相交的那個節點；如果沒相交，則返回 null。

比如題目給我們舉的例子，如果輸入的兩個連結串列如下圖：
![400](06%20-%20Machine%20Learning/attachments/Pasted%20image%2020250825121755.png)
那么我们的算
那麼我們的演算法應該返回 `c1` 這個節點。

這個題直接的想法可能是用 `HashSet` 記錄一個連結串列的所有節點，然後和另一條連結串列對比，但這就需要額外的空間。

如果不用額外的空間，只使用兩個指標，你如何做呢？
難點在於，由於兩條連結串列的長度可能不同，兩條連結串列之間的節點無法對應：
![400](06%20-%20Machine%20Learning/attachments/Pasted%20image%2020250825122205.png)

如果用兩個指標 `p1` 和 `p2` 分別在兩條連結串列上前進，並不能**同時**走到公共節點，也就無法得到相交節點 `c1`。

**解決這個問題的關鍵是，透過某些方式，讓 `p1` 和 `p2` 能夠同時到達相交節點 `c1`**。

所以，我們可以讓 `p1` 遍歷完連結串列 `A` 之後開始遍歷連結串列 `B`，讓 `p2` 遍歷完連結串列 `B` 之後開始遍歷連結串列 `A`，這樣相當於「邏輯上」兩條連結串列接在了一起。

如果這樣進行拼接，就可以讓 `p1` 和 `p2` 同時進入公共部分，也就是同時到達相交節點 `c1`：

![400](06%20-%20Machine%20Learning/attachments/Pasted%20image%2020250825122324.png)

那你可能會問，如果說兩個連結串列沒有相交點，是否能夠正確的返回 null 呢？

這個邏輯可以覆蓋這種情況的，相當於 c1 節點是 null 空指標嘛，可以正確返回 null。
```cpp
class Solution {
public:
    ListNode* getIntersectionNode(ListNode* headA, ListNode* headB) {
        // p1 指向 A 链表头结点，p2 指向 B 链表头结点
        ListNode* p1 = headA;
        ListNode* p2 = headB;
        while (p1 != p2) {
            // p1 走一步，如果走到 A 链表末尾，转到 B 链表
            p1 = (p1 == nullptr) ? headB : p1->next;
            // p2 走一步，如果走到 B 链表末尾，转到 A 链表
            p2 = (p2 == nullptr) ? headA : p2->next;
        }
        return p1;
    }
};
```
## 長度差距法

> 當我們讓較長的鏈結串列先「跳過」幾個節點，會不會錯過交會點？
21
### 📘 為什麼不會錯過交會點

### 🔹 前提條件

設：

- `lenA = lenA_pre + lenC`
    
- `lenB = lenB_pre + lenC`  
    其中：
    
- `lenA_pre` = A 在交點之前的獨立長度
    
- `lenB_pre` = B 在交點之前的獨立長度
    
- `lenC` = 交點之後共享的長度


### 🔹 具體流程

1. 計算 `lenA`, `lenB`。
    
2. 設 `diff = |lenA - lenB|`。
    
3. 讓 **較長的鏈結串列** 先走 `diff` 步。
    
    - 如果 `lenA > lenB` → A 先走 `lenA - lenB` 步。
        
    - 如果 `lenB > lenA` → B 先走 `lenB - lenA` 步。
        
4. 之後兩個指標同時往前走，直到遇到相同節點（或 `nullptr`）。
    
### 🔹 為什麼不會跳過交會點

關鍵觀察：

- 交會點一定出現在 **尾端共享區段 (`lenC`)**。
    
- 我們讓較長的鏈結串列先「走差距 `diff`」，就是為了讓 **兩個指標剩下的步數一樣多**。
    

數學上：

假設 `lenA > lenB`：

- A 走 `lenA - lenB` 步後，剩下的長度 = `(lenB_pre + lenC)`。
    
- B 還沒走，剩下的長度也 = `(lenB_pre + lenC)`。
    

👉 所以此時 A 和 B **距離交會點的距離相同**。  
他們之後同步前進，不可能錯過交會點，只可能在交會點相遇，或最後一起到 `nullptr`。

### 🔹 ASCII 視覺化

範例：

```
A: a1 → a2 → a3
                  ↘
                   c1 → c2 → c3
                  ↗
B: b1
```

長度：

- `lenA = 5` (a1,a2,a3,c1,c2,c3)
    
- `lenB = 4` (b1,c1,c2,c3)
    
- `diff = 1`
    

步驟：

1. 讓 A 先走 1 步（到 a2）。
    
2. 現在 A 與 B 剩下的長度都 = 4。
    
3. 同步走：
    
    - A: a2 → a3 → c1 → c2 → c3
        
    - B: b1 → c1 → c2 → c3
        
4. 在 `c1` 相遇 ✅
    

👉 交會點一定不會被跳過。

### ✅ 結論

不會錯過交會點，原因是 **調整步數後，兩個指標距離交會點的距離相等**，因此同步移動時要嘛同時到達交會點，要嘛同時到 `nullptr`。
