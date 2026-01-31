<div align="right">
  <a href="README.md">中文</a> | <a href="README_EN.md">English</a>
</div>

# FM Knowledge Guide

> 🎓 帮助银行开发者和业务分析师快速掌握金融市场业务知识的 AI Skill

![Document Viewer UI](sample/img/ui.png)

---

## ✨ 功能特点

| 功能 | 描述 |
|------|------|
| 📚 **知识覆盖全面** | 涵盖衍生品、抵押品管理、交易生命周期、监管框架等核心领域 |
| 🎯 **角色自适应** | 根据用户角色（开发者/BA/运营）自动调整内容深度 |
| 📊 **Mermaid 图表** | 自动生成流程图、时序图、ER 图等可视化内容 |
| 🌐 **内置文档查看器** | 深色主题 Web 界面，支持 Markdown 渲染和图表展示 |
| 📁 **自动归档** | 生成的学习文档自动保存到 `docs/fm-guide/` 目录 |

## 📖 支持的主题

- **衍生品 (Derivatives)**: Swaps, Options, Futures, Forwards, IRS, CCS
- **抵押品管理 (Collateral)**: CSA, Margin Call, VM/IM, Haircut, Threshold  
- **交易生命周期 (Trade Lifecycle)**: Confirmation, Clearing, Settlement, DVP
- **消息标准 (Messaging)**: SWIFT MT/MX, FIX, FpML, ISO 20022
- **监管框架 (Regulations)**: EMIR, Dodd-Frank, MiFID II, Basel III, UMR
- **风险管理 (Risk)**: VaR, PFE, CVA, Greeks

---

## 🚀 使用方式

### 方式一：Workflow 触发（推荐）

在支持 Workflow 的 AI 编辑器中，直接使用 `@/fm-knowledge-guider` 触发：

```
@/fm-knowledge-guider margin call
@/fm-knowledge-guider SWIFT
@/fm-knowledge-guider CSA
```

**特点**：
- ✅ 无需安装，开箱即用
- ✅ 自动加载相关参考资料
- ✅ 自动生成文档并启动查看器

---

### 方式二：Skill 安装

通过 `npx skills` 命令安装到本地：

```bash
# 安装 skill
npx skills add Kooooooma/skills@fm-knowledge-guide -g -y

# 验证安装
npx skills list | grep fm-knowledge-guide
```

安装后，AI 助手会自动识别金融市场相关问题并调用此 Skill。

---

## 📂 输出说明

生成的学习文档保存在项目的 `docs/fm-guide/` 目录：

```
docs/fm-guide/
├── margin-call-guide.md    # Margin Call 学习指南
├── swift-guide.md          # SWIFT 消息标准指南
├── csa-guide.md            # CSA 抵押品协议指南
└── viewer.html             # 文档查看器
```

### 启动文档查看器

```bash
npx -y http-server docs/fm-guide -p 0 -o /viewer.html
```

浏览器将自动打开，左侧边栏显示所有已生成的文档，右侧渲染 Markdown 内容和 Mermaid 图表。

---

## 🏗️ 项目结构

```
fm-knowledge-guide/
├── SKILL.md                 # Skill 主入口和工作流定义
├── references/              # 金融领域参考资料
│   ├── collateral-management.md
│   ├── derivatives-basics.md
│   ├── trade-lifecycle.md
│   ├── messaging-standards.md
│   ├── regulatory-framework.md
│   ├── risk-management.md
│   ├── market-data.md
│   ├── domain-overview.md
│   └── glossary.md
└── templates/
    ├── learning-guide-template.md  # 文档生成模板
    └── viewer.html                 # Web 查看器模板
```

---

## 📝 示例交互

| 用户输入 | 生成内容 |
|----------|----------|
| `margin call` | Margin Call 完整生命周期、VM vs IM 对比、计算公式 |
| `SWIFT` | MT/MX 消息格式、ISO 20022 迁移、字段映射 |
| `CSA` | ISDA 框架、抵押品条款、系统实现要点 |
| `trade lifecycle` | 交易前/执行/交易后全流程、T+2 结算 |

---

## ⭐ Star History

[![Star History Chart](https://api.star-history.com/svg?repos=Kooooooma/skills&type=Date)](https://star-history.com/#Kooooooma/skills&Date)

---

## 📄 License

Apache License 2.0
