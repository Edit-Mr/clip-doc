---
sidebar_position: 9
title: 與擴充套件互動
---

擴充套件間互動通過暴露一個函式實現，在擴充套件中，使用 `registerGlobalFunction` 函式將擴充套件原型中的某一個成員函式暴露，允許其他擴充套件通過 `callGlobalFunction` 函式呼叫。注意，在其他擴充套件中直接獲取某一個擴充套件的實例同樣也是不被禁止的，但其結果是未定義的行為。

## 範例
```javascript title="src/index.js"
class HelloExtension {
    onInit() {
        api.registerGlobalFunction('helloWorld', this.helloWorld);
    }

    helloWorld() {
        console.log('Hello, world!');
    }
}
```

**registerGlobalFunction(name, func)**：接受兩個參數，分別表示全域性函式名和函式對象，函式名推薦使用 `擴充名.函式名` 的形式，以防止與其他擴充套件的函式衝突。如果某個名稱已經被佔用了，那麼后註冊的函式不應當被載入，並拋出一個錯誤。

**unregisterGlobalFunction(name)**：刪除某個已經註冊的全域性函式，注意，你應當只刪除自己註冊的全域性函式，否則行為未定義。在擴充套件被解除安裝后，該擴充套件註冊的全域性函式應當被刪除。

**callGlobalFunction(name, ...arg)**：接受一個函式名，後面跟隨著參數列表，用來呼叫對應的函式。如果對應的函式不存在，則應當拋出一個錯誤。

## 原型

```javascript
function registerGlobalFunction(name, func): void;
function unregisterGlobalFunction(name): void;
function callGlobalFunction(name, ...arg): any;
```