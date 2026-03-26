# CCteam-creator 安装和使用指南

## 一、安装方法

### 方法 1：手动安装（推荐）

```bash
# 1. 克隆仓库
git clone https://github.com/simonggx/CCteam-creator.git

# 2. 安装英文版
cp -r CCteam-creator/skills/CCteam-creator ~/.codebuddy/skills/CCteam-creator

# 或安装中文版
cp -r CCteam-creator/cn/skills/CCteam-creator ~/.codebuddy/skills/CCteam-creator
```

### 方法 2：项目级安装（团队共享）

```bash
# 在项目根目录下安装，方便团队成员共享
cp -r CCteam-creator/skills/CCteam-creator .codebuddy/skills/CCteam-creator
```

### 验证安装

```bash
# 检查安装是否成功
ls ~/.codebuddy/skills/CCteam-creator/
# 应该看到: SKILL.md  references/
```

---

## 二、使用方法

### 2.1 触发团队创建

在 CodeBuddy 中，使用以下任一方式触发：

**命令方式**：
```
/CCteam-creator
```

**自然语言方式**：
```
为我的电商项目创建一个团队
set up a team for my project
帮我组建一个开发团队
build a team for my API project
```

**触发关键词**：`team`、`swarm`、`start project`、`set up project`、`create team`、`build team`

### 2.2 团队配置流程

创建团队时会经历以下步骤：

**Step 1: 需求咨询**
- Team-lead 会介绍团队机制
- 了解你的项目需求、技术栈、目标
- 推荐合适的角色组合

**Step 2: 确认方案**
- 确认项目名称（kebab-case 格式，如 `my-api`）
- 确认角色列表
- 确认阶段划分

**Step 3-4: 创建团队**
- 生成 `.plans/<project>/` 目录结构
- 生成 `CODEBUDDY.md` 运营手册
- 派生各角色的 agent

**Step 5: 开始工作**
- Team-lead 分配任务
- 各 agent 并行工作

### 2.3 可用角色

| 角色 | 名称 | 能力 |
|------|------|------|
| 后端开发 | `backend-dev` | 服务端代码 + TDD + API 开发 |
| 前端开发 | `frontend-dev` | 客户端代码 + TDD + 组件开发 |
| 研究员 | `researcher` | 代码搜索 + 技术调研（只读） |
| E2E 测试 | `e2e-tester` | Playwright 测试 + 浏览器自动化 |
| 代码审查 | `reviewer` | 安全/质量/性能审查（只读） |
| 代码清理 | `cleaner` | 死代码清理 + 重构 |

### 2.4 项目文件结构

创建团队后，项目会生成以下结构：

```
.plans/<project>/
├── task_plan.md          # 主计划（导航地图）
├── findings.md           # 团队级发现汇总
├── progress.md           # 工作日志
├── decisions.md          # 架构决策记录
├── docs/                 # 项目知识库
│   ├── architecture.md   # 系统架构
│   ├── api-contracts.md  # API 定义
│   └── invariants.md     # 系统边界约束
├── backend-dev/          # 后端开发工作目录
│   ├── findings.md       # 任务索引
│   └── task-xxx/         # 具体任务文件夹
├── researcher/           # 研究员工作目录
└── reviewer/             # 审查员工作目录
```

### 2.5 开发工作流

```
Phase 0: 需求对齐
    ↓
Phase 1: 技术调研
    ↓
Phase 2: 核心开发（TDD + 垂直切片）
    ↓
Phase 3: 集成测试
    ↓
Phase 4: 审查清理
```

### 2.6 常用命令

| 操作 | 命令 |
|------|------|
| 检查任务状态 | `TaskList` |
| 查看某 agent 进度 | `Read .plans/<project>/<agent>/progress.md` |
| 给 agent 发消息 | `SendMessage(to: "backend-dev", message: "...")` |
| 查看主计划 | `Read .plans/<project>/task_plan.md` |

### 2.7 注意事项

1. **所有角色使用 GLM-5.0 模型**
2. **Team-lead 是主对话**，不是派生的 agent
3. **进度持久化在文件中**，上下文压缩不会丢失工作
4. **大型任务需要审查**，小修改不需要
5. **Doc-Code 同步**：API 变更必须更新 `docs/api-contracts.md`

---

## 三、示例对话

```
用户: 帮我创建一个团队来开发用户认证系统

Team-lead: 好的，让我了解一下你的需求：
1. 这是一个新项目还是在现有代码基础上开发？
2. 需要哪些功能？（注册、登录、OAuth、JWT？）
3. 技术栈是什么？

用户: 新项目，需要注册登录和 JWT，用 Node.js + React

Team-lead: 根据你的需求，我推荐以下团队配置：
- backend-dev: Node.js API 开发
- frontend-dev: React 前端开发
- researcher: 技术方案调研
- reviewer: 代码审查

确认项目名称: auth-system
确认开始创建团队？

用户: 确认

Team-lead: [创建团队和文件...]
团队已创建完成！当前成员：
| 名称 | 角色 | 状态 |
|------|------|------|
| backend-dev | 后端开发 | 就绪 |
| frontend-dev | 前端开发 | 就绪 |
| researcher | 研究员 | 就绪 |
| reviewer | 审查员 | 就绪 |

Phase 1: 技术调研已启动...
```
