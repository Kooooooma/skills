<div align="right">
  <a href="README_ZH.md">中文</a> | <a href="README.md">English</a>
</div>

# Koma Skills

> 🧰 精选 AI Skill 合集，用于专业领域任务

---

## 📦 技能列表

### 📚 FM Knowledge Guide

> 🎓 帮助银行开发者和业务分析师快速掌握金融市场业务知识的 AI Skill

![Document Viewer UI](sample/img/ui.png)

| 功能 | 描述 |
|------|------|
| 📚 **知识覆盖全面** | 涵盖衍生品、抵押品管理、交易生命周期、监管框架等核心领域 |
| 🎯 **角色自适应** | 根据用户角色（开发者/BA/运营）自动调整内容深度 |
| 📊 **Mermaid 图表** | 自动生成流程图、时序图、ER 图等可视化内容 |
| 🌐 **内置文档查看器** | 深色主题 Web 界面，支持 Markdown 渲染和图表展示 |
| 📁 **自动归档** | 生成的学习文档自动保存到 `docs/fm-guide/` 目录 |

**支持的主题**: 衍生品、抵押品管理、交易生命周期、消息标准、监管框架、风险管理

**使用方式**:

```bash
# Workflow 触发（推荐）
@/fm-knowledge-guider margin call

# Skill 安装
npx skills add Kooooooma/skills@fm-knowledge-guide -g
```

<details>
<summary>📂 项目结构</summary>

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

</details>

---

### 🛡️ Code Security Scanner

> 🔍 扫描代码仓库安全威胁的 AI Skill — 数据外泄、后门暴露、恶意代码植入、供应链攻击检测

| 功能 | 描述 |
|------|------|
| 🔴 **数据外泄检测** | 检测凭证/令牌是否被发送到外部服务器 |
| 🔴 **后门检测** | 发现隐藏端点、反向 Shell、未文档化的远程访问入口 |
| 🔴 **恶意代码检测** | 识别 `eval()`、代码混淆、`postinstall` 脚本漏洞利用 |
| 🟡 **依赖链风险** | 审计 npm 依赖的仿冒包、已知漏洞包 |
| 🟡 **文件系统风险** | 检测对 `~/.ssh/`、浏览器 Cookie、凭证存储的读取行为 |

**优化语言**: TypeScript / JavaScript / Node.js 项目

**使用方式**:

```bash
# Skill 安装
npx skills add Kooooooma/skills@code-security-scanner -g
```

**示例交互**:

| 用户输入 | 扫描范围 |
|----------|----------|
| `扫描这个项目的安全威胁` | 完整 5 阶段审计 |
| `检查这个代码库是否有后门` | 后门检测 |
| `审计 npm 依赖` | 依赖链分析 |
| `检查是否有数据外泄` | 凭证泄漏检测 |

<details>
<summary>📂 项目结构</summary>

```
code-security-scanner/
├── SKILL.md                 # Skill 主入口和扫描工作流
└── references/              # 检测规则参考
    ├── data-exfiltration.md     # 🔴 凭证/令牌泄漏模式
    ├── backdoor-detection.md    # 🔴 反向 Shell、隐藏端点模式
    ├── malicious-code-patterns.md # 🔴 eval、代码混淆、postinstall 漏洞
    ├── dependency-risks.md      # 🟡 供应链攻击模式
    └── filesystem-risks.md      # 🟡 敏感文件访问模式
```

</details>

---

## ⭐ Star History

[![Star History Chart](https://api.star-history.com/svg?repos=Kooooooma/skills&type=Date)](https://star-history.com/#Kooooooma/skills&Date)

---

## 📄 License

Apache License 2.0
