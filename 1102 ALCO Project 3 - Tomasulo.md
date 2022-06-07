
1102 ALCO Project 3 - Tomasulo
===


Tomasulo's Algorithm 
---
![](https://i.imgur.com/uv2ukjM.jpg)
* FP multipliers 的RS1, RS2編號改為`RS4`, `RS5`
* [圖片來源](https://www.researchgate.net/figure/1-The-basic-structure-of-a-MIPS-floating-point-unit-using-Tomasulos-algorithm_fig1_318502489)



Input  
---
1. 分別輸入 ADD/SUB, MUL, DIV Instuction的`cycle time`
2. 輸入一段RISC-V Instruction
3. 使用 `ctrl^z` 終止輸入
```gherkin=
ADDI F1, F2, 1
SUB F1, F3, F4
DIV F1, F2, F3
MUL F2, F3, F4
ADD F2, F4, F2
ADDI F4, F1, 2
MUL F5, F5, F5
ADD F1, F4, F4
```

![](https://i.imgur.com/zgqIBHn.png)

Output  
---
* 輸出所有有變化的`cycle`
* 包含`RF`, `RAT`, 兩組`RS` (ADD/SUB & MUL/DIV) , ALU `BUFFER`



流程說明
---
1. `void ReadInstruction()`:
   * 一行一行讀入輸入的Instruction
   * 依序push_back()到存Instruction的vector裡
   
2. `void Fetch()`: 
   * 順序擷取Instruction 
   * 順序解碼Instruction 
   * 把Instruction移除
   
3. `void Issue()`: 
   * 把每個Instruction送到對應的 RS  

4. `void Execute(ROB& buffer)`:  
    * RS準備完成, 開始execute : 依照 oprand 執行加減乘除運算

5. `void Commit(ROB& buffer)`: 
    * 把結果寫回RF, RAT以及RS
6. `void printCycle()`: 
    * 若該Cycle有改變就輸出該Cycle下的狀態

程式碼說明
---
* `RS1`, `RS2`, `RS3`為Adder的RS, `RS4`, `RS5` 為 Multiplier的RS
* 總共有5個Register: `F1`, `F2`, `F3`, `F4`, `F5` 
* `vector<int> RF = { 0, 0, 2, 4, 6, 8 }`: 
    預設有6個Register (F0 = 0)
* 初始RF裡的值
  
    | Reg name | Value |
    | -------- | ----- 
    |F0        |   0 |
    |F1        |   0 |  
    |F2        |   2 |
    |F3        |   4 |
    |F4        |   6 |
    |F5        |   8 |


* `vector<string> RAT(6)`:
    儲存所有Register Renaming的名稱


輸出結果
---
* `cycle 1:`
    ![](https://i.imgur.com/itCSVqr.png)
    
* `cycle 2 & 3:`
    ![](https://i.imgur.com/BH9HJgF.png)
    
* `cycle 4 & 5:`
    ![](https://i.imgur.com/86eAcI2.png)

* `cycle 6 & 7:`
    ![](https://i.imgur.com/A35qJTE.png)
    
* `cycle 8 & 9:`
    ![](https://i.imgur.com/c6xJN4T.png)


* `cycle 10 & 11:`
    ![](https://i.imgur.com/ivatLSU.png)



* `cycle 12:`
    ![](https://i.imgur.com/h2CGQu5.png)




