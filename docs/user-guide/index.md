# 第二大脑初始化用户指南

本指南只介绍初始化功能：把一个新领域的第二大脑从零建起来。没有你的确认，工具不会做任何远程写操作；中途失败会暂停在可恢复的状态，也不会覆盖你的无关改动。

## 初始化做什么

给定一个领域，初始化会创建该领域的第二大脑主仓库，以及该领域所需的配套资产仓库，并把配套仓库作为子模块挂载到主仓库的标准目录下。整个过程不改动其他领域，也不触碰工作区之外的内容。

## 命名规则

初始化需要两个名称：领域简称，例如 `search`；领域英文名，例如 `search-engineering`。两者由你确认后写入请求配置，主仓库取 `quanttide-{简称}`，例如 `quanttide-search`；配套资产仓库按固定模式套用简称或英文名生成，例如 `quanttide-context-of-search-engineering`。程序只检查命名格式，不臆测领域语义。

## 固定资产类型

配套资产类型是一套固定规则，每个类型都有确定的仓库名模式与挂载位置。初始化默认包含以下六类。

| **资产类型** | **仓库名** | **挂载位置** |
| :-- | :-- | :-- |
| 应用云 | `qtcloud-{简称}` | `apps/qtcloud-{简称}` |
| 工具箱 | `quanttide-{简称}-toolkit` | `packages/quanttide-{简称}-toolkit` |
| 实验室 | `quanttide-{简称}-lab` | `examples/quanttide-{简称}-lab` |
| Context | `quanttide-context-of-{英文名}` | `data/context` |
| Journal | `quanttide-journal-of-{英文名}` | `data/journal` |
| Intention | `quanttide-intention-of-{英文名}` | `data/intention` |

其余类型（如 archive、report、library）模式相同，可按需追加。

## 子模块与云端创建

配套仓库以子模块挂载，而子模块指针必须指向一个真实存在的提交。因此云端创建仓库时必须带一个初始文件，通常是一份 README，让仓库一建好就有初始提交；否则子模块无法关联，别人克隆主仓库后对应目录会是空的。工具在创建时默认写入 README 作为初始提交。

## 公开与私有

初始化默认创建公开仓库。如果这次需要在私有仓库中初始化，在请求配置里指定可见性为私有即可。可见性由配置决定，不写死，以便同一套规则既能建公开资产，也能建尚未公开的资产。

## 开始之前

需要 Python 3.10 或以上、PyYAML 6 和 Git；要操作公开 GitHub 时，还需要已登录的 GitHub CLI。工具默认在本地模式工作，只创建本地仓库；需要创建云端仓库时，在命令里指定提供方与账号或组织。用法如下。

```sh
python3 src/engine.py init --config request.yaml --workspace <工作区> --run-dir <记录目录>
```

请求配置的字段定义见工具自带的 [请求配置](../../src/cli/docs/request-schema.md)。
