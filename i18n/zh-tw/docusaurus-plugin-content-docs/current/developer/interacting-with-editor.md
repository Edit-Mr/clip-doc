---
sidebar_position: 8
title: 與編輯器互動
---

你可以使用 ``api.getXXXInstance()`` 訪問編輯器實例。

## 示例
```javascript
const { api } = require('clipcc-extension');
// ...
const GUI = api.getGuiInstance();
const VM = api.getVmInstance();
const Block = api.getBlockInstance();
```
現在你可以修改和訪問該實例了，以下是一個例子：
```javascript
// Get the frame rate.
const frameRate = VM.runtime.frameRate;
// Modify the frame rate.
VM.runtime.setFrameRate(30);
```
你也可以覆寫某個類別的函式。
```javascript
const originalFunc = vm.runtime.sequencer.stepThread.prototype;
vm.runtime.sequencer.stepThread.prototype = function(thread) {
    console.log('current thread:', thread);
    originalFunc.call(this, thread);
};
```
:::caution

擴充套件可以獲取編輯器的一些實例，通過直接修改或呼叫實例的方式完成更為複雜的功能，但對於實例的所有操作的結果完全取決於編輯器的實現，其穩定性和可行性不被保證，在版本間可能存在變動。

:::

:::caution

在社區版中，Block 實例是無法在播放器環境下訪問的。

:::

## 原型

```javascript
function getVmInstance(): Object;
function getGuiInstance(): Object;
function getStageCanvas(): HTMLCanvasElement;
function getBlockInstance(): Object;
```