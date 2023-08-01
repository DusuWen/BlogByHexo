---
title: 声明文件与JSDoc
date: 2023-08-01 16:39:00
tags: TypeScript
---

### 什么是声明文件

声明文件用以描述JavaScript模块或库的类型信息，可以让TypeScript在使用这个库时进行类型检查和智能提示。声明文件通常使用.d.ts拓展名。
<!-- more -->

### 什么时候使用声明文件

如果需要设计一个库需要同时支持js和ts，可以将.d.ts文件随着js文件一起发布，在ts项目中可以取得这个库的类型信息。或者在一个传统js项目中为一个js文件中的某些方法添加更加详细的类型标注，以便编辑器可以进行更智能的类型提示和代码补全，例如webstorm可以对一个js方法的入参返回值进行类型推断然后智能提示，但是如果人为手动添加.d.ts文件进行类型描述可以得到比webstorm自动进行的类型推断更加准确的类型标注，从而减少调用方法时因为入参或返回值的类型信息错误而引起的不必要的bug。

### 如何编写声明文件

声明文件必需以 .d.ts 为后缀，并且放在一个单独的文件内，在需要添加类型标注的js文件统计目录下添加同名的以.d.ts结尾的文件，如在index.js旁可以添加index.d.ts。在index.js中定义了一个方法，并导出给全局使用

```javascript
export const formatTime = (time, format = "YYYY-MM-DD HH:mm:ss") => {
    const formattedTime = moment(time);
    if (formattedTime.isValid()) {
        if (formattedTime.parseZone().utcOffset() === 0) {
            return formattedTime.local().format(format)
        } else {
            return formattedTime.format(format)
        }
    } else {
        return "-"
    }
};
```

示例中formatTime接受2个参数，第一个参数可为string或number，第二个参数可不传入，如不传入默认为“YYYY-MM-DD HH:mm:ss”，如果传入需要为string，同时返回一个string的返回值，如果需要添加类型说明，那么index.d.ts文件如下

```typescript
declare function formatTime(time:string | number, format?:string="YYYY-MM-DD HH:mm:ss"): string
export { formatTime }
```

在一个ts或者js项目中引入index.js和index.d.ts后都可以正常使用index.js并且编辑器会自动家在.d.ts文件来为开发者在使用formatTime方法时提供智能提示和代码补全

### 有没有必要编写声明文件

在我们的项目开发过程中，不管是使用react还是vue体系，都会沉淀大量非业务代码的基建代码，属于我们的重要资产，而一个方法类在多处业务页面调用时应该有着完善的文档来提供支持，而且随着vue3和ts的到来，众多在vue2时代积累的方法类、js纯函数将无法直接导入至ts项目中，此时通过编写声明文件提供类型推断的支持从而避免ts重构，尤其面对重要或者及其复杂的方法时，ts重构是很有风险并且耗时的事情，所以为js方法类添加声明文件可以为以后升级迁移至ts项目做前置准备。

### 另一种选择JSDoc

如果只是为了给公共基建方法添加注释以便得到完整的代码提示、类型声明还可以使用JSDoc。JSDoc是一种用于JavaScript代码文档化的标准注释格式。它允许开发人员在代码中使用特定的注释标签来描述函数、方法、变量等的用途、参数、返回值等信息，从而生成清晰、易读的代码文档。

上面的index.js就可以写成以下形式

```javascript

/**
 * format time
 * @param {string|number} time need format
 * @param {string} [format] default use YYYY-MM-DD HH:mm:ss
 * @returns {string}
 */
export const formatTime = (time, format = "YYYY-MM-DD HH:mm:ss") => {
    const formattedTime = moment(time);
    if (formattedTime.isValid()) {
        if (formattedTime.parseZone().utcOffset() === 0) {
            return formattedTime.local().format(format)
        } else {
            return formattedTime.format(format)
        }
    } else {
        return "-"
    }
};
```

以上jsdoc的示例可以得到和.d.ts一样的类型推断提示以及代码补全，还有对方法和参数的注释
