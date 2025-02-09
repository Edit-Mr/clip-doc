---
sidebar_position: 2
title: 自定義返回值
---

該功能可以讓你減少使用不必要的積木，同時提升作品積木邏輯清晰度以實現更多更復雜的功能。

![自定義返回值](/img/custom-reporter.png)

## 如何使用
1. 點選 分類欄中的「函式」。
2. 點選 「製作新的函式」 按鈕。
3. 自定義你的積木，完成後勾選「自定義返回值」。
4. 點選 「完成」。
5. 定義這個函式，別忘了再最後返回一個值。

## 注意
1. 目前 ClipCC 對於邏輯運算的短路運算和參數的計算順序是未定義的。之後該問題可能會被修復，但你仍應確保現在的專案不要出現依賴於這些特性的行為。
2. 爲了保證你的作品安全, **請儘量減少遞迴呼叫。**
3. 你不能在首次定義完成後編輯函式型別。
4. 爲了防止參數積木和自定義返回值積木混淆，我們調整了原有自定義積木的顏色。