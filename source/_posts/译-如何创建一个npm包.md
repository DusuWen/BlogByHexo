---
title: 译-如何创建一个npm包
date: 2024-09-04 14:05:49
tags:
  - 译
---

在本指南中，我们将介绍将包发不到npm所需的每一步骤。

这不是一个最简指南，我们将从一个空文件夹建立一个完全可用于生产环境的包，包括
* Git 用于版本控制
* TypeScript 用于编写代码并保持类型安全 
* Prettier 用于格式化我们的代码
* @arethetypeswrong/cli 用于检查我们的导出
* tsup 用于把我们的 TypeScript 代码编译为 CJS 和 ESM
* Vitest 用于运行测试
* GitHub Actions 用于运行 CI 集成
* Changesets 用于记录版本和发布包
<!-- more -->

## 1: Git
在本节中，我们将创建一个新的 Git 仓库，设置一个 .gitignore 文件，进行初次提交，然后在 GitHub 上创建一个新仓库，并将我们的代码推送到 GitHub。

### 1.1 初始化工程
执行下面命令初始化一个新的 git 工程
```bash
git init
```

### 1.2 设置.gitignore
在你的项目根目录创建一个.gitignore 文件并且添加以下内容
```
node_modules
```

### 1.3 初次提交
运行以下命令来进行初次提交：
```bash
git add .
git commit -m "Initial commit"
```

### 1.4 在 GitHub 创建新仓库
使用 GitHub CLI，运行以下命令来创建一个新仓库。在这个示例中，我选择了 `tt-package-demo` 作为仓库名称：
```bash
gh repo create tt-package-demo --source=. --public
```

### 1.5 推送到 GitHub
运行以下命令将代码推送到 GitHub：
```bash
git push --set-upstream origin main
```
## 2: `package.json`  
在本节中，我们将创建一个 `package.json` 文件，添加一个许可证字段，创建一个 `LICENSE` 文件，并添加一个 `README.md` 文件。

### 2.1: 创建 `package.json` 文件
创建一个 `package.json` 文件，并填入以下内容：

```json
{
  "name": "tt-package-demo",
  "version": "1.0.0",
  "description": "A demo package for Total TypeScript",
  "keywords": ["demo", "typescript"],
  "homepage": "https://github.com/mattpocock/tt-package-demo",
  "bugs": {
    "url": "https://github.com/mattpocock/tt-package-demo/issues"
  },
  "author": "Matt Pocock <team@totaltypescript.com> (https://totaltypescript.com)",
  "repository": {
    "type": "git",
    "url": "git+https://github.com/mattpocock/tt-package-demo.git"
  },
  "files": ["dist"],
  "type": "module"
}
```

- `name` 是人们将用来安装你包的名称。它必须在 npm 上唯一。你可以创建组织作用域（例如 `@total-typescript/demo`），这些作用域是免费的，可以帮助使其唯一。
- `version` 是你包的版本。它应该遵循语义版本控制：`0.0.1` 格式。每次发布新版本时，你应该增加此数字。
- `description` 和 `keywords` 是你包的简短描述。它们会在 npm 注册表中的搜索结果中列出。
- `homepage` 是你包主页的 URL。GitHub 仓库是一个不错的默认选择，或者如果你有文档网站，也可以使用。
- `bugs` 是用户报告包问题的 URL。
- `author` 是你！你可以选择添加你的电子邮件和网站。如果你有多个贡献者，你可以按照相同的格式将他们指定为一个贡献者数组。
- `repository` 是你包的仓库 URL。这将在 npm 注册表中创建一个指向你 GitHub 仓库的链接。
- `files` 是一个文件数组，指明人们安装你的包时应包含的文件。在本例中，我们包含了 `dist` 文件夹。`README.md`、`package.json` 和 `LICENSE` 默认包含在内。
- `type` 设置为 `module`，以表明你的包使用 ECMAScript 模块，而不是 CommonJS 模块。

### 2.2: 添加许可证字段
向你的 `package.json` 文件中添加一个许可证字段。选择一个许可证类型。这里选择了 MIT 许可证。

```json
{
  "license": "MIT"
}
```

### 2.3: 添加 `LICENSE` 文件
创建一个名为 `LICENSE`（无扩展名）的文件，包含你的许可证文本。对于 MIT 许可证，内容如下：

```
MIT License

Copyright (c) [year] [fullname]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

将 `[year]` 和 `[fullname]` 占位符替换为当前年份和你的名字。

### 2.4: 添加 `README.md` 文件
创建一个 `README.md` 文件，描述你的包。以下是一个示例：

```markdown
**tt-package-demo**

A demo package for Total TypeScript.
```

这将在人们查看你的包时显示在 npm 注册表中。

## 3: TypeScript

在本节中，我们将安装 TypeScript，设置 `tsconfig.json`，创建源文件，创建索引文件，设置构建脚本，运行构建，添加 `dist` 到 `.gitignore`，设置 CI 脚本，并为 DOM 配置我们的 `tsconfig.json`。


### 3.1: 安装 TypeScript

运行以下命令来安装 TypeScript：

```bash
npm install --save-dev typescript
```

我们添加 `--save-dev` 是将 TypeScript 安装为开发依赖。这意味着在用户安装你的包时，TypeScript 不会被包含其中。


### 3.2: 设置 `tsconfig.json`

创建一个 `tsconfig.json` 文件，并填入以下内容：

```json
{
  "compilerOptions": {
    /* 基本选项： */
    "esModuleInterop": true,
    "skipLibCheck": true,
    "target": "es2022",
    "allowJs": true,
    "resolveJsonModule": true,
    "moduleDetection": "force",
    "isolatedModules": true,
    "verbatimModuleSyntax": true,

    /* 严格性 */
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "noImplicitOverride": true,

    /* 如果使用 TypeScript 进行转译： */
    "module": "NodeNext",
    "outDir": "dist",
    "rootDir": "src",
    "sourceMap": true,

    /* 并且如果你在构建一个库： */
    "declaration": true,

    /* 并且如果你在单仓库中构建一个库： */
    "declarationMap": true
  }
}
```

这些选项在我的 [TSConfig 速查表](链接) 中有详细说明。


### 3.3: 为 DOM 配置 `tsconfig.json`

如果你的代码在 DOM 中运行（即需要访问 `document`、`window` 或 `localStorage` 等），请跳过此步骤。

如果你的代码不需要访问 DOM API，请在 `tsconfig.json` 中添加以下内容：

```json
{
  "compilerOptions": {
    // ...其他选项
    "lib": ["es2022"]
  }
}
```

这将防止 DOM 类型定义在你的代码中可用。

如果你不确定，跳过此步骤。


### 3.4: 创建源文件

创建一个 `src/utils.ts` 文件，并添加以下内容：

```typescript
export const add = (a: number, b: number) => a + b;
```


### 3.5: 创建索引文件

创建一个 `src/index.ts` 文件，并添加以下内容：

```typescript
export { add } from "./utils.js";
```

`.js` 扩展名看起来有些奇怪。本文中有更多解释。


### 3.6: 设置构建脚本

在 `package.json` 中添加一个 `scripts` 对象，并填入以下内容：

```json
{
  "scripts": {
    "build": "tsc"
  }
}
```

这将编译你的 TypeScript 代码为 JavaScript。


### 3.7: 运行构建

运行以下命令来编译你的 TypeScript 代码：

```bash
npm run build
```

这将创建一个 `dist` 文件夹，里面包含编译后的 JavaScript 代码。


### 3.8: 将 `dist` 添加到 `.gitignore`

在你的 `.gitignore` 文件中添加 `dist` 文件夹：

```
dist
```

这将防止你的编译代码被包含在 Git 仓库中。


### 3.9: 设置 CI 脚本

在 `package.json` 中添加一个 `ci` 脚本，并填入以下内容：

```json
{
  "scripts": {
    "ci": "npm run build"
  }
}
```

这为在 CI 上运行所有必要操作提供了一个快捷方式。


这样，你已经完成了 TypeScript 的安装和配置，设置了必要的构建脚本，并确保编译后的代码不会被提交到 Git 仓库中。

## 4: Prettier

在本节中，我们将安装 Prettier，设置 `.prettierrc` 文件，设置格式化脚本，运行格式化脚本，设置检查格式脚本，将检查格式脚本添加到 CI 脚本中，并运行 CI 脚本。

Prettier 是一个代码格式化工具，它会自动将你的代码格式化为一致的风格，从而使代码更易于阅读和维护。


### 4.1: 安装 Prettier

运行以下命令来安装 Prettier：

```bash
npm install --save-dev prettier
```


### 4.2: 设置 `.prettierrc`

创建一个 `.prettierrc` 文件，并添加以下内容：

```json
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "all",
  "printWidth": 80,
  "tabWidth": 2
}
```

你可以在此文件中添加更多选项以自定义 Prettier 的行为。完整的选项列表可以在[这里](链接)找到。


### 4.3: 设置格式化脚本

在 `package.json` 中添加一个 `format` 脚本，并填入以下内容：

```json
{
  "scripts": {
    "format": "prettier --write ."
  }
}
```

这将使用 Prettier 格式化项目中的所有文件。


### 4.4: 运行格式化脚本

运行以下命令来格式化项目中的所有文件：

```bash
npm run format
```

你可能会注意到一些文件发生了变化。使用以下命令将它们提交：

```bash
git add .
git commit -m "Format code with Prettier"
```


### 4.5: 设置检查格式脚本

在 `package.json` 中添加一个 `check-format` 脚本，并填入以下内容：

```json
{
  "scripts": {
    "check-format": "prettier --check ."
  }
}
```

这将检查项目中的所有文件是否格式化正确。


### 4.6: 将其添加到 CI 脚本

在 `package.json` 中的 CI 脚本中添加 `check-format` 脚本：

```json
{
  "scripts": {
    "ci": "npm run build && npm run check-format"
  }
}
```

这将使 `check-format` 脚本成为 CI 过程的一部分。


通过这些步骤，你已经成功配置了 Prettier，以确保你的代码风格一致，并将其集成到了 CI 流程中。

## 5: `exports`、`main` 和 `@arethetypeswrong/cli`

在本节中，我们将安装 `@arethetypeswrong/cli`，设置 `check-exports` 脚本，运行 `check-exports` 脚本，设置 `main` 字段，再次运行 `check-exports` 脚本，设置 CI 脚本并运行。

`@arethetypeswrong/cli` 是一个工具，用于检查你的包的导出是否正确。这非常重要，因为错误的导出会导致使用你包的用户遇到问题。


### 5.1: 安装 `@arethetypeswrong/cli`

运行以下命令来安装 `@arethetypeswrong/cli`：

```bash
npm install --save-dev @arethetypeswrong/cli
```


### 5.2: 设置 `check-exports` 脚本

在 `package.json` 中添加一个 `check-exports` 脚本，并填入以下内容：

```json
{
  "scripts": {
    "check-exports": "attw --pack ."
  }
}
```

这将检查你的包的所有导出是否正确。


### 5.3: 运行 `check-exports` 脚本

运行以下命令来检查包的所有导出是否正确：

```bash
npm run check-exports
```

你应该会注意到出现了多个错误：

```
┌───────────────────┬──────────────────────┐
│                   │ "tt-package-demo"    │
├───────────────────┼──────────────────────┤
│ node10            │ 💀 Resolution failed │
├───────────────────┼──────────────────────┤
│ node16 (from CJS) │ 💀 Resolution failed │
├───────────────────┼──────────────────────┤
│ node16 (from ESM) │ 💀 Resolution failed │
├───────────────────┼──────────────────────┤
│ bundler           │ 💀 Resolution failed │
└───────────────────┴──────────────────────┘
```

这表明没有版本的 Node 或任何打包工具能够使用我们的包。

让我们来修复这个问题。

---

### 5.4: 设置 `main` 字段

在 `package.json` 中添加一个 `main` 字段，并填入以下内容：

```json
{
  "main": "dist/index.js"
}
```

这告诉 Node 在哪里找到你的包的入口点。


### 5.5: 再次运行 `check-exports` 脚本

运行以下命令再次检查包的所有导出是否正确：

```bash
npm run check-exports
```

你应该会注意到只剩下一个警告：

```
┌───────────────────┬──────────────────────────────┐
│                   │ "tt-package-demo"            │
├───────────────────┼──────────────────────────────┤
│ node10            │ 🟢                           │
├───────────────────┼──────────────────────────────┤
│ node16 (from CJS) │ ⚠️ ESM (dynamic import only) │
├───────────────────┼──────────────────────────────┤
│ node16 (from ESM) │ 🟢 (ESM)                     │
├───────────────────┼──────────────────────────────┤
│ bundler           │ 🟢                           │
└───────────────────┴──────────────────────────────┘
```

这表明我们的包与运行 ESM 的系统兼容。使用 CJS 的人（通常在旧系统中）需要使用动态导入来引入它。

---

### 5.6: 修复 CJS 警告

如果你不想支持 CJS（我推荐这样做），将 `check-exports` 脚本更改为：

```json
{
  "scripts": {
    "check-exports": "attw --pack . --ignore-rules=cjs-resolves-to-esm"
  }
}
```

现在，运行 `check-exports` 将显示所有内容都为绿色：

```
┌───────────────────┬───────────────────┐
│                   │ "tt-package-demo" │
├───────────────────┼───────────────────┤
│ node10            │ 🟢                │
├───────────────────┼───────────────────┤
│ node16 (from CJS) │ 🟢 (ESM)          │
├───────────────────┼───────────────────┤
│ node16 (from ESM) │ 🟢 (ESM)          │
├───────────────────┼───────────────────┤
│ bundler           │ 🟢                │
└───────────────────┴───────────────────┘
```

如果你更愿意同时发布 CJS 和 ESM，请跳过此步骤。


### 5.7: 将 `check-exports` 脚本添加到 CI 脚本中

在 `package.json` 中的 CI 脚本中添加 `check-exports` 脚本：

```json
{
  "scripts": {
    "ci": "npm run build && npm run check-format && npm run check-exports"
  }
}
```


通过这些步骤，你已经成功配置了 `@arethetypeswrong/cli` 来检查包的导出，并将其集成到了 CI 流程中，确保你的包与不同的环境兼容。
## 6: 使用 `tsup` 实现双重发布

如果你想发布 CJS 和 ESM 两种格式的代码，可以使用 `tsup`。`tsup` 是基于 `esbuild` 构建的工具，它可以将你的 TypeScript 代码编译成两种格式。

我的个人建议是跳过此步骤，只发布 ES Modules。这样可以简化你的设置，避免双重发布带来的问题，如双包危害（Dual Package Hazard）。

但是，如果你想继续进行，以下是具体步骤。


### 6.1: 安装 `tsup`

运行以下命令来安装 `tsup`：

```bash
npm install --save-dev tsup
```


### 6.2: 创建 `tsup.config.ts` 文件

创建一个 `tsup.config.ts` 文件，并填入以下内容：

```typescript
import { defineConfig } from "tsup";

export default defineConfig({
  entryPoints: ["src/index.ts"],
  format: ["cjs", "esm"],
  dts: true,
  outDir: "dist",
  clean: true,
});
```

- `entryPoints` 是一个数组，指定你的包的入口点。在本例中，我们使用 `src/index.ts`。
- `format` 是一个数组，指定输出的格式。我们使用 `cjs`（CommonJS）和 `esm`（ECMAScript modules）。
- `dts` 是一个布尔值，告诉 `tsup` 生成声明文件。
- `outDir` 是输出编译代码的目录。
- `clean` 告诉 `tsup` 在构建之前清理输出目录。


### 6.3: 修改 `build` 脚本

在 `package.json` 中将 `build` 脚本修改为以下内容：

```json
{
  "scripts": {
    "build": "tsup"
  }
}
```

我们现在将使用 `tsup` 编译代码，而不是 `tsc`。


### 6.4: 添加 `exports` 字段

在 `package.json` 中添加 `exports` 字段，并填入以下内容：

```json
{
  "exports": {
    "./package.json": "./package.json",
    ".": {
      "import": "./dist/index.js",
      "default": "./dist/index.cjs"
    }
  }
}
```

`exports` 字段告诉使用你的包的程序如何找到 CJS 和 ESM 版本。在本例中，我们将使用 `import` 的用户指向 `dist/index.js`，将使用 `require` 的用户指向 `dist/index.cjs`。

建议将 `./package.json` 也添加到 `exports` 字段中，因为某些工具需要轻松访问你的 `package.json` 文件。


### 6.5: 再次运行 `check-exports` 脚本

运行以下命令来检查包的所有导出是否正确：

```bash
npm run check-exports
```

现在，所有内容都显示为绿色：

```
┌───────────────────┬───────────────────┐
│                   │ "tt-package-demo" │
├───────────────────┼───────────────────┤
│ node10            │ 🟢                │
├───────────────────┼───────────────────┤
│ node16 (from CJS) │ 🟢 (CJS)          │
├───────────────────┼───────────────────┤
│ node16 (from ESM) │ 🟢 (ESM)          │
├───────────────────┼───────────────────┤
│ bundler           │ 🟢                │
└───────────────────┴───────────────────┘
```


### 6.6: 将 TypeScript 用作 Linter

由于我们不再使用 `tsc` 来编译代码，而 `tsup` 也不会检查代码中的错误（它只是将代码转换为 JavaScript），这意味着如果我们的代码中存在 TypeScript 错误，CI 脚本不会报错。

让我们修复这个问题。


#### 6.6.1: 在 `tsconfig.json` 中添加 `noEmit`

在 `tsconfig.json` 中添加 `noEmit` 字段：

```json
{
  "compilerOptions": {
    // ...other options
    "noEmit": true
  }
}
```


#### 6.6.2: 删除 `tsconfig.json` 中未使用的字段

从 `tsconfig.json` 中删除以下字段：

- `outDir`
- `rootDir`
- `sourceMap`
- `declaration`
- `declarationMap`

在我们的新 `linting` 设置中，这些字段已不再需要。


#### 6.6.3: 可选：将 `module` 改为 `Preserve`

可选地，你现在可以将 `module` 更改为 `Preserve`：

```json
{
  "compilerOptions": {
    // ...other options
    "module": "Preserve"
  }
}
```

这意味着你将不再需要使用 `.js` 扩展名来导入文件。这意味着 `index.ts` 可以如下所示：

```typescript
export * from "./utils";
```


#### 6.6.4: 添加 `lint` 脚本

在 `package.json` 中添加一个 `lint` 脚本，并填入以下内容：

```json
{
  "scripts": {
    "lint": "tsc"
  }
}
```

这将运行 TypeScript 作为 linter。


#### 6.6.5: 将 `lint` 添加到 CI 脚本中

在 `package.json` 中的 CI 脚本中添加 `lint` 脚本：

```json
{
  "scripts": {
    "ci": "npm run build && npm run check-format && npm run check-exports && npm run lint"
  }
}
```


现在，我们可以在 CI 过程中捕获 TypeScript 错误。通过以上步骤，你已经成功配置了 `tsup` 来实现 CJS 和 ESM 的双重发布，并将 TypeScript 作为 linter 集成到 CI 流程中。

## 7: 使用 Vitest 进行测试

在这一部分，我们将安装 Vitest，创建测试，设置测试脚本，运行测试脚本，设置开发脚本，并将测试脚本添加到我们的 CI 脚本中。Vitest 是一个现代的测试运行器，支持 ESM 和 TypeScript，类似于 Jest，但更轻便和高效。


### 7.1: 安装 Vitest

运行以下命令来安装 Vitest：

```bash
npm install --save-dev vitest
```


### 7.2: 创建一个测试文件

在 `src/utils.test.ts` 文件中编写以下内容：

```typescript
import { add } from "./utils.js";
import { test, expect } from "vitest";

test("add", () => {
  expect(add(1, 2)).toBe(3);
});
```

这是一个简单的测试，它检查 `add` 函数是否返回正确的值。


### 7.3: 设置测试脚本

在 `package.json` 中添加以下 `test` 脚本：

```json
{
  "scripts": {
    "test": "vitest run"
  }
}
```

`vitest run` 将在项目中运行所有测试，且不会进行文件监听。


### 7.4: 运行测试脚本

运行以下命令来执行测试：

```bash
npm run test
```

你应该看到如下输出：

```
 ✓ src/utils.test.ts (1)
   ✓ add

 Test Files  1 passed (1)
      Tests  1 passed (1)
```

这表明你的测试成功通过。


### 7.5: 设置开发脚本

在开发过程中，通常会运行测试并让它监听文件的变化。为此，在 `package.json` 中添加一个 `dev` 脚本：

```json
{
  "scripts": {
    "dev": "vitest"
  }
}
```

这将使 Vitest 进入文件监听模式，便于开发时自动运行测试。


### 7.6: 将测试脚本添加到 CI 脚本

在 `package.json` 中，将 `test` 脚本添加到 CI 流程中：

```json
{
  "scripts": {
    "ci": "npm run build && npm run check-format && npm run check-exports && npm run lint && npm run test"
  }
}
```


现在，你已经成功集成了 Vitest 进行测试，并确保测试会作为 CI 流程的一部分进行。通过文件监听模式，你可以在开发过程中实时检查代码的正确性。
## 8: Set up CI with GitHub Actions

在这一部分，我们将创建一个 GitHub Actions 工作流，在每次提交和拉取请求时运行 CI 流程。这是确保我们的包始终处于正常工作状态的重要步骤。


### 8.1: 创建工作流

在项目的根目录下创建 `.github/workflows/ci.yml` 文件，内容如下：

```yaml
name: CI

on:
  pull_request:
  push:
    branches:
      - main

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

jobs:
  ci:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Use Node.js
        uses: actions/setup-node@v4
        with:
          node-version: "20"

      - name: Install dependencies
        run: npm install

      - name: Run CI
        run: npm run ci
```

这个文件为 GitHub 提供了运行 CI 流程的指令。

- **name**：定义工作流的名称为 `CI`。
- **on**：指定工作流何时运行。这里设置为在 `main` 分支上的每次提交和拉取请求时运行。
- **concurrency**：确保同一分支上不会同时运行多个工作流实例，`cancel-in-progress` 参数用来取消正在运行的旧实例。
- **jobs**：定义要运行的一组任务。在这个例子中，我们有一个名为 `ci` 的任务。
- **actions/checkout@v4**：拉取代码到工作环境。
- **actions/setup-node@v4**：设置 Node.js 和 npm 环境。
- **npm install**：安装项目依赖。
- **npm run ci**：运行项目的 CI 脚本。

如果 CI 过程中的任何部分失败，GitHub 将在提交旁边显示红色的 "X"，表示工作流失败。


### 8.2: 测试工作流

将你的更改推送到 GitHub，然后检查仓库的 **Actions** 选项卡。你应该会看到工作流正在运行。

这确保每次提交和每个拉取请求都将自动触发 CI 流程，为你提供对代码的持续检查与反馈。

## 9: Publishing with Changesets

在这一部分，我们将安装 Changesets、初始化它、设置发布脚本，并最终将包发布到 npm。Changesets 是一个帮助你版本化并发布包的工具，非常适合将代码发布到 npm。


### 9.1: 安装 @changesets/cli

运行以下命令安装 Changesets CLI：

```bash
npm install --save-dev @changesets/cli
```


### 9.2: 初始化 Changesets

运行以下命令来初始化 Changesets：

```bash
npx changeset init
```

此命令将创建一个 `.changeset` 文件夹，其中包含 `config.json` 文件，你的 changeset 将存储在此文件夹中。


### 9.3: 设置 Changeset 发布为公开

在 `.changeset/config.json` 中，将 `access` 字段修改为 `public`：

```json
{
  "access": "public"
}
```

这一步确保你的包能够公开发布到 npm。


### 9.4: 将 commit 设置为 true

在 `.changeset/config.json` 中，将 `commit` 字段设置为 `true`：

```json
{
  "commit": true
}
```

这样在版本发布后，changeset 会自动提交到你的代码库。


### 9.5: 设置 local-release 脚本

在 `package.json` 中添加 `local-release` 脚本：

```json
{
  "scripts": {
    "local-release": "changeset version && changeset publish"
  }
}
```

这个脚本将运行你的 CI 流程并发布包到 npm，你可以在本地运行此命令发布新的版本。


### 9.6: 在 `prepublishOnly` 中运行 CI

在 `package.json` 中添加 `prepublishOnly` 脚本：

```json
{
  "scripts": {
    "prepublishOnly": "npm run ci"
  }
}
```

这会在发布包之前自动运行 CI 流程，确保代码没有问题。


### 9.7: 添加 changeset

运行以下命令添加 changeset：

```bash
npx changeset
```

这会打开一个交互式提示，帮助你创建一个新的 changeset。你可以将这个发布标记为 `patch` 版本，并添加类似于 "Initial release" 的描述。


### 9.8: 提交你的更改

将更改提交到代码库：

```bash
git add .
git commit -m "Prepare for initial release"
```


### 9.9: 运行 local-release 脚本

运行以下命令发布你的包：

```bash
npm run local-release
```

这个命令会运行 CI 流程、版本化包并发布到 npm。在代码库中还会创建一个 `CHANGELOG.md` 文件，详细说明每次发布的更改。


### 9.10: 查看你的包

打开以下地址查看你的包：

```text
http://npmjs.com/package/<your-package-name>
```

恭喜你！你已经成功将包发布到 npm。

## 总结

现在，你已经完成了包的完整设置，包含了：

- TypeScript 项目配置
- Prettier，用于格式化和检查代码格式
- @arethetypeswrong/cli，用于检查包的导出是否正确
- tsup，用于将 TypeScript 编译为 JavaScript
- vitest，用于运行测试
- GitHub Actions，用于 CI 流程
- Changesets，用于版本化和发布包

后续还可以设置 Changesets GitHub action 和 PR bot，让贡献者在提交 PR 时自动推荐添加 changeset。如果你还有问题，请告诉我！
