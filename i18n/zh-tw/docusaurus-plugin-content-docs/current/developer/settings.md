---
sidebar_position: 10
title: 設定
---

設定項被定義在擴充套件檔案內部根目錄下的 `settings.json` 檔案中，用於新增設定項到編輯器。擴充套件的全部設定項在擴充套件被載入時就會新增到編輯器中，而不是啟用時。即使用者可以在擴充套件並沒有被啟用的時候修改該擴充套件的設定項。

## 數據格式
```json title="settings.json"
[
    {
        "id": "option1",
        "type": "boolean",
        "default": false
    },
    {
        "id": "option2",
        "type": "number",
        "default": 0
    }
]
```
所有的設定項均以 `object` 形式定義，其最終在設定中的順序與 `settings.json` 順序一致，基本鍵值說明如下：

**id**：設定項的 ID，注意在擴充套件載入後，實際的 ID 為你的擴充套件 ID 後加上設定項的 ID，如上述 `option1` 的實際 id 為 `your.extension.id.option1`，其中 `your.extension.id` 是你的擴充套件 ID，其被定義于 `info.json`。這確保了外掛間的設定不會出現衝突。

**type**：設定項型別，對應了在設定中的控制元件型別，具體取值見後。

**default**：預設值，如果對應的控制元件是可輸入的，那麼預設值同時會作為該控制元件的 placeholder 屬性。當設定被使用者恢復預設值時，該設定項將被設定為此值。

## 型別
### Boolean（布林值）
這個設定項是一個布林型別，其值只能是 `true` 或者 `false`，對應的控制元件為 `Switch`。
```json
{
    "id": "option",
    "type": "boolean",
    "default": false
}
```
### Number（數字）
這個設定項是一個數字型別，對應的控制元件為 `Input`。
```json
{
    "id": "option",
    "type": "number",
    "default": 0,
    "max": 20,
    "min": 1,
    "precision": 0
}
```

**max**：（可選）限定數字的最大值。

**min**：（可選）限定數字的最小值。

**precision**：（可選）限定數字的小數點后位數，`0` 表示限定為整數。
### Selector (選擇器)
這個設定項是一個字串型別，對應的控制元件為 `Selector`
```json
{
    "id": "option",
    "type": "selector",
    "default": "apple",
    "options": [{
        "id": "apple",
        "message": "message.apple"
    }, {
        "id": "boy",
        "message": "message.boy"
    }, {
        "id": "cat",
        "message": "message.cat"
    }, {
        "id": "dog",
        "message": "message.dog"
    }]
}
```
**items**：設定選擇器的全部選項，每個選項應當以一個對象的形式說明，這個對象的 `id` 表示該選項的值，`message` 表示對應的翻譯 ID。
## 翻譯
設定項應當有翻譯文字以及（可選的）描述文字。如果一個設定項沒有設定描述文字的翻譯，那麼不會顯示其描述文字。
```json
{
    "your.extension.id.settings.option1": "Option 1",
    "your.extension.id.settings.option1.description": "Help message for option 1"
}
```
如上述內容所示，設定項翻譯的鍵應當為 `your.extension.id.settings` 加上設定項 ID 的形式，對應的描述文字的鍵應在其後面新增 `.description`。
## 介面
```javascript
function getSettings (id: string): any;
```
效果: 獲取設定中某項的值