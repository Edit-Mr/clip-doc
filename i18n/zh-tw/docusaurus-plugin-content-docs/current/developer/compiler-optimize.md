---
sidebar_position: 11
title: 編譯器優化
---
:::caution
以下所涉及的擴充套件API能處於草案階段，所涉及的內容可能在未來被修改。
:::

ClipCC 提供對擴充套件積木的編譯器介面支援。如果你想為你的積木使用編譯器 , 你需要在你的積木原型內新增一個``compile``函式。

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
    function: args => args.VALUE,
    compile: (args, isWrap, variablePool, thread) => {
        return "..."; // The snippet of JavaScript code
    }
});
```
如果你不指定``compile``函式, 編譯器將會回退到相容層來執行你的積木。

對於一個返回 Promise 的積木, 你需要響應 ``isWrap`` 來確認是否需要重新整理舞臺。你可以使用 ``yield;`` 來暫停執行緒並等待舞臺重新整理。對於一個"BRANCH"積木, 你必須指定該函式，否則你的積木將無法在編譯器模式下執行。

目前返回的程式碼片段僅可訪問 ``BlockUtility(util)`` 對像和自定義函式的參數。