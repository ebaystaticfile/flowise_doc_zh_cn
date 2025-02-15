# 节点构建

### 安装 Git

首先，安装 Git 并克隆 Flowise 仓库。您可以按照[入门](../getting-started/#for-developers)指南中的步骤操作。

### 结构

Flowise 将每个节点集成都放在 `packages/components/nodes` 文件夹下。让我们尝试创建一个简单的工具！

### 创建计算器工具

在 `packages/components/nodes/tools` 文件夹下创建一个名为 `Calculator` 的新文件夹。然后创建一个名为 `Calculator.ts` 的新文件。在这个文件中，我们将首先编写基类。

代码块 0 (JavaScript)
```javascript
import { INode } from '../../../src/Interface'
import { getBaseClasses } from '../../../src/utils'

class Calculator_Tools implements INode {
    label: string
    name: string
    version: number
    description: string
    type: string
    icon: string
    category: string
    author: string
    baseClasses: string[]

    constructor() {
        this.label = 'Calculator'
        this.name = 'calculator'
        this.version = 1.0
        this.type = 'Calculator'
        this.icon = 'calculator.svg'
        this.category = 'Tools'
        this.author = 'Your Name'
        this.description = 'Perform calculations on response'
        this.baseClasses = [this.type, ...getBaseClasses(Calculator)]
    }
}

module.exports = { nodeClass: Calculator_Tools }
```
代码块 1

每个节点都将实现 `INode` 基类。每个属性的含义如下：

<table><thead><tr><th width="271">属性</th><th>描述</th></tr></thead><tbody><tr><td>label</td><td>在 UI 上显示的节点名称</td></tr><tr><td>name</td><td>代码使用的名称。必须为 **驼峰式命名法**</td></tr><tr><td>version</td><td>节点的版本</td></tr><tr><td>type</td><td>通常与 label 相同。用于定义在 UI 上哪些节点可以连接到此特定类型</td></tr><tr><td>icon</td><td>节点的图标</td></tr><tr><td>category</td><td>节点的类别</td></tr><tr><td>author</td><td>节点的创建者</td></tr><tr><td>description</td><td>节点描述</td></tr><tr><td>baseClasses</td><td>节点的基类，因为节点可以从基组件扩展。用于定义在 UI 上哪些节点可以连接到此节点</td></tr></tbody></table>

### 定义类

现在组件类已部分完成，我们可以继续定义实际的工具类，在本例中为 `Calculator`。

在同一个 `Calculator` 文件夹下创建一个新文件，并命名为 `core.ts`

代码块 2 (JavaScript)
```javascript
import { Parser } from "expr-eval"
import { Tool } from "@langchain/core/tools"

export class Calculator extends Tool {
    name = "calculator"
    description = `Useful for getting the result of a math expression. The input to this tool should be a valid mathematical expression that could be executed by a simple calculator.`

    async _call(input: string) {
        try {
            return Parser.evaluate(input).toString()
        } catch (error) {
            return "I don't know how to do that."
        }
    }
}
```
代码块 3

### 完成

返回 `Calculator.ts` 文件，我们可以通过添加 `async init` 函数来完成它。在这个函数中，我们将初始化上面创建的 `Calculator` 类。当执行流程时，将调用每个节点中的 `init` 函数，而当 LLM 决定调用此工具时，将执行 `_call` 函数。

```javascript
import { INode } from '../../../src/Interface'
import { getBaseClasses } from '../../../src/utils'
import { Calculator } from './core'

class Calculator_Tools implements INode {
    label: string
    name: string
    version: number
    description: string
    type: string
    icon: string
    category: string
    author: string
    baseClasses: string[]

    constructor() {
        this.label = 'Calculator'
        this.name = 'calculator'
        this.version = 1.0
        this.type = 'Calculator'
        this.icon = 'calculator.svg'
        this.category = 'Tools'
        this.author = 'Your Name'
        this.description = 'Perform calculations on response'
        this.baseClasses = [this.type, ...getBaseClasses(Calculator)]
    }


    async init() {
        return new Calculator()
    }
}

module.exports = { nodeClass: Calculator_Tools }
```

### 构建和运行

在 `packages/server` 内的 `.env` 文件中，创建一个新的环境变量：

```javascript
SHOW_COMMUNITY_NODES=true
```

现在我们可以使用 `pnpm build` 和 `pnpm start` 来启动组件！

<figure><img src="../.gitbook/assets/image (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>
