# 實作計畫：Flappy Kiro

## 概覽

以單一 HTML 檔案實作 Flappy Kiro 瀏覽器遊戲，使用 HTML Canvas 2D API 與原生 JavaScript（ES6+），不依賴任何外部框架。實作順序依照遊戲核心模組逐步建構，最終整合為完整可運行的遊戲。

## 任務

- [x] 1. 建立 HTML 骨架與畫布設定
  - 建立 `index.html`，包含 `<canvas id="gameCanvas">` 元素
  - 設定畫布尺寸為 480×640（含底部分數欄 40px）
  - 在 `<script>` 中定義 `CONFIG` 常數物件，涵蓋所有遊戲參數（重力、跳躍速度、管道速度、間隙大小、顏色、音效路徑、圖片路徑等）
  - 取得 Canvas 2D context，準備渲染環境
  - _需求：1.1, 1.2_

- [x] 2. 實作資源載入器（Asset Loader）
  - [x] 2.1 實作 `assetLoader` 物件
    - 實作 `loadAll(onComplete)` 方法，預載 `ghosty.png`、`jump.wav`、`game_over.wav`
    - 實作 `getImage(key)` 與 `getAudio(key)` 方法
    - 所有資源載入完成後才呼叫 `onComplete` 回呼
    - _需求：2.2, 2.4, 4.3_

- [x] 3. 實作遊戲狀態機（Game State）
  - [x] 3.1 實作 `gameState` 物件
    - 定義三種狀態：`WAITING`、`PLAYING`、`GAME_OVER`
    - 實作 `reset()`（保留 `highScore`）、`setState(s)`、`getState()` 方法
    - 初始狀態設為 `WAITING`
    - _需求：1.7, 4.4, 6.5_

- [x] 4. 實作幽靈控制器（Ghost Controller）
  - [x] 4.1 實作 `ghost` 物件的物理邏輯
    - 實作 `reset()` 將幽靈重置至初始位置（水平固定於 `CONFIG.GHOST_X`）
    - 實作 `jump()` 施加 `CONFIG.JUMP_VELOCITY` 向上速度
    - 實作 `update()` 每幀套用重力加速度並更新垂直位置
    - 實作 `getBounds()` 回傳 `Rect` 邊界框
    - _需求：2.1, 2.3, 2.5, 2.6_
  - [x] 4.2 實作 `ghost.render(ctx)` 方法
    - 使用 `assetLoader.getImage('ghost')` 繪製幽靈圖片
    - _需求：2.2_

- [x] 5. 實作管道管理器（Pipe Manager）
  - [x] 5.1 實作 `pipeManager` 物件
    - 實作 `reset()` 清除所有管道並重置計時器
    - 實作 `spawnPipe()` 隨機決定垂直位置，生成一組上下管道
    - 實作 `update(dt)` 移動管道、依時間間隔生成新管道、回收離屏管道
    - 實作 `getPipes()` 回傳當前管道陣列
    - _需求：3.1, 3.3, 3.4, 3.5, 3.6_
  - [x] 5.2 實作 `pipeManager.render(ctx)` 方法
    - 以 `CONFIG.PIPE_COLOR` 繪製所有管道（上管道從頂部向下、下管道從底部向上）
    - _需求：3.2_

- [x] 6. 實作碰撞偵測器（Collision Detector）
  - [x] 6.1 實作 `rectOverlap(a, b)` 純函式
    - 判斷兩個 `Rect` 是否重疊，回傳布林值
    - _需求：4.1_
  - [x] 6.2 實作 `checkCollision(ghost, pipes, canvasHeight)` 函式
    - 檢查幽靈與所有管道的碰撞
    - 檢查幽靈是否超出畫布上下邊界
    - _需求：4.1, 4.2_

- [x] 7. 實作計分系統（Score System）
  - [x] 7.1 實作 `scoreSystem` 物件
    - 實作 `reset()` 重置當前分數（保留最高分）
    - 實作 `update(ghost, pipes)` 判斷幽靈是否通過管道並加分（使用 `pipe.scored` 防止重複計分）
    - 實作 `getScore()` 與 `getHighScore()` 方法
    - 當分數超過最高分時自動更新最高分
    - _需求：5.1, 5.3, 5.4, 5.5_
  - [x] 7.2 實作 `scoreSystem.render(ctx)` 方法
    - 在底部深色橫條中以「Score: X | High: X」格式渲染分數
    - _需求：5.2_

- [x] 8. 實作音效系統（Audio System）
  - [x] 8.1 實作 `audioSystem` 物件
    - 實作 `play(key)` 方法，使用 `assetLoader.getAudio(key)` 播放音效
    - 以 `try/catch` 或 `.catch()` 靜默忽略播放失敗
    - _需求：7.1, 7.2, 7.3_

- [x] 9. 實作渲染器（Renderer）
  - [x] 9.1 實作背景與裝飾渲染
    - 實作 `renderer.drawBackground(ctx)` 繪製淡藍色背景與手繪素描紋理效果
    - 實作 `renderer.drawClouds(ctx)` 繪製白色圓角矩形雲朵
    - 實作 `renderer.drawScoreBar(ctx)` 繪製底部深色橫條
    - _需求：1.3, 1.4, 1.5, 1.6_
  - [x] 9.2 實作狀態畫面渲染
    - 實作 `renderer.drawWaitingScreen(ctx)` 顯示開始提示
    - 實作 `renderer.drawGameOverScreen(ctx, score, highScore)` 顯示結束畫面（含最終分數與重新開始提示）
    - _需求：1.7, 4.5_

- [x] 10. 實作輸入處理與遊戲迴圈
  - [x] 10.1 實作輸入事件監聽
    - 監聽 `keydown`（空白鍵）與 `click`（畫布點擊）事件
    - `WAITING` 狀態下：切換至 `PLAYING` 並觸發第一次跳躍
    - `PLAYING` 狀態下：呼叫 `ghost.jump()` 並播放跳躍音效
    - `GAME_OVER` 狀態下：重置所有模組並切換至 `PLAYING`
    - _需求：2.3, 2.4, 6.1, 6.2, 6.3, 6.4, 6.5_
  - [x] 10.2 實作主遊戲迴圈
    - 使用 `requestAnimationFrame` 驅動迴圈
    - 計算 `deltaTime`
    - `PLAYING` 狀態下依序執行：更新幽靈 → 更新管道 → 碰撞偵測 → 更新分數
    - 碰撞時切換至 `GAME_OVER` 並播放結束音效
    - 每幀依序渲染：背景 → 雲朵 → 管道 → 幽靈 → 分數欄 → 狀態畫面（若需要）
    - _需求：2.1, 2.5, 3.4, 4.1, 4.2, 4.3, 4.4_

- [x] 11. 整合所有模組並完成最終接線
  - [x] 11.1 在 `assetLoader.loadAll` 的回呼中初始化所有模組並啟動遊戲迴圈
    - 確保資源載入完成後才啟動迴圈
    - 確認 `WAITING` 畫面正確顯示
    - _需求：1.7_
  - [x] 11.2 撰寫整合測試（手動驗證清單）
    - 驗證：開啟 HTML 檔案後顯示等待畫面
    - 驗證：按空白鍵或點擊後遊戲開始，幽靈跳躍並播放音效
    - 驗證：管道持續生成並向左移動
    - 驗證：碰撞後顯示結束畫面並播放音效
    - 驗證：重新開始後分數歸零但最高分保留
    - _需求：1.7, 2.3, 3.1, 4.5, 5.4, 6.1_

- [x] 12. 最終檢查點
  - 確認遊戲可直接以瀏覽器開啟 `index.html` 運行，無需任何伺服器
  - 確認所有音效、圖片資源路徑正確
  - 確認最高分在同一會話中跨多局保留
  - 如有問題請向使用者提問

---

## 噴射背包系統與視覺增強任務

- [x] 13. 調整畫布尺寸與縮放
  - [x] 13.1 更新 CONFIG 常數中的畫布尺寸
    - 將 `CANVAS_WIDTH` 從 480 更新為 800
    - 將 `CANVAS_HEIGHT` 從 640 更新為 600
    - 調整 `GHOST_X` 位置以適應新畫布寬度
    - _需求：15.1, 15.2_
  - [x] 13.2 驗證所有遊戲元素比例正確
    - 測試管道、幽靈、分數欄在新尺寸下的顯示
    - 確認間隙大小與遊戲難度保持一致
    - _需求：15.2, 15.3_

- [x] 14. 擴展音效系統支援循環播放
  - [x] 14.1 實作 `audioSystem.playLoop(key)` 方法
    - 設定音效的 `loop` 屬性為 `true`
    - 使用 `try/catch` 靜默處理播放失敗
    - _需求：11.2_
  - [x] 14.2 實作 `audioSystem.stop(key)` 與 `audioSystem.stopAll()` 方法
    - 實作停止指定音效的方法
    - 實作停止所有音效的方法（用於遊戲結束）
    - _需求：11.3_
  - [x] 14.3 更新資源載入器以載入新音效
    - 在 `assetLoader` 中添加 `jetpack_collect.wav`（可選）
    - 在 `assetLoader` 中添加 `acceleration_music.wav`（可選）
    - 確保音效載入失敗時靜默忽略
    - _需求：11.1, 11.4_

- [x] 15. 實作遊戲狀態加速管理
  - [x] 15.1 擴展 `gameState` 物件以支援加速狀態
    - 添加 `isAccelerating` 布林值屬性
    - 添加 `accelerationTimeLeft` 計時器（毫秒）
    - 添加 `jetpacksCollected`、`currentStreak`、`longestStreak`、`totalJetpacks` 統計屬性
    - _需求：9.1, 14.1, 14.2_
  - [x] 15.2 實作 `gameState.startAcceleration()` 方法
    - 設定 `isAccelerating = true`
    - 初始化 `accelerationTimeLeft = CONFIG.ACCELERATION_DURATION`
    - 增加 `jetpacksCollected` 與 `currentStreak` 計數
    - 更新 `longestStreak` 與 `totalJetpacks` 統計
    - _需求：9.1, 9.6, 14.2_
  - [x] 15.3 實作 `gameState.updateAcceleration(dt)` 方法
    - 每幀減少 `accelerationTimeLeft`
    - 當時間歸零時自動結束加速狀態並重置 `currentStreak`
    - _需求：9.4, 9.5_
  - [x] 15.4 實作 `gameState.getScoreMultiplier()` 方法
    - 加速狀態時回傳 `CONFIG.SCORE_MULTIPLIER`（2 倍）
    - 正常狀態時回傳 1
    - _需求：12.1, 12.2, 12.3_
  - [x] 15.5 實作 `gameState.getJetpackStats()` 方法
    - 回傳包含 `collected`、`longestStreak`、`totalJetpacks` 的統計物件
    - _需求：14.1, 14.2, 14.3_

- [x] 16. 實作噴射背包管理器（Jetpack Manager）
  - [x] 16.1 實作 `jetpackManager` 物件核心功能
    - 實作 `reset()` 清除所有噴射背包並重置計數器
    - 實作 `spawnJetpack()` 在隨機高度生成噴射背包（避免過於靠近邊界）
    - 實作 `update(dt, speedMultiplier, pipesPassedCount)` 移動噴射背包、判斷生成時機、回收離屏噴射背包
    - 實作 `getJetpacks()` 回傳當前噴射背包陣列
    - 實作 `removeJetpack(jetpack)` 移除指定噴射背包
    - _需求：8.1, 8.3, 8.4, 8.5, 8.6_
  - [x] 16.2 實作噴射背包生成邏輯
    - 根據 `pipesPassedCount` 判斷生成時機（每 3-5 組管道）
    - 使用 `CONFIG.JETPACK_SPAWN_INTERVAL` 與 `CONFIG.JETPACK_SPAWN_VARIANCE` 控制隨機性
    - _需求：8.1_
  - [x] 16.3 實作 `jetpackManager.render(ctx, time)` 方法
    - 使用 `assetLoader.getImage('jetpack')` 繪製噴射背包圖片
    - 實作閃爍效果（使用 `Math.sin` 與 `glowPhase` 控制透明度）
    - _需求：8.2, 8.7_
  - [x] 16.4 更新資源載入器以載入噴射背包圖片
    - 在 `assetLoader` 中添加 `jetpack.png` 圖片載入
    - _需求：8.2_

- [ ] 17. 實作粒子系統（Particle System）
  - [~] 17.1 實作火焰粒子系統
    - 實作 `particleSystem.emitFlame(x, y)` 在指定位置發射火焰粒子
    - 每幀生成 `CONFIG.FLAME_PARTICLE_COUNT` 個粒子
    - 實作粒子物理：向後移動（負 X 方向）+ 隨機垂直擾動
    - 實作粒子生命週期管理（`CONFIG.FLAME_PARTICLE_LIFETIME`）
    - 實作 `particleSystem.renderFlames(ctx)` 繪製火焰粒子（橙色/黃色漸變，透明度衰減）
    - _需求：10.1, 10.2_
  - [~] 17.2 實作彩虹軌跡系統
    - 實作 `particleSystem.addTrailPoint(x, y)` 添加軌跡點
    - 維護最多 `CONFIG.RAINBOW_TRAIL_LENGTH` 個軌跡點（FIFO 佇列）
    - 實作 `particleSystem.renderTrail(ctx)` 繪製彩虹軌跡（HSL 色彩空間，透明度衰減）
    - _需求：10.4, 10.5_
  - [~] 17.3 實作速度線系統
    - 實作 `particleSystem.renderSpeedLines(ctx, ghostY)` 繪製速度線
    - 生成 `CONFIG.SPEED_LINE_COUNT` 條水平線，均勻分佈於畫布高度
    - 使用半透明白色渲染，長度為 `CONFIG.SPEED_LINE_LENGTH`
    - _需求：10.3_
  - [~] 17.4 實作 `particleSystem.reset()` 與 `particleSystem.update(dt)` 方法
    - 實作清除所有粒子的方法
    - 實作更新所有粒子狀態的方法（移動、衰減、回收）
    - _需求：10.1, 10.2, 10.4_

- [ ] 18. 實作 UI 系統（UI System）
  - [~] 18.1 實作倒數計時器渲染
    - 實作 `uiSystem.renderTimer(ctx, timeLeft)` 方法
    - 在畫布右上角顯示格式為「⏱ X.Xs」的計時器
    - 使用 `CONFIG.TIMER_COLOR` 與 `CONFIG.TIMER_FONT` 樣式
    - _需求：13.1, 13.2, 13.3, 13.4_
  - [~] 18.2 實作分數倍增器指示渲染
    - 實作 `uiSystem.renderMultiplier(ctx, multiplier)` 方法
    - 在分數旁邊顯示「×2」指示（僅加速時）
    - 使用金色（`#ffd700`）渲染
    - _需求：12.1, 12.2_
  - [~] 18.3 實作噴射背包統計渲染
    - 實作 `uiSystem.renderStats(ctx, stats)` 方法
    - 在遊戲結束畫面顯示收集總數、最長連擊數、會話總數
    - _需求：14.3, 14.4_

- [x] 19. 更新碰撞偵測支援無敵狀態
  - [x] 19.1 更新 `checkCollision` 函式簽名
    - 添加 `isInvincible` 參數
    - 邊界檢查始終啟用（上下邊界）
    - 管道碰撞檢查在 `isInvincible = true` 時跳過
    - _需求：9.3, 4.1, 4.2_
  - [x] 19.2 實作 `checkJetpackCollection` 函式
    - 實作 `checkJetpackCollection(ghost, jetpacks)` 純函式
    - 使用 `rectOverlap` 判斷幽靈與噴射背包的碰撞
    - 回傳被收集的噴射背包物件或 `null`
    - _需求：8.6_

- [x] 21. 更新管道管理器支援速度倍增
  - [x] 21.1 更新 `pipeManager.update` 方法
    - 添加 `speedMultiplier` 參數
    - 管道移動速度為 `CONFIG.PIPE_SPEED * speedMultiplier`
    - _需求：9.2_
  - [x] 21.2 實作 `pipeManager.getPipesPassedCount()` 方法
    - 追蹤已通過的管道總數（用於噴射背包生成判斷）
    - _需求：8.1_

- [ ] 22. 更新渲染器支援噴射背包統計
  - [~] 22.1 更新 `renderer.drawGameOverScreen` 方法
    - 添加 `jetpackStats` 參數
    - 呼叫 `uiSystem.renderStats(ctx, jetpackStats)` 顯示統計資訊
    - _需求：14.3_

- [x] 23. 整合噴射背包系統至遊戲迴圈
  - [x] 23.1 更新遊戲迴圈以支援加速狀態
    - 在 `PLAYING` 狀態下每幀呼叫 `gameState.updateAcceleration(dt)`
    - 取得當前速度倍增器 `gameState.isAccelerating() ? CONFIG.ACCELERATION_SPEED_MULTIPLIER : 1`
    - 取得當前分數倍增器 `gameState.getScoreMultiplier()`
    - _需求：9.1, 9.4, 9.5_
  - [x] 23.2 整合噴射背包更新與渲染
    - 在管道更新後呼叫 `jetpackManager.update(dt, speedMultiplier, pipeManager.getPipesPassedCount())`
    - 在幽靈更新後呼叫 `checkJetpackCollection(ghost, jetpackManager.getJetpacks())`
    - 若收集到噴射背包，呼叫 `gameState.startAcceleration()` 並播放收集音效
    - 移除被收集的噴射背包 `jetpackManager.removeJetpack(collectedJetpack)`
    - _需求：8.6, 9.1, 11.1_
  - [x] 23.3 整合粒子系統更新與渲染
    - 在加速狀態時每幀呼叫 `particleSystem.emitFlame(ghost.x, ghost.y)` 與 `particleSystem.addTrailPoint(ghost.x, ghost.y)`
    - 每幀呼叫 `particleSystem.update(dt)` 更新粒子狀態
    - 在渲染階段依序渲染：彩虹軌跡 → 幽靈 → 火焰粒子 → 速度線
    - _需求：10.1, 10.2, 10.3, 10.4_
  - [x] 23.4 整合 UI 系統渲染
    - 在加速狀態時呼叫 `uiSystem.renderTimer(ctx, gameState.getAccelerationTimeLeft())`
    - 在加速狀態時呼叫 `uiSystem.renderMultiplier(ctx, gameState.getScoreMultiplier())`
    - _需求：13.1, 13.2, 13.3_
  - [x] 23.5 更新碰撞偵測呼叫
    - 傳遞 `gameState.isAccelerating()` 作為 `isInvincible` 參數
    - _需求：9.3_
  - [x] 23.6 更新計分系統呼叫
    - 傳遞 `gameState.getScoreMultiplier()` 作為 `multiplier` 參數
    - _需求：12.1, 12.2_
  - [x] 23.7 整合加速音效
    - 在 `startAcceleration()` 時呼叫 `audioSystem.playLoop('acceleration_music')`（可選）
    - 在加速結束時呼叫 `audioSystem.stop('acceleration_music')`
    - _需求：11.2, 11.3_

- [x] 24. 更新所有模組的 reset 方法
  - [x] 24.1 確保 `gameState.reset()` 重置加速狀態
    - 重置 `isAccelerating`、`accelerationTimeLeft`、`jetpacksCollected`、`currentStreak`
    - 保留 `longestStreak` 與 `totalJetpacks` 會話統計
    - _需求：14.4_
  - [x] 24.2 確保遊戲重新開始時呼叫所有 reset 方法
    - 呼叫 `jetpackManager.reset()`
    - 呼叫 `particleSystem.reset()`
    - 呼叫 `audioSystem.stopAll()`
    - _需求：9.5, 11.3_

- [x] 25. 測試與驗證噴射背包系統
  - [x] 25.1 撰寫手動測試清單
    - 驗證：噴射背包在通過約 3-5 組管道後生成
    - 驗證：收集噴射背包後立即進入加速狀態
    - 驗證：加速期間可穿越管道而不觸發碰撞
    - 驗證：加速期間通過管道分數為 2 倍
    - 驗證：倒數計時器正確顯示並倒數至 0
    - 驗證：火焰粒子從幽靈位置向後發射
    - 驗證：彩虹軌跡在幽靈後方顯示並淡出
    - 驗證：速度線在加速時顯示
    - 驗證：加速結束後恢復正常狀態（速度、碰撞、分數）
    - 驗證：遊戲結束畫面顯示噴射背包統計（收集數、連擊數）
    - 驗證：畫布尺寸為 800×600，所有元素比例正確
    - 驗證：連續收集多個噴射背包時連擊數正確累加
    - 驗證：加速音效正確播放與停止（如有音效檔案）
    - _需求：8.1, 8.6, 9.1, 9.2, 9.3, 9.4, 9.5, 9.6, 10.1, 10.2, 10.3, 10.4, 10.5, 11.1, 11.2, 11.3, 12.1, 12.2, 12.3, 13.1, 13.2, 13.3, 13.4, 14.1, 14.2, 14.3, 14.4, 15.1, 15.2, 15.3_

- [ ] 26. 最終檢查點 - 噴射背包系統
  - 確認所有新功能正常運作
  - 確認畫布尺寸調整後遊戲平衡性合理
  - 確認所有視覺效果流暢且無效能問題
  - 確認音效系統正確處理循環播放與停止
  - 確認噴射背包統計在會話期間正確保留
  - 如有問題請向使用者提問

## 備註

- 標有 `*` 的子任務為選填項目，可跳過以加速 MVP 開發
- 每個任務均對應具體需求編號以確保可追溯性
- 所有程式碼集中於單一 `index.html` 檔案的 `<script>` 標籤內
- 音效播放失敗應靜默忽略，不影響遊戲流程
- 噴射背包系統任務（13-26）建立在基礎遊戲任務（1-12）之上
- 粒子系統應使用物件池模式以避免效能問題
- 所有新模組應遵循 IIFE 模式並提供 `reset()` 方法
