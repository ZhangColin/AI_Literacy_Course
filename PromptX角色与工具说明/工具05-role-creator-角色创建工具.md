# role-creator - 角色创建工具

## 工具概述

role-creator是PromptX的角色创建工具，让女娲能够创建和管理AI角色。

**核心定位：**
- 女娲的专属工具
- DPML文件生成器
- 角色结构管理器
- 角色验证工具

**设计理念：**
- 规范化：严格遵循DPML规范
- 模块化：支持分文件组织
- 验证性：确保角色可用
- 简洁性：接口清晰简单

**重要说明：**
⚠️ 此工具主要由女娲角色使用，普通用户通过与女娲对话来创建角色。

## 核心功能

### 1. 创建角色（create）

**功能说明：**
创建新的AI角色

**参数：**
```javascript
{
  action: 'create',
  roleId: 'my-assistant',  // 角色ID（小写+连字符）
  role: {
    metadata: {
      name: '我的助手',
      description: '专属AI助手',
      version: '1.0.0'
    },
    personality: [
      {
        ref: '@!thought://problem-solving'
      }
    ],
    principle: [
      {
        ref: '@!execution://workflow'
      }
    ],
    knowledge: [
      {
        ref: '@!knowledge://domain-knowledge'
      }
    ]
  },
  files: {
    thought: {
      'problem-solving': {
        exploration: '如何探索问题...',
        reasoning: '如何推理...'
      }
    },
    execution: {
      'workflow': {
        process: '工作流程...',
        constraint: '约束条件...'
      }
    },
    knowledge: {
      'domain-knowledge': '专业知识内容...'
    }
  }
}
```

**返回：**
```javascript
{
  success: true,
  roleId: 'my-assistant',
  filesCreated: [
    '/path/to/my-assistant/my-assistant.role.md',
    '/path/to/my-assistant/thought/problem-solving.thought.md',
    '/path/to/my-assistant/execution/workflow.execution.md',
    '/path/to/my-assistant/knowledge/domain-knowledge.knowledge.md'
  ],
  validated: true
}
```

**文件结构：**
```
my-assistant/
├── my-assistant.role.md       # 主文件
├── thought/                    # 思维模式文件夹
│   └── problem-solving.thought.md
├── execution/                  # 执行流程文件夹
│   └── workflow.execution.md
└── knowledge/                  # 知识文件夹
    └── domain-knowledge.knowledge.md
```

**使用场景：**
- 女娲创建新角色
- 基于ISSUE探索结果生成角色

### 2. 更新角色（update）

**功能说明：**
更新现有角色

**参数：**
```javascript
{
  action: 'update',
  roleId: 'my-assistant',
  files: {
    // 要更新的文件
    thought: {
      'problem-solving': {
        exploration: '更新后的探索方式...'
      }
    },
    // 新增文件
    execution: {
      'new-process': {
        process: '新流程...'
      }
    }
  },
  // 更新主文件引用
  updateReferences: true
}
```

**返回：**
```javascript
{
  success: true,
  roleId: 'my-assistant',
  filesUpdated: ['thought/problem-solving.thought.md'],
  filesCreated: ['execution/new-process.execution.md'],
  referencesUpdated: true
}
```

**使用场景：**
- 女娲优化现有角色
- 添加新的能力模块
- 修正角色定义

### 3. 删除角色（delete）

**功能说明：**
删除角色及其所有文件

**参数：**
```javascript
{
  action: 'delete',
  roleId: 'my-assistant',
  confirm: true  // 安全确认
}
```

**返回：**
```javascript
{
  success: true,
  roleId: 'my-assistant',
  filesDeleted: 5
}
```

**安全机制：**
- 需要确认参数
- 删除前备份（可选）
- 不可恢复操作

**使用场景：**
- 清理不需要的角色
- 重新创建角色前

### 4. 验证角色（validate）

**功能说明：**
验证角色DPML规范合规性

**参数：**
```javascript
{
  action: 'validate',
  roleId: 'my-assistant'
}
```

**返回：**
```javascript
{
  success: true,
  valid: true,
  errors: [],
  warnings: [
    'knowledge文件较大，建议拆分'
  ],
  structure: {
    hasPersonality: true,
    hasPrinciple: true,
    hasKnowledge: true,
    fileCount: 4,
    referencesValid: true
  }
}
```

**验证项目：**
- DPML标签规范
- 文件结构完整性
- 引用关系正确性
- 文件命名规范
- 内容格式正确

**使用场景：**
- 创建后验证
- 更新后检查
- 问题诊断

### 5. 读取角色（read）

**功能说明：**
读取角色的完整定义

**参数：**
```javascript
{
  action: 'read',
  roleId: 'my-assistant',
  includeFiles: true  // 是否包含所有文件内容
}
```

**返回：**
```javascript
{
  success: true,
  roleId: 'my-assistant',
  role: {
    metadata: { /* 元数据 */ },
    personality: [ /* 引用列表 */ ],
    principle: [ /* 引用列表 */ ],
    knowledge: [ /* 引用列表 */ ]
  },
  files: {
    thought: { /* 文件内容 */ },
    execution: { /* 文件内容 */ },
    knowledge: { /* 文件内容 */ }
  }
}
```

**使用场景：**
- 女娲读取现有角色
- 理解角色定义
- 准备更新角色

### 6. 列出角色（list）

**功能说明：**
列出所有可用角色

**参数：**
```javascript
{
  action: 'list',
  scope: 'user'  // 'system' | 'project' | 'user' | 'all'
}
```

**返回：**
```javascript
{
  success: true,
  roles: [
    {
      roleId: 'my-assistant',
      name: '我的助手',
      description: '专属AI助手',
      scope: 'user',
      created: '2024-01-01T12:00:00Z',
      modified: '2024-01-02T12:00:00Z'
    }
  ],
  total: 10
}
```

**作用域：**
- system: 系统内置角色
- project: 项目级角色
- user: 用户自定义角色
- all: 所有角色

**使用场景：**
- 查看可用角色
- 选择基础模板
- 管理角色

### 7. 复制角色（clone）

**功能说明：**
复制现有角色作为新角色的基础

**参数：**
```javascript
{
  action: 'clone',
  sourceRoleId: 'assistant',
  targetRoleId: 'my-assistant',
  modifications: {
    metadata: {
      name: '我的助手',
      description: '基于assistant的自定义版本'
    }
  }
}
```

**返回：**
```javascript
{
  success: true,
  sourceRoleId: 'assistant',
  targetRoleId: 'my-assistant',
  filesCloned: 5
}
```

**使用场景：**
- 基于现有角色定制
- 快速创建相似角色
- 学习参考

### 8. 导出角色（export）

**功能说明：**
导出角色为可分享的包

**参数：**
```javascript
{
  action: 'export',
  roleId: 'my-assistant',
  output: '/path/to/export/my-assistant.zip',
  includeMetadata: true
}
```

**返回：**
```javascript
{
  success: true,
  roleId: 'my-assistant',
  outputPath: '/path/to/export/my-assistant.zip',
  fileSize: 10240
}
```

**使用场景：**
- 分享角色
- 备份角色
- 迁移角色

### 9. 导入角色（import）

**功能说明：**
导入外部角色包

**参数：**
```javascript
{
  action: 'import',
  packagePath: '/path/to/role-package.zip',
  targetRoleId: 'imported-role',  // 可选，自定义ID
  validate: true
}
```

**返回：**
```javascript
{
  success: true,
  roleId: 'imported-role',
  filesImported: 5,
  validated: true
}
```

**使用场景：**
- 导入分享的角色
- 恢复备份
- 跨环境迁移

### 10. 生成文档（generateDoc）

**功能说明：**
为角色生成说明文档

**参数：**
```javascript
{
  action: 'generateDoc',
  roleId: 'my-assistant',
  output: '/path/to/doc/my-assistant.md',
  format: 'markdown'  // 'markdown' | 'html'
}
```

**返回：**
```javascript
{
  success: true,
  roleId: 'my-assistant',
  outputPath: '/path/to/doc/my-assistant.md'
}
```

**使用场景：**
- 生成角色文档
- 用户指南
- 开发文档

## DPML规范详解

### 文件结构规范

**标准目录结构：**
```
{roleId}/
├── {roleId}.role.md          # 主文件（必需）
├── thought/                   # 思维模式（可选）
│   ├── {name}.thought.md
│   └── ...
├── execution/                 # 执行流程（可选）
│   ├── {name}.execution.md
│   └── ...
└── knowledge/                 # 知识体系（可选）
    ├── {name}.knowledge.md
    └── ...
```

### 主文件格式

**{roleId}.role.md：**
```markdown
<role>

<personality>
<reference protocol="thought" resource="problem-solving" />
<reference protocol="thought" resource="analytical-thinking" />
</personality>

<principle>
<reference protocol="execution" resource="workflow" />
<reference protocol="execution" resource="quality-check" />
</principle>

<knowledge>
<reference protocol="knowledge" resource="domain-knowledge" />
</knowledge>

</role>
```

**核心标签：**
- `<role>`: 角色根容器
- `<personality>`: 思维模式容器
- `<principle>`: 行为准则容器
- `<knowledge>`: 私有知识容器
- `<reference>`: 引用标记

### Thought文件格式

**{name}.thought.md：**
```markdown
<thought>

<exploration>
开放性探索的方式...
</exploration>

<reasoning>
逻辑推理的方法...
</reasoning>

<challenge>
批判性思考的角度...
</challenge>

<plan>
方案规划的步骤...
</plan>

</thought>
```

**子标签（按需使用）：**
- `<exploration>`: 开放性探索
- `<reasoning>`: 逻辑推理
- `<challenge>`: 批判性思考
- `<plan>`: 方案规划

### Execution文件格式

**{name}.execution.md：**
```markdown
<execution>

<process>
主干流程步骤...
</process>

<constraint>
硬性约束条件...
</constraint>

<rule>
IF-THEN规则...
</rule>

<guideline>
最佳实践指导...
</guideline>

<criteria>
验收标准...
</criteria>

</execution>
```

**子标签（按需使用）：**
- `<process>`: 主干流程（基本必需）
- `<constraint>`: 硬性约束
- `<rule>`: 条件规则
- `<guideline>`: 最佳实践
- `<criteria>`: 验收标准

### Knowledge文件格式

**{name}.knowledge.md：**
```markdown
<knowledge>

# 知识标题

## 子主题

具体知识内容...

- 要点1
- 要点2

</knowledge>
```

**特点：**
- 不需要子标签
- 直接使用Markdown组织内容
- 填补语义鸿沟
- 只放私有信息

## 使用场景详解

### 场景1：女娲创建新角色（ISSUE流程）

**女娲的工作流程：**
```javascript
// 1. ISSUE探索后，女娲准备创建角色

// 2. 设计角色结构
const roleDesign = {
  roleId: 'technical-writer',
  metadata: {
    name: '技术文档写手',
    description: '专业的技术文档创作助手'
  },
  thoughtFiles: ['structured-thinking', 'reader-empathy'],
  executionFiles: ['writing-workflow', 'quality-check'],
  knowledgeFiles: ['tech-stack', 'writing-standards']
};

// 3. 调用role-creator创建
await roleCreator.execute({
  action: 'create',
  roleId: roleDesign.roleId,
  role: {
    metadata: roleDesign.metadata,
    personality: [
      { ref: '@!thought://structured-thinking' },
      { ref: '@!thought://reader-empathy' }
    ],
    principle: [
      { ref: '@!execution://writing-workflow' },
      { ref: '@!execution://quality-check' }
    ],
    knowledge: [
      { ref: '@!knowledge://tech-stack' },
      { ref: '@!knowledge://writing-standards' }
    ]
  },
  files: {
    thought: {
      'structured-thinking': {
        reasoning: '结构化思考方法...',
        plan: '文档规划步骤...'
      },
      'reader-empathy': {
        exploration: '读者视角探索...'
      }
    },
    execution: {
      'writing-workflow': {
        process: '写作流程步骤...',
        criteria: '质量标准...'
      },
      'quality-check': {
        guideline: '检查指南...'
      }
    },
    knowledge: {
      'tech-stack': '团队技术栈：React, Node.js...',
      'writing-standards': '文档标准：格式、用语...'
    }
  }
});

// 4. 验证创建结果
const validation = await roleCreator.execute({
  action: 'validate',
  roleId: 'technical-writer'
});

// 5. 通知用户
console.log('角色创建成功！可以通过discover查看和激活。');
```

### 场景2：女娲优化现有角色

**流程：**
```javascript
// 1. 用户："我想让这个角色增加XX能力"

// 2. 女娲读取现有角色
const existingRole = await roleCreator.execute({
  action: 'read',
  roleId: 'technical-writer',
  includeFiles: true
});

// 3. 分析需求，设计新能力

// 4. 更新角色
await roleCreator.execute({
  action: 'update',
  roleId: 'technical-writer',
  files: {
    thought: {
      // 新增思维模式
      'creative-thinking': {
        exploration: '创意思维方式...',
        challenge: '突破常规的方法...'
      }
    },
    execution: {
      // 更新工作流程
      'writing-workflow': {
        process: '更新后的写作流程...',
        // 其他部分保持不变
      }
    }
  },
  updateReferences: true
});

// 5. 验证更新
const validation = await roleCreator.execute({
  action: 'validate',
  roleId: 'technical-writer'
});
```

### 场景3：基于模板快速创建

**流程：**
```javascript
// 1. 克隆现有角色
await roleCreator.execute({
  action: 'clone',
  sourceRoleId: 'writer',
  targetRoleId: 'blog-writer',
  modifications: {
    metadata: {
      name: '博客写手',
      description: '专注博客写作的助手'
    }
  }
});

// 2. 自定义修改
await roleCreator.execute({
  action: 'update',
  roleId: 'blog-writer',
  files: {
    knowledge: {
      'blog-standards': '博客写作标准和风格...'
    }
  }
});
```

## 最佳实践

### 1. 遵循DPML规范

```javascript
// ✅ 正确：使用标准标签
const role = {
  personality: [
    { ref: '@!thought://problem-solving' }
  ]
};

// ❌ 错误：自定义标签
const role = {
  personality: [
    { ref: '@!custom://my-thinking' }  // 不符合规范
  ]
};
```

### 2. 合理组织文件

```javascript
// ✅ 推荐：按职责分离
files: {
  thought: {
    'analytical-thinking': { /* 分析思维 */ },
    'creative-thinking': { /* 创意思维 */ }
  },
  execution: {
    'main-workflow': { /* 主流程 */ },
    'error-handling': { /* 错误处理 */ }
  }
}

// ❌ 不推荐：全部放在一个文件
files: {
  thought: {
    'all-thinking': { /* 所有思维混在一起 */ }
  }
}
```

### 3. 语义鸿沟识别

```javascript
// ✅ 放入knowledge：私有信息
knowledge: {
  'team-structure': '团队架构：XX部门，YY负责人...',
  'internal-tools': '内部工具列表：...'
}

// ❌ 不放入knowledge：通用知识
knowledge: {
  'what-is-react': 'React是一个JavaScript库...'  // 这是通用知识
}
```

### 4. 奥卡姆剃刀原则

```javascript
// ✅ 简洁：只保留必要信息
process: `
1. 分析需求
2. 设计方案
3. 执行实施
4. 验证结果
`

// ❌ 冗余：重复和显而易见的内容
process: `
1. 首先，我们需要仔细分析用户的需求，理解他们想要什么...
2. 然后，基于分析结果，我们要设计一个详细的实施方案...
...（大量重复和冗余内容）
```

## 常见问题

### Q1: 谁应该使用role-creator？
**A:** 
- 主要：女娲角色
- 普通用户通过与女娲对话来创建角色
- 不需要直接调用此工具

### Q2: 创建角色需要多长时间？
**A:** 
- 女娲：2分钟内完成
- 包括ISSUE探索和文件生成
- 自动验证和注册

### Q3: 如何确保角色可用？
**A:** 
- 创建后自动验证
- 使用validate操作检查
- 通过discover确认可见

### Q4: 角色文件存储在哪里？
**A:** 
- 用户角色：~/.promptx/resource/role/
- 项目角色：{project}/.promptx/resource/role/
- 系统角色：PromptX安装目录

### Q5: DPML规范必须严格遵守吗？
**A:** 
- 是的，必须严格遵守
- 否则角色无法正确加载
- 使用validate操作检查合规性

### Q6: 如何修改系统内置角色？
**A:** 
- 不能直接修改系统角色
- 可以clone后修改
- 或创建新的用户角色

## 总结

role-creator是PromptX的角色创建工具，核心特点：

**规范驱动：**
- 严格遵循DPML规范
- 自动验证合规性
- 确保角色可用

**女娲专用：**
- 女娲的专属工具
- ISSUE流程集成
- 2分钟快速创建

**功能完整：**
- 创建、更新、删除
- 验证、克隆、导入导出
- 文档生成

**结构清晰：**
- Thought/Execution/Knowledge三层
- 模块化文件组织
- 引用机制

**适用场景：**
- 女娲创建新角色
- 女娲优化现有角色
- 角色管理和维护

role-creator是PromptX角色生态的核心工具，让女娲能够创造符合用户需求的AI角色。

