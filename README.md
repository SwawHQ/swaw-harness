# Swaw Harness · Harness a million tools（御百万工具）

能否让 AI 解决问题时，沉淀工具？
若有百万工具，如何驾驭？

## 一、聚集、组装、御用（Gather · Weave · Wield）

### 聚集：把成功的方法做成工具。

按 swaw harness 规范开发工具模块（必须是无交互 cli 程序、允许指定日志和产出物的存储位置）或下载、克隆别人发布的模块。目前支持 Node/Bun/Python/UV 代码打包、Rust 二进制自动编译。

内置发布能力，能将源代码模块打包或编译为工具包，默认发布到仓库根 data/admin/packages/ 下，按 `<作者名-hash[0:2]>/<作者名-hash[2:4]>/<作者名>/<分组>/<包名>` 方式组织。

### 组装：把工具组装成为自己的技能图。
执行 `data/admin.exe core/admin/user/create user1` 新建一个 harness 用户，得到`data/user1.exe`和数据空间`data/user1/`，在`data/user1/map`下，把工具用目录树方式进行组织，建立你的技能图：

```
map/
├── toolbox1
│   ├── sub1
│   │   ├── tool1
│   │   │   └── tool.toml
│   │   └── tool2
│   │       └── tool.toml
│   └── sub2
│       └── tool1
│           └── tool.toml
├── toolbox2
│ 
├── toolbox3
```

例如，编辑`toolbox1/sub1/tool1/tool.toml`映射到示例的 helloworld 命令工具：
```
schema = "swaw.harness.tool/v2"
package = "c3/dh/swaw/templates/helloworld"
version = "1.*"
executable = "helloworld.exe"
arguments = []
```


### 御用：查找、调用工具，作为工作流组合执行。
执行一个节点：
```
data/user1.exe toolbox1/sub1/tool1
```
按前面配置，会调用`admin/packages/c3/dh/swaw/templates/helloworld`

执行一个目录树：
```
data/user1.exe toolbox1/.tree.no-structure    # 无特定顺序执行，多用于批量测试
data/user1.exe toolbox1/.tree.parent-success  # 有序执行：父目录层级的 CLI 成功才执行子目录的
data/user1.exe toolbox1/.tree                 # 基于
```

`data/user1.exe`也能基于技能图目录树，自动渲染类似 Mac Finder 的 web ui，以可视化探索/执行技能图。目录树的有序执行，可在`tool.toml`同目录追加`check.toml`，描述多个`tool.toml`之间依赖关系，从而把技能图变为工作流。


## 二、让解决问题的过程，物化为解决问题的能力

编程语言可以划分：解释型语言、编译型语言。

当下，Agent 的工作模式：
输入提示词 -> Agent 收集上下文、规划步骤、编程、调用工具 -> 完成任务

我们只能保存提示词，任务要再次执行，Agent 会重复一遍：收集上下文、规划步骤、编程……很像“解释型语言”的工作模式。

能否让 Agent 多做一步，在任务成功后，把收集上下文的途径、规划的步骤、编写的程序，沉淀为工具或工作流？后续变为：

调用工具或工作流 -> 完成任务

或仍让 Agent 辅助：

输入提示词 -> 语义理解与搜寻 -> 调用工具或工作流 -> 完成任务

仅必要时，才引入 Agent。等若把提示词，转化为可重复执行的工具或工作流，使得 Agent 的工作模式，更像“编译型语言”。


## 三、人类与 Agent 共同的能力底座

显然，这有成本：工具/工作流首次沉淀时，会多出调试等任务，沉淀后，也得管理和维护。

带来的收益是：

1. 可以预见，Token 消耗会大幅下降（后续补充实际可信的测试对比）

2. 工具/工作流，相对 Agent 背后依赖的 LLM 而言，更可预期。

3. 这些工具/工作流是沉淀在我们自己手中，没有 Agent 时候，也可以手动调用，Swaw Harness 还专门提供了 web ui。

4. 相比开发自己的 LLM/Agent，沉淀工具/工作流的门槛要低得多。

Swaw Harness 希望 AI 能力增长的同时，也增强人类自己的能力底座。

## 四、收集自己的数据经验

除`data/user1/map`还有`data/user1/runs`,每次执行一个节点或目录树，会生成一个专属的日志数据目录：`/runs/<runsId>/`。

其中保存完整的启动参数、包版本、执行结果……可用于追踪、重播、形成数据经验。我希望这些数据最终能被用来做微调与训练。

## 当前进展

## 快速理解项目结构



