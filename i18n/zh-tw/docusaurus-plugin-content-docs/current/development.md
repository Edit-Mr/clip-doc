---
sidebar_position: 3
title: 開發
---

## 安裝

:::tip

請確保在開始之前已經安裝了[Node.js](https:////www.nodejs.org)和[Git](https://git-scm.com/)，此外我們建議使用 Linux 而不是 Windows 進行配置

:::

### 在現有的 React 應用中使用
```bash
yarn install clipcc-gui@latest
```
或者
```bash
yarn install https://github.com/Clipteam/clipcc-gui.git
```
### 下載源碼並運行
```bash
git clone https://github.com/Clipteam/clipcc-gui.git
cd clipcc-gui
yarn install
yarn start
```
然後前往 [http://localhost:8601/](http://localhost:8601/)查看運行輸出

如果你想要編輯`clipcc-vm`或`clipcc-block`，請使用 ``yarn link``
```bash
# 請先在工作目錄中clone你需要的倉庫
cd clipcc-vm && yarn && yarn link
cd clipcc-block && yarn
# 編譯clipcc-block並link它，需要Python 3
yarn build
yarn link
# ...
cd clipcc-gui
yarn link clipcc-vm clipcc-block
yarn start
```
:::note 
只有 ``clipcc-vm`` 和 ``clipcc-gui`` 支持熱重載
:::

**如果你需要在其他應用中引用 ``clipcc-gui`` 的本地副本，你需要先執行 ``yarn build:dist``**

你也可以使用Yarn工作區, 詳情[點此](hhttps://classic.yarnpkg.com/blog/2017/08/02/introducing-workspaces/)

### 和原版 Scratch 的區別
1. clipcc-gui 使用 React 17.0.2 而不是 React 16.2
2. clipcc-block 重寫了編譯腳本以使用 Python3，並可以在 Windows 上運行
3. 切換包管理器為 Yarn
## 測試
### 文檔

在編寫測試前你也許會想閱讀[Jest](https://facebook.github.io/jest/docs/en/api.html)和[Enzyme](http://airbnb.io/enzyme/docs/api/)的文檔

到[jest cli docs](https://facebook.github.io/jest/docs/en/cli.html#content)來查看更多選項

### 執行測試

*筆記: 如果你是Windows使用者，請在Windows的`cmd.exe`  執行而不是Git Bash/MINGW64.*

在執行任何程式前，請先確保有先在這個資料夾(clipcc-gui)的最頂層執行`yarn install`。

#### 主要測試指令

一次執行Tlinter, unit tests, build，和integration tests
```bash
yarn test
```

#### 執行單位測試

單獨運行單元測試：
```bash
yarn run test:unit
```

在監視模式下運行單元測試（監視代碼更變並持續運行測試):
```bash
yarn run test:unit -- --watch
```

您可以運行單個集成測試文件（在此示例中執行`按鈕`測試）：

```bash
$(yarn bin)/jest --runInBand test/unit/components/button.test.jsx
```

#### 運行集成測試

集成測試使用無頭瀏覽器來操作repo中實際的HTML和javascript。

您將看不到此動作（儘管在播放聲音時您可以聽到它！）。

請注意，集成測試要求您首先創建一個可以在瀏覽器中加載的構建：

```bash
yarn run build
```

Then, you can run all integration tests:

```bash
yarn run test:integration
```

Or, you can run a single file of integration tests (in this example, the `backpack` tests):

```bash
$(yarn bin)/jest --runInBand test/integration/backpack.test.js
```

If you want to watch the browser as it runs the test, rather than running headless, use:

```bash
USE_HEADLESS=no $(yarn bin)/jest --runInBand test/integration/backpack.test.js
```

## Troubleshooting

### Ignoring optional dependencies

When running `yarn install`, you can get warnings about optionsl dependencies:

```
npm WARN optional Skipping failed optional dependency /chokidar/fsevents:
npm WARN notsup Not compatible with your operating system or architecture: fsevents@1.2.7
```

You can suppress them by adding the `no-optional` switch:

```
yarn install --no-optional
```

Further reading: [Stack Overflow](https://stackoverflow.com/questions/36725181/not-compatible-with-your-operating-system-or-architecture-fsevents1-0-11)

### Resolving dependencies

When installing for the first time, you can get warnings that need to be resolved:

```
npm WARN eslint-config-scratch@5.0.0 requires a peer of babel-eslint@^8.0.1 but none was installed.
npm WARN eslint-config-scratch@5.0.0 requires a peer of eslint@^4.0 but none was installed.
npm WARN scratch-paint@0.2.0-prerelease.20190318170811 requires a peer of react-intl-redux@^0.7 but none was installed.
npm WARN scratch-paint@0.2.0-prerelease.20190318170811 requires a peer of react-responsive@^4 but none was installed.
```

You can check which versions are available:

```
yarn view react-intl-redux@0.* version
```

You will need to install the required version:

```
yarn install  --no-optional --save-dev react-intl-redux@^0.7
```

The dependency itself might have more missing dependencies, which will show up like this:

```
user@machine:~/sources/scratch/clipcc-gui (491-translatable-library-objects)$ yarn install  --no-optional --save-dev react-intl-redux@^0.7
clipcc-gui@0.1.0 /media/cuideigin/Linux/sources/scratch/clipcc-gui
├── react-intl-redux@0.7.0
└── UNMET PEER DEPENDENCY react-responsive@5.0.0
```

You will need to install those as well:

```
yarn install  --no-optional --save-dev react-responsive@^5.0.0
```

Further reading: [Stack Overflow](https://stackoverflow.com/questions/46602286/npm-requires-a-peer-of-but-all-peers-are-in-package-json-and-node-modules)
### Transitions

These are names for the action which causes a state change. Some examples are:

* `START_FETCHIN