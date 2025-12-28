# tool-creator - 工具创建工具

## 工具概述

tool-creator是PromptX的工具创建工具，让鲁班能够开发和管理AI工具。

**核心定位：**
- 鲁班的专属工具
- ToolX工具生成器
- 工具结构管理器
- 工具验证工具

**设计理念：**
- 标准化：统一的工具接口
- 模块化：清晰的代码结构
- 可测试：支持dry-run
- 易维护：规范的代码组织

**重要说明：**
⚠️ 此工具主要由鲁班角色使用，普通用户通过与鲁班对话来开发工具。

## 核心功能

### 1. 创建工具（create）

**功能说明：**
创建新的ToolX工具

**参数：**
```javascript
{
  action: 'create',
  toolId: 'my-tool',  // 工具ID
  tool: {
    metadata: {
      id: 'my-tool',
      name: '我的工具',
      description: '工具描述',
      version: '1.0.0'
    },
    dependencies: {
      'axios': '^1.0.0',
      'lodash': '^4.17.21'
    },
    bridges: {
      'http:get': {
        real: `async (url, api) => {
          const axios = await api.importx('axios');
          return await axios.get(url);
        }`,
        mock: `async (url, api) => {
          return { data: 'mock data' };
        }`
      }
    },
    execute: `async (params) => {
      const { api } = this;
      
      // 使用Bridge调用外部依赖
      const result = await api.bridge.execute('http:get', 
        params.url, 
        api
      );
      
      return {
        success: true,
        data: result.data
      };
    }`,
    schema: {
      parameters: {
        type: 'object',
        properties: {
          url: {
            type: 'string',
            description: 'URL地址'
          }
        },
        required: ['url']
      }
    }
  }
}
```

**返回：**
```javascript
{
  success: true,
  toolId: 'my-tool',
  filePath: '/path/to/my-tool.tool.js',
  validated: true
}
```

**生成的文件结构：**
```javascript
// my-tool.tool.js

module.exports = {
  getDependencies() {
    return {
      'axios': '^1.0.0',
      'lodash': '^4.17.21'
    };
  },
  
  getMetadata() {
    return {
      id: 'my-tool',
      name: '我的工具',
      description: '工具描述',
      version: '1.0.0'
    };
  },
  
  getBridges() {
    return {
      'http:get': {
        real: async (url, api) => {
          const axios = await api.importx('axios');
          return await axios.get(url);
        },
        mock: async (url, api) => {
          return { data: 'mock data' };
        }
      }
    };
  },
  
  getSchema() {
    return {
      parameters: {
        type: 'object',
        properties: {
          url: {
            type: 'string',
            description: 'URL地址'
          }
        },
        required: ['url']
      }
    };
  },
  
  async execute(params) {
    const { api } = this;
    
    const result = await api.bridge.execute('http:get', 
      params.url, 
      api
    );
    
    return {
      success: true,
      data: result.data
    };
  }
};
```

**使用场景：**
- 鲁班开发新工具
- 集成第三方服务
- 扩展AI能力

### 2. 更新工具（update）

**功能说明：**
更新现有工具

**参数：**
```javascript
{
  action: 'update',
  toolId: 'my-tool',
  updates: {
    metadata: {
      version: '1.1.0',
      description: '更新后的描述'
    },
    dependencies: {
      'new-package': '^2.0.0'
    },
    execute: `async (params) => {
      // 更新后的执行逻辑
    }`
  }
}
```

**返回：**
```javascript
{
  success: true,
  toolId: 'my-tool',
  updated: true,
  validated: true
}
```

**使用场景：**
- 修复bug
- 添加功能
- 优化性能

### 3. 删除工具（delete）

**功能说明：**
删除工具文件

**参数：**
```javascript
{
  action: 'delete',
  toolId: 'my-tool',
  confirm: true
}
```

**返回：**
```javascript
{
  success: true,
  toolId: 'my-tool',
  deleted: true
}
```

**使用场景：**
- 清理过时工具
- 重新开发

### 4. 验证工具（validate）

**功能说明：**
验证工具代码正确性

**参数：**
```javascript
{
  action: 'validate',
  toolId: 'my-tool',
  checkSyntax: true,
  checkBridges: true,
  checkSchema: true
}
```

**返回：**
```javascript
{
  success: true,
  valid: true,
  errors: [],
  warnings: [
    'Bridge "http:get"的mock数据较简单'
  ],
  checks: {
    syntax: true,
    bridges: true,
    schema: true,
    dependencies: true
  }
}
```

**验证项目：**
- 语法正确性
- Bridge定义完整性
- Schema规范性
- 依赖可用性
- 接口实现完整性

**使用场景：**
- 创建后验证
- 更新后检查
- 调试工具

### 5. 测试工具（test）

**功能说明：**
执行工具的dry-run测试

**参数：**
```javascript
{
  action: 'test',
  toolId: 'my-tool',
  mode: 'dryrun',  // 'dryrun' | 'real'
  params: {
    url: 'https://api.example.com/data'
  }
}
```

**返回：**
```javascript
{
  success: true,
  result: {
    success: true,
    data: 'mock data'
  },
  bridgeTests: {
    'http:get': {
      success: true,
      usedMock: true
    }
  },
  executionTime: 50  // 毫秒
}
```

**测试模式：**
- dryrun: 使用mock数据
- real: 使用真实实现

**使用场景：**
- 开发时测试
- 验证逻辑
- 调试问题

### 6. 读取工具（read）

**功能说明：**
读取工具的完整代码

**参数：**
```javascript
{
  action: 'read',
  toolId: 'my-tool'
}
```

**返回：**
```javascript
{
  success: true,
  toolId: 'my-tool',
  code: '/* 完整的工具代码 */',
  metadata: { /* 元数据 */ },
  structure: {
    hasDependencies: true,
    hasBridges: true,
    hasSchema: true,
    hasExecute: true
  }
}
```

**使用场景：**
- 查看工具实现
- 学习参考
- 准备更新

### 7. 列出工具（list）

**功能说明：**
列出所有可用工具

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
  tools: [
    {
      toolId: 'my-tool',
      name: '我的工具',
      description: '工具描述',
      scope: 'user',
      version: '1.0.0',
      created: '2024-01-01T12:00:00Z',
      modified: '2024-01-02T12:00:00Z'
    }
  ],
  total: 15
}
```

**使用场景：**
- 查看可用工具
- 管理工具
- 选择工具

### 8. 克隆工具（clone）

**功能说明：**
复制现有工具作为新工具的基础

**参数：**
```javascript
{
  action: 'clone',
  sourceToolId: 'http-client',
  targetToolId: 'my-http-client',
  modifications: {
    metadata: {
      name: '我的HTTP客户端',
      description: '定制的HTTP工具'
    }
  }
}
```

**返回：**
```javascript
{
  success: true,
  sourceToolId: 'http-client',
  targetToolId: 'my-http-client',
  cloned: true
}
```

**使用场景：**
- 基于现有工具定制
- 快速创建相似工具
- 学习参考

### 9. 生成文档（generateDoc）

**功能说明：**
为工具生成使用文档

**参数：**
```javascript
{
  action: 'generateDoc',
  toolId: 'my-tool',
  output: '/path/to/doc/my-tool.md',
  format: 'markdown'
}
```

**返回：**
```javascript
{
  success: true,
  toolId: 'my-tool',
  outputPath: '/path/to/doc/my-tool.md'
}
```

**使用场景：**
- 生成工具文档
- 用户指南
- API参考

### 10. 安装依赖（installDeps）

**功能说明：**
安装工具所需的npm依赖

**参数：**
```javascript
{
  action: 'installDeps',
  toolId: 'my-tool',
  force: false  // 强制重新安装
}
```

**返回：**
```javascript
{
  success: true,
  toolId: 'my-tool',
  installed: ['axios@1.0.0', 'lodash@4.17.21'],
  skipped: [],
  failed: []
}
```

**使用场景：**
- 准备工具运行环境
- 更新依赖
- 修复依赖问题

## 工具接口规范

### 标准工具接口

**必需方法：**
```javascript
module.exports = {
  // 获取依赖（可选）
  getDependencies() {
    return {
      'package-name': '^version'
    };
  },
  
  // 获取元数据（必需）
  getMetadata() {
    return {
      id: 'tool-id',
      name: '工具名称',
      description: '工具描述',
      version: '1.0.0'
    };
  },
  
  // 获取参数Schema（必需）
  getSchema() {
    return {
      parameters: {
        type: 'object',
        properties: {
          param1: {
            type: 'string',
            description: '参数说明'
          }
        },
        required: ['param1']
      }
    };
  },
  
  // 执行方法（必需）
  async execute(params) {
    const { api } = this;
    
    // 工具逻辑
    
    return {
      success: true,
      data: result
    };
  },
  
  // Bridge定义（可选但推荐）
  getBridges() {
    return {
      'category:operation': {
        real: async (args, api) => {
          // 真实实现
        },
        mock: async (args, api) => {
          // Mock实现
        }
      }
    };
  }
};
```

### Bridge模式详解

**为什么需要Bridge：**
- 隔离外部依赖
- 支持dry-run测试
- 提高可测试性
- 便于mock

**Bridge命名规范：**
```
category:operation

例如：
http:get
db:query
file:read
api:call
```

**Bridge实现：**
```javascript
getBridges() {
  return {
    'http:get': {
      // 真实实现
      real: async (url, options, api) => {
        const axios = await api.importx('axios');
        return await axios.get(url, options);
      },
      
      // Mock实现
      mock: async (url, options, api) => {
        // 返回合理的mock数据
        return {
          data: { message: 'mock response' },
          status: 200,
          headers: {}
        };
      }
    }
  };
}
```

**使用Bridge：**
```javascript
async execute(params) {
  const { api } = this;
  
  // 调用Bridge
  const result = await api.bridge.execute(
    'http:get',      // Bridge名称
    params.url,      // 参数
    params.options,
    api              // API对象
  );
  
  return { success: true, data: result.data };
}
```

### API对象详解

**api.importx：智能模块加载**
```javascript
const axios = await api.importx('axios');
const _ = await api.importx('lodash');
```

**特点：**
- 自动处理ESM/CommonJS差异
- 4层降级策略
- 缓存机制
- 预装包优先

**api.bridge：Bridge执行器**
```javascript
const result = await api.bridge.execute(
  'bridge-name',
  arg1,
  arg2,
  api
);
```

**api.environment：环境变量**
```javascript
const apiKey = await api.environment.get('API_KEY');
await api.environment.set('API_KEY', 'value');
```

**api.logger：日志记录**
```javascript
api.logger.info('信息日志', { data });
api.logger.error('错误日志', error);
api.logger.debug('调试日志');
```

**api.storage：持久化存储**
```javascript
api.storage.setItem('key', value);
const value = api.storage.getItem('key');
api.storage.removeItem('key');
```

## 使用场景详解

### 场景1：鲁班创建HTTP客户端工具

**需求：**创建一个HTTP请求工具

**流程：**
```javascript
// 1. 鲁班接收需求："我要一个能发HTTP请求的工具"

// 2. 技术调研
// - 了解HTTP请求原理
// - 选择axios作为基础库
// - 设计Bridge隔离

// 3. 创建工具
await toolCreator.execute({
  action: 'create',
  toolId: 'http-client',
  tool: {
    metadata: {
      id: 'http-client',
      name: 'HTTP客户端',
      description: '发送HTTP请求',
      version: '1.0.0'
    },
    dependencies: {
      'axios': '^1.6.0'
    },
    bridges: {
      'http:get': {
        real: async (url, config, api) => {
          const axios = await api.importx('axios');
          return await axios.get(url, config);
        },
        mock: async (url, config, api) => {
          return {
            data: { message: 'mock data' },
            status: 200
          };
        }
      },
      'http:post': {
        real: async (url, data, config, api) => {
          const axios = await api.importx('axios');
          return await axios.post(url, data, config);
        },
        mock: async (url, data, config, api) => {
          return {
            data: { success: true },
            status: 200
          };
        }
      }
    },
    execute: `async (params) => {
      const { api } = this;
      const { method, url, data, headers } = params;
      
      const config = { headers };
      
      let result;
      if (method === 'GET') {
        result = await api.bridge.execute('http:get', url, config, api);
      } else if (method === 'POST') {
        result = await api.bridge.execute('http:post', url, data, config, api);
      }
      
      return {
        success: true,
        status: result.status,
        data: result.data
      };
    }`,
    schema: {
      parameters: {
        type: 'object',
        properties: {
          method: {
            type: 'string',
            enum: ['GET', 'POST'],
            description: 'HTTP方法'
          },
          url: {
            type: 'string',
            description: 'URL地址'
          },
          data: {
            type: 'object',
            description: '请求数据（POST）'
          },
          headers: {
            type: 'object',
            description: '请求头'
          }
        },
        required: ['method', 'url']
      }
    }
  }
});

// 4. Dry-run测试
await toolCreator.execute({
  action: 'test',
  toolId: 'http-client',
  mode: 'dryrun',
  params: {
    method: 'GET',
    url: 'https://api.example.com/data'
  }
});

// 5. 验证
await toolCreator.execute({
  action: 'validate',
  toolId: 'http-client'
});

// 6. 通知用户
console.log('工具创建成功！通过discover可以查看和使用。');
```

### 场景2：鲁班优化现有工具

**需求：**工具出现bug需要修复

**流程：**
```javascript
// 1. 读取现有工具
const tool = await toolCreator.execute({
  action: 'read',
  toolId: 'http-client'
});

// 2. 分析问题，修改代码

// 3. 更新工具
await toolCreator.execute({
  action: 'update',
  toolId: 'http-client',
  updates: {
    execute: `async (params) => {
      const { api } = this;
      
      // 添加错误处理
      try {
        // ... 原有逻辑 ...
      } catch (error) {
        api.logger.error('HTTP请求失败', error);
        return {
          success: false,
          error: error.message
        };
      }
    }`
  }
});

// 4. 测试
await toolCreator.execute({
  action: 'test',
  toolId: 'http-client',
  mode: 'dryrun'
});
```

### 场景3：基于模板快速创建

**流程：**
```javascript
// 1. 克隆现有工具
await toolCreator.execute({
  action: 'clone',
  sourceToolId: 'http-client',
  targetToolId: 'wechat-client'
});

// 2. 定制修改
await toolCreator.execute({
  action: 'update',
  toolId: 'wechat-client',
  updates: {
    metadata: {
      name: '企业微信客户端',
      description: '发送企业微信消息'
    },
    // 修改具体实现...
  }
});
```

## 最佳实践

### 1. 遵循Bridge模式

```javascript
// ✅ 正确：所有外部调用通过Bridge
getBridges() {
  return {
    'db:query': {
      real: async (sql, api) => {
        const mysql = await api.importx('mysql2/promise');
        return await mysql.query(sql);
      },
      mock: async (sql, api) => {
        return [{ id: 1, name: 'test' }];
      }
    }
  };
}

async execute(params) {
  const result = await api.bridge.execute('db:query', params.sql, api);
  return { success: true, data: result };
}

// ❌ 错误：直接在execute中调用外部模块
async execute(params) {
  const mysql = require('mysql2/promise');  // ❌
  const result = await mysql.query(params.sql);
  return result;
}
```

### 2. 提供完整的Mock数据

```javascript
// ✅ 推荐：Mock数据结构与真实一致
mock: async (url, api) => {
  return {
    data: {
      id: 1,
      name: 'test',
      items: [
        { id: 1, value: 'item1' },
        { id: 2, value: 'item2' }
      ]
    },
    status: 200,
    headers: { 'content-type': 'application/json' }
  };
}

// ❌ 不推荐：Mock数据过于简单
mock: async (url, api) => {
  return { data: 'mock' };
}
```

### 3. 完善的错误处理

```javascript
async execute(params) {
  const { api } = this;
  
  try {
    // 参数验证
    if (!params.url) {
      return {
        success: false,
        error: 'URL is required'
      };
    }
    
    // 执行逻辑
    const result = await api.bridge.execute('http:get', params.url, api);
    
    return {
      success: true,
      data: result.data
    };
    
  } catch (error) {
    api.logger.error('执行失败', error);
    return {
      success: false,
      error: error.message
    };
  }
}
```

### 4. 清晰的Schema定义

```javascript
getSchema() {
  return {
    parameters: {
      type: 'object',
      properties: {
        url: {
          type: 'string',
          description: 'API URL地址',
          pattern: '^https?://'  // 正则验证
        },
        method: {
          type: 'string',
          enum: ['GET', 'POST', 'PUT', 'DELETE'],
          default: 'GET'
        },
        timeout: {
          type: 'number',
          description: '超时时间（毫秒）',
          minimum: 0,
          maximum: 60000,
          default: 5000
        }
      },
      required: ['url']
    }
  };
}
```

## 常见问题

### Q1: 谁应该使用tool-creator？
**A:** 
- 主要：鲁班角色
- 普通用户通过与鲁班对话来开发工具
- 不需要直接调用

### Q2: 为什么要用Bridge模式？
**A:** 
- 隔离外部依赖
- 支持dry-run测试
- 提高可测试性
- 便于调试

### Q3: 工具开发需要多长时间？
**A:** 
- 简单工具：3-5分钟
- 中等工具：10-15分钟
- 复杂工具：20-30分钟

### Q4: 如何测试工具？
**A:** 
- 使用dry-run模式
- 验证Bridge的mock
- 检查返回数据结构
- 真实环境测试

### Q5: 工具文件存储在哪里？
**A:** 
- 用户工具：~/.promptx/resource/tool/
- 项目工具：{project}/.promptx/resource/tool/
- 系统工具：PromptX安装目录

### Q6: 如何发布工具？
**A:** 
- 使用export导出
- 分享给他人
- 或提交到PromptX社区

## 总结

tool-creator是PromptX的工具创建工具，核心特点：

**鲁班专用：**
- 鲁班的专属工具
- 3-5分钟快速交付
- Bridge模式规范

**功能完整：**
- 创建、更新、删除
- 验证、测试
- 克隆、导入导出
- 文档生成

**标准化：**
- 统一的工具接口
- Bridge模式隔离
- Schema规范
- API标准

**适用场景：**
- 鲁班开发新工具
- 集成第三方服务
- 扩展AI能力
- 工具维护

tool-creator是PromptX工具生态的核心，让鲁班能够快速开发高质量的AI工具。

