# 需求文件

## 簡介

Flappy Kiro 是一款在瀏覽器中運行的復古風格橫向無限滾動遊戲。玩家控制一個幽靈角色，透過點擊滑鼠或按下空白鍵讓幽靈向上飛，並引導幽靈穿越從上下方延伸的綠色管道。遊戲採用淡藍色手繪素描風格視覺效果，底部顯示當前分數與最高分數。碰到管道或畫面邊界時遊戲結束。

## 詞彙表

- **遊戲（Game）**：整個 Flappy Kiro 瀏覽器遊戲應用程式
- **幽靈（Ghost）**：玩家控制的角色，使用 `assets/ghosty.png` 圖片
- **管道（Pipe）**：從畫面上下方延伸的綠色障礙物，中間留有間隙
- **間隙（Gap）**：上下管道之間供幽靈通過的空間
- **重力（Gravity）**：持續將幽靈向下拉的物理力
- **跳躍（Jump）**：玩家觸發的向上推力動作
- **分數（Score）**：玩家成功通過的管道組數
- **最高分（High Score）**：本次遊戲會話中達到的最高分數
- **碰撞（Collision）**：幽靈與管道或畫面邊界接觸的事件
- **遊戲結束畫面（Game Over Screen）**：碰撞後顯示的結束介面
- **畫布（Canvas）**：遊戲渲染所使用的 HTML Canvas 元素
- **噴射背包（Jetpack）**：可收集的道具物品，使用 `jetpack.png` 圖片
- **加速狀態（Acceleration State）**：幽靈收集噴射背包後進入的特殊狀態
- **加速持續時間（Acceleration Duration）**：加速狀態持續的時間長度，約 5 秒
- **無敵狀態（Invincibility）**：加速狀態期間幽靈可穿越管道而不觸發碰撞
- **火焰粒子（Flame Particle）**：噴射背包啟動時從背包發射的視覺效果粒子
- **速度線（Speed Line）**：加速期間顯示的視覺效果，強化速度感
- **彩虹軌跡（Rainbow Trail）**：加速期間幽靈身後顯示的彩色軌跡效果
- **分數倍增器（Score Multiplier）**：加速期間通過管道時的分數加成，為 2 倍
- **倒數計時器（Countdown Timer）**：顯示剩餘加速時間的 UI 元素
- **噴射背包統計（Jetpack Statistics）**：追蹤收集的噴射背包總數與最長加速連擊數

---

## 需求

### 需求 1：遊戲初始化與畫面渲染

**使用者故事：** 身為玩家，我希望開啟網頁後能看到遊戲畫面，以便立即開始遊玩。

#### 驗收標準

1. THE 遊戲（Game）SHALL 在瀏覽器中透過單一 HTML 檔案運行，無需任何伺服器或安裝。
2. THE 遊戲（Game）SHALL 使用 HTML Canvas 元素渲染所有遊戲畫面。
3. THE 遊戲（Game）SHALL 以淡藍色為背景色渲染畫布。
4. THE 遊戲（Game）SHALL 在背景上渲染手繪素描風格的紋理效果，以呈現復古視覺風格。
5. THE 遊戲（Game）SHALL 在背景中渲染白色圓角矩形作為雲朵裝飾元素。
6. THE 遊戲（Game）SHALL 在畫布底部渲染深色橫條作為分數顯示區域。
7. WHEN 遊戲載入完成，THE 遊戲（Game）SHALL 顯示等待玩家輸入的開始提示畫面。

---

### 需求 2：幽靈角色物理行為

**使用者故事：** 身為玩家，我希望幽靈能受重力影響自然下墜，並在我操作時向上飛，以便體驗流暢的飛行手感。

#### 驗收標準

1. WHILE 遊戲進行中，THE 幽靈（Ghost）SHALL 持續受到向下的重力加速度影響。
2. WHILE 遊戲進行中，THE 幽靈（Ghost）SHALL 使用 `assets/ghosty.png` 圖片渲染於畫布上。
3. WHEN 玩家按下空白鍵或點擊畫布，THE 幽靈（Ghost）SHALL 獲得一個固定大小的向上速度（跳躍推力）。
4. WHEN 跳躍動作觸發，THE 遊戲（Game）SHALL 播放 `assets/jump.wav` 音效。
5. THE 幽靈（Ghost）SHALL 在每個畫面更新時根據當前速度與重力更新其垂直位置。
6. THE 幽靈（Ghost）SHALL 保持固定的水平位置，不隨畫面滾動移動。

---

### 需求 3：管道生成與滾動

**使用者故事：** 身為玩家，我希望管道能持續從右側出現並向左滾動，以便體驗無限滾動的挑戰。

#### 驗收標準

1. WHILE 遊戲進行中，THE 遊戲（Game）SHALL 以固定時間間隔在畫布右側生成新的管道組（上管道與下管道各一）。
2. THE 管道（Pipe）SHALL 以綠色渲染，並從畫布上下邊緣向內延伸。
3. THE 管道（Pipe）SHALL 在上下管道之間保留固定大小的間隙（Gap）供幽靈通過。
4. WHILE 遊戲進行中，THE 管道（Pipe）SHALL 以固定速度向左移動。
5. WHEN 管道完全移出畫布左側邊界，THE 遊戲（Game）SHALL 將該管道從記憶體中移除。
6. THE 遊戲（Game）SHALL 隨機決定每組管道的垂直位置，使間隙出現在不同高度。

---

### 需求 4：碰撞偵測與遊戲結束

**使用者故事：** 身為玩家，我希望碰到管道或邊界時遊戲能立即結束，以便了解我的失誤。

#### 驗收標準

1. WHEN 幽靈（Ghost）的邊界框與任一管道（Pipe）的邊界框重疊，THE 遊戲（Game）SHALL 觸發遊戲結束事件。
2. WHEN 幽靈（Ghost）的垂直位置超出畫布上邊界或下邊界，THE 遊戲（Game）SHALL 觸發遊戲結束事件。
3. WHEN 遊戲結束事件觸發，THE 遊戲（Game）SHALL 播放 `assets/game_over.wav` 音效。
4. WHEN 遊戲結束事件觸發，THE 遊戲（Game）SHALL 停止所有遊戲物件的更新與移動。
5. WHEN 遊戲結束事件觸發，THE 遊戲（Game）SHALL 在畫布上顯示遊戲結束畫面，包含最終分數與重新開始提示。

---

### 需求 5：計分系統

**使用者故事：** 身為玩家，我希望能看到當前分數與最高分，以便追蹤我的進步。

#### 驗收標準

1. WHEN 幽靈（Ghost）成功通過一組管道（Pipe）的間隙，THE 遊戲（Game）SHALL 將分數（Score）增加 1。
2. THE 遊戲（Game）SHALL 在畫布底部深色橫條中以「Score: X | High: X」格式持續顯示當前分數與最高分。
3. WHEN 當前分數（Score）超過最高分（High Score），THE 遊戲（Game）SHALL 將最高分更新為當前分數。
4. WHEN 遊戲重新開始，THE 遊戲（Game）SHALL 將當前分數重置為 0，但保留最高分不變。
5. THE 遊戲（Game）SHALL 在同一瀏覽器會話期間保留最高分記錄。

---

### 需求 6：遊戲重新開始

**使用者故事：** 身為玩家，我希望遊戲結束後能快速重新開始，以便繼續挑戰更高分數。

#### 驗收標準

1. WHEN 遊戲結束畫面顯示時，THE 遊戲（Game）SHALL 接受玩家的點擊或空白鍵輸入作為重新開始指令。
2. WHEN 重新開始指令觸發，THE 遊戲（Game）SHALL 將幽靈（Ghost）重置至初始位置與速度。
3. WHEN 重新開始指令觸發，THE 遊戲（Game）SHALL 清除所有現有管道（Pipe）並重新開始生成。
4. WHEN 重新開始指令觸發，THE 遊戲（Game）SHALL 將當前分數重置為 0。
5. WHEN 重新開始指令觸發，THE 遊戲（Game）SHALL 立即進入遊戲進行狀態。

---

### 需求 7：音效系統

**使用者故事：** 身為玩家，我希望遊戲有音效回饋，以便增強遊戲體驗。

#### 驗收標準

1. WHEN 跳躍動作觸發，THE 遊戲（Game）SHALL 播放 `assets/jump.wav` 音效。
2. WHEN 遊戲結束事件觸發，THE 遊戲（Game）SHALL 播放 `assets/game_over.wav` 音效。
3. IF 瀏覽器音效播放失敗，THEN THE 遊戲（Game）SHALL 繼續正常運行而不中斷遊戲流程。

---

### 需求 8：噴射背包道具生成與收集

**使用者故事：** 身為玩家，我希望在遊戲中隨機出現噴射背包道具，以便收集後獲得特殊能力。

#### 驗收標準

1. WHILE 遊戲進行中，THE 遊戲（Game）SHALL 在通過約每 3 至 5 組管道後於畫布右側生成一個噴射背包（Jetpack）道具。
2. THE 噴射背包（Jetpack）SHALL 使用 `jetpack.png` 圖片渲染於畫布上。
3. THE 噴射背包（Jetpack）SHALL 在隨機高度位置生成，以增加收集挑戰性。
4. WHILE 遊戲進行中，THE 噴射背包（Jetpack）SHALL 以與管道相同的速度向左移動。
5. WHEN 噴射背包完全移出畫布左側邊界，THE 遊戲（Game）SHALL 將該噴射背包從記憶體中移除。
6. WHEN 幽靈（Ghost）的邊界框與噴射背包（Jetpack）的邊界框重疊，THE 遊戲（Game）SHALL 觸發噴射背包收集事件並移除該噴射背包。
7. THE 噴射背包（Jetpack）SHALL 以閃爍或發光效果渲染，以提高視覺可見度。

---

### 需求 9：噴射背包加速效果

**使用者故事：** 身為玩家，我希望收集噴射背包後獲得速度提升與無敵能力，以便體驗刺激的加速遊玩。

#### 驗收標準

1. WHEN 噴射背包收集事件觸發，THE 遊戲（Game）SHALL 將幽靈（Ghost）切換至加速狀態（Acceleration State）。
2. WHILE 加速狀態（Acceleration State）啟用，THE 幽靈（Ghost）SHALL 以增加的水平速度向右移動，使管道與背景相對加速向左滾動。
3. WHILE 加速狀態（Acceleration State）啟用，THE 幽靈（Ghost）SHALL 進入無敵狀態（Invincibility），可穿越管道（Pipe）而不觸發碰撞（Collision）。
4. THE 加速狀態（Acceleration State）SHALL 持續約 5 秒的加速持續時間（Acceleration Duration）。
5. WHEN 加速持續時間結束，THE 遊戲（Game）SHALL 將幽靈（Ghost）恢復至正常速度與碰撞偵測狀態。
6. WHEN 加速狀態啟用期間收集另一個噴射背包，THE 遊戲（Game）SHALL 延長加速持續時間或提供額外獎勵。

---

### 需求 10：噴射背包視覺效果

**使用者故事：** 身為玩家，我希望噴射背包啟動時有明顯的視覺效果，以便清楚知道加速狀態已啟用。

#### 驗收標準

1. WHILE 加速狀態（Acceleration State）啟用，THE 遊戲（Game）SHALL 從噴射背包位置發射火焰粒子（Flame Particle）。
2. THE 火焰粒子（Flame Particle）SHALL 以動畫方式向後移動並逐漸消失，模擬火箭引擎效果。
3. WHILE 加速狀態（Acceleration State）啟用，THE 遊戲（Game）SHALL 在幽靈周圍渲染速度線（Speed Line），以強化速度感。
4. WHILE 加速狀態（Acceleration State）啟用，THE 遊戲（Game）SHALL 在幽靈身後渲染彩虹軌跡（Rainbow Trail）。
5. THE 彩虹軌跡（Rainbow Trail）SHALL 以漸變色彩與淡出效果渲染，隨時間逐漸消失。

---

### 需求 11：噴射背包音效

**使用者故事：** 身為玩家，我希望噴射背包啟動時有音效回饋，以便增強沉浸感。

#### 驗收標準

1. WHEN 噴射背包收集事件觸發，THE 遊戲（Game）SHALL 播放噴射背包啟動音效，模擬火箭引擎啟動聲。
2. WHILE 加速狀態（Acceleration State）啟用，THE 遊戲（Game）SHALL 播放特殊背景音樂或音效循環。
3. WHEN 加速持續時間結束，THE 遊戲（Game）SHALL 停止加速期間的特殊音效。
4. IF 瀏覽器音效播放失敗，THEN THE 遊戲（Game）SHALL 繼續正常運行而不中斷遊戲流程。

---

### 需求 12：噴射背包計分加成

**使用者故事：** 身為玩家，我希望在加速狀態期間獲得額外分數，以便獎勵我收集噴射背包的努力。

#### 驗收標準

1. WHILE 加速狀態（Acceleration State）啟用，THE 遊戲（Game）SHALL 將通過管道的分數以 2 倍分數倍增器（Score Multiplier）計算。
2. WHEN 幽靈（Ghost）在加速狀態期間成功通過一組管道，THE 遊戲（Game）SHALL 將分數（Score）增加 2 點而非 1 點。
3. WHEN 加速持續時間結束，THE 遊戲（Game）SHALL 將分數倍增器恢復為正常的 1 倍。

---

### 需求 13：噴射背包倒數計時器顯示

**使用者故事：** 身為玩家，我希望看到剩餘加速時間，以便規劃我的遊玩策略。

#### 驗收標準

1. WHILE 加速狀態（Acceleration State）啟用，THE 遊戲（Game）SHALL 在畫布上顯示倒數計時器（Countdown Timer）。
2. THE 倒數計時器（Countdown Timer）SHALL 以秒為單位顯示剩餘加速持續時間。
3. THE 倒數計時器（Countdown Timer）SHALL 每幀更新，以即時反映剩餘時間。
4. WHEN 加速持續時間結束，THE 遊戲（Game）SHALL 隱藏倒數計時器。

---

### 需求 14：噴射背包統計追蹤

**使用者故事：** 身為玩家，我希望追蹤我收集的噴射背包數量與連擊記錄，以便了解我的遊玩成就。

#### 驗收標準

1. THE 遊戲（Game）SHALL 追蹤本次遊戲會話中收集的噴射背包總數。
2. THE 遊戲（Game）SHALL 追蹤最長加速連擊數，定義為連續收集噴射背包而不讓加速狀態中斷的次數。
3. THE 遊戲（Game）SHALL 在遊戲結束畫面（Game Over Screen）顯示噴射背包統計（Jetpack Statistics），包含收集總數與最長連擊數。
4. WHEN 遊戲重新開始，THE 遊戲（Game）SHALL 重置當前遊戲的噴射背包統計，但保留會話最佳記錄。

---

### 需求 15：擴大遊戲畫布

**使用者故事：** 身為玩家，我希望遊戲畫面更大，以便獲得更好的視覺體驗與遊玩空間。

#### 驗收標準

1. THE 遊戲（Game）SHALL 使用比原始尺寸更大的畫布（Canvas）尺寸進行渲染。
2. THE 遊戲（Game）SHALL 調整所有遊戲元素的位置與比例，以適應新的畫布尺寸。
3. THE 遊戲（Game）SHALL 保持遊戲元素之間的相對比例與間距一致。
