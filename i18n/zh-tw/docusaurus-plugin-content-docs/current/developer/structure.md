---
sidebar_position: 3
title: 擴充套件結構
---
ClipCC 擴充套件必須以 zip 格式打包為 `*.ccx` 檔案。

在 `*.ccx` 檔案內部的根目錄下，必須包含 `info.json` 和 `main.js` 兩個檔案。

## 擴充套件資訊

在擴充套件檔案內部根目錄下的 `info.json` 為擴充套件的基本資訊，該檔案中各欄位必須按照下面規定的方式填寫。缺少必要欄位的，應當拒絕載入該擴充套件；有無用欄位的，不對無用欄位做任何響應。

```json
{
    "id": "extension.example",
    "author": "Clip Team",
    "version": "1.0.0",
    "icon": "assets/icon.jpg",
    "inset_icon": "assets/inset_icon.svg",
    "api": 1,
    "optional": false,
    "dependency": {
        "anothor.extension": "0.1.0"
    }
}
```

### 欄位說明
> 欄位說明（標註有可選的表示這一選項是可選的，否則必須填寫）：

**id**：外掛的 ID，必須是唯一的，推薦的寫法為 `作者ID.外掛名`，其中整個 ID 必須滿足 `[a-zA-Z0-9_-]+`。不推薦使用多個 `.` 分割 ID，如有必要，每個 `.` 之間必須至少有一個合法的字元。

**author**：作者名，可以是一個字串，或者一個列表。

**version**：版本。

**icon**：外掛頭圖。

**inset_icon**：外掛小圖示。

**api**：API 版本標識（標準版本），如果目前的編輯器不支援該 API，則應當拒絕載入該擴充套件。

**optional**：（可選）是否是可選的擴充套件，預設為 `false`。如果設為 `true` 則表示這個擴充套件是可選的，不載入並不會對作品檔案造成影響。如果為 `false` 表示這個擴充套件必須被載入才能保證作品檔案正常打開，例如這個擴充套件新增了新的模組。

**dependency**：(可選）依賴的擴充套件，鍵為擴充套件 ID，值為依賴的版本。版本號有如下格式：`1.2.0` 表示版本只能為 `1.2.0`；`1.2.*` 表示版本號可以為 `1.2.0`、`1.2.1` 等，在匹配中 `*` 可以出現多次，但必須表示版本號中完整的某一段，即 `1.2.3*` 是不合法的，並且 `*` 不能出現在包含 `^` 或 `~` 開頭的版本號匹配中；`^1.2.3` 表示版本號大於等於 `1.2.3` 但小於 `2.0.0`；`~1.2.3` 表示版本號大於等於 `1.2.3` 但小於 `1.3.0`。
## 入口檔案
在擴充套件檔案內部根目錄下的 `main.js` 為擴充套件的入口檔案，在擴充套件被載入到編輯器時，該檔案將被載入編輯器並執行。

入口檔案必須顯式導出一個類，該類應當實現 `onInit` 和 `onUninit` 方法（但不是強制的），以響應載入和解除安裝事件。

目前，擴充套件導出的類必須以 CommonJS 形式導出，即導出到 `module.exports`。

下面是一個基於 CommonJS 規範的擴充套件最小實現：

```javascript
class HelloExtension {
    onInit() { /* ... */}
    onUninit() { /* ... */ }
}

module.exports = HelloExtension;
```