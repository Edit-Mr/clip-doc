---
sidebar_position: 5
title: 新增積木
---
本篇將會演示如何新增積木。
## 示例
想要訪問擴充套件介面，我們要先引入 ``api``實例。
```javascript
const { api } = require('clipcc-extension');
```
### 新增積木
在新增積木之前，我們需要先新增一個分類。
```javascript
api.addCategory({
    categoryId: 'your.category.id',
    messageId: 'your.category.id',
    color: '#339900' // HEX color code
});
```
``categoryId`` 是該分類的唯一識別符號, ``messageId``是用於標識該分類的語言文字，將會作為該分類的顯示名稱（在沒有語言檔案的情況下）。
現在我們需要引入 ``type`` 實例來定義積木的型別。

```javascript
const { type } = require('clipcc-extension');
```
現在我們可以新增一個積木了。
```javascript
api.addBlock({
    opcode: 'your.block.id',
    type: type.BlockType.REPORTER,
    messageId: 'your.block.id',
    categoryId: 'your.category.id',
    param: {
        VALUE: {
            type: type.ParameterType.STRING,
            default: 'Hello World!'
        }
    },
    function: args => args.VALUE;
});
```
``opcode`` 是該積木的唯一識別符號, ``messageId``是用於標識該分類的語言文字，將會作為該分類的顯示名稱（在沒有語言檔案的情況下）。 ``categoryId``用於標明該積木來自於哪裡。 ``param``是該積木的參數, 函式則用於定義該函式的執行行為。

**我們推薦使用箭頭函式而不是在類中定義函式**, 因為擴充套件可能會在執行過程中丟失 ``this`` 指向。

### 移除積木
因為一些原因，你可能會想移除一個積木。
```javascript
api.removeBlock('your.block.id');
```
如果你想移除一個分類，你可以使用
```javascript
api.removeCategory('your.category.id');
```
它將會移除該分類以及其包含的所有積木。我們推薦你在解除安裝擴充套件時使用該方法。
## 原型
### 分類
```javascript
class CategoryPrototype {
    categoryId: string;
    messageId: string;
    color: string;
}

function addCategory(category: CategoryPrototype): void;
function removeCategory(categoryId: string): void;
```
### 積木
```javascript
interface BlockPrototype {
    opcode: string;
    type: BlockType;
    option?: BlockOption;
    messageId: string;
    categoryId: string;
    function: Function;
    param?: { [key: string]: ParameterPrototype };
}

enum BlockType {
    COMMAND, REPORTER, BOOLEAN, BRANCH, HAT
}

class ParameterPrototype {
    type: ParameterType;
    default: any;
    shadow: ShadowPrototype;
}

enum ParameterType {
    NUMBER, STRING, BOOLEAN, ANY
}

class ShadowPrototype {
    type: string;
    fieldName: string;
}

class BlockOption {
    terminal?: boolean;
    monitor?: boolean;
    filter?: FilterType;
}

enum FilterType {
    SPRITE, STAGE, ALL, HIDE
}

function addBlock(block: BlockPrototype): void;
function removeBlock(opcode: string): void;
```