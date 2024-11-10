# Git Commit Message 規範

此規範用於撰寫清晰一致的 Git commit 訊息，讓團隊能迅速理解代碼更改的內容及目的，提升 Code Review 的效率。

## Commit Message 組成
![image](https://github.com/user-attachments/assets/baebc4db-c29e-4fa7-ad68-49ba9d035ae4)

### Header: `<type>(<scope>): <subject>`

- **type**：commit 類別，描述本次更改的性質，為必要欄位。常用類別包括：
  - **feat**：新增或修改功能
  - **fix**：修復 Bug
  - **docs**：文件變更
  - **style**：代碼格式變更，不影響程式碼運行（如空白、縮排）
  - **refactor**：重構代碼，不涉及功能更改或 Bug 修復
  - **perf**：提升性能
  - **test**：新增或更新測試代碼
  - **chore**：其他雜項維護
  - **revert**：回滾到前一個 commit

- **scope**：commit 影響的範圍（如資料庫、控制層），為可選欄位。

- **subject**：簡要描述此次變更（50 字以內，無句號結尾），為必要欄位。

### Body
- 描述此次變更的詳細內容，每行不超過 72 個字元。
- 說明更改的原因、變更的內容及其影響，對比與先前行為的差異。

### Footer
- 填寫關聯的任務編號或 Issue（如有）。
- **BREAKING CHANGE**（可忽略）：記錄不兼容的變更，格式為 `BREAKING CHANGE:`，附上變更描述、原因和遷移指導。

---

## Type 指引

Type 的選擇影響 Code Review 的角度：
- 若為 **fix**，應聚焦於問題的解決方式。
- 若為 **refactor**，可輕鬆閱讀重構過程，無需關注行為變更。
- 統一使用 Type 可使 Code Review 方向明確，提升效率。

---

## Commit 訊息範例

### 範例: fix

#### **fix: 自訂表單新增/編輯頁面，修正離開頁面提醒邏輯**

問題：
1. 進入新增頁面後，未執行動作時離開仍會跳出提醒。
2. 回到表單列表頁面後，仍跳出離開提醒。

原因：
1. 新增頁面自動建立空白題組會調用 `sort_item`，初始化 `unload` 事件處理器。
2. 回到上一頁後應取消 `unload` 事件監聽。

調整項目：
1. 初始化 `unload` 事件處理器時排除新增表單時的情境。
2. 回到上一頁後清除 `unload` 事件處理器。

issue #1335

---

### 範例: feat

#### **feat: 表單統計，多顯示計畫名稱欄位**

需求：
- 列表資訊增加「計畫名稱」欄位，以便後續匯出資料。

調整項目：
1. `Assessment_form.php`：匯出表單統計時新增「訓練計畫名」欄位。
2. `customize.php`：表單統計查詢時顯示訓練計畫名欄位。

issue #1200

---

### 範例: chore

#### **chore: 更新 testing 環境**

更新 `ci-phpunit-test` 套件版本 0.16 => 0.17

調整項目：
1. 移除 `MX/Modules` 中客製化的 Testing 邏輯，解決測試環境檔案存取問題。
2. 新增 `tests/unit` 和 `tests/integration` 目錄，移動測試檔案至適當位置。
3. 調整 `AdminTestCase.php`，繼承 `TestCase`，供測試案例使用。

issue #709

---

### 範例: style

#### **style: message 頁面，對 Component 做 Beautifier**

經 IE 瀏覽器測試後發現 Component 夾帶 ES6 語法。為方便日後維護，先對所有壓縮的程式碼進行 Beautifier。

調整項目：
1. Beautifier 處理壓縮程式碼。
2. 移除過時的註解代碼。

issue #1219, #1028

---

### 範例: refactor

#### **refactor: 重構取得「簽核流程種類名稱」邏輯**

原邏輯分散於多個檔案，重構後統一透過 `Process::get_type_name($process_type)` 取得流程種類名稱。

調整項目：
1. `Process.php`：新增 `get_type_name()` 方法。
2. 刪除不必要的檔案與方法，改用 `Process::get_type_name()`。

issue #1253

---

### 範例: perf

#### **perf: 評核表單列表，優化取得受評者速度**

調整前頁面載入需 52 秒，調整後 5 秒。

調整方式：
- 改成一次性 DB 查詢所有受評者資料，再於 PHP 端分配。

issue #1272

---

### 範例: docs

#### **docs: 新增註解**
#### **docs: 修正型別註解**

更新註解以提升 IDE 的類別讀取正確性，並移除過期的註解。

issue #1229
