# filesystem - 文件系统操作工具

## 工具概述

filesystem是PromptX的文件系统操作工具，让AI能够安全、便捷地进行文件和目录的读写操作。

**核心定位：**
- AI的文件管理助手
- 安全的文件操作接口
- 跨平台的文件系统抽象
- 标准化的文件操作规范

**设计理念：**
- 安全第一：防止误操作和危险操作
- 简单易用：标准化的操作接口
- 功能完整：覆盖常见文件操作需求
- 错误友好：清晰的错误提示

## 核心功能

### 1. 文件读取（read）

**功能说明：**
读取文件内容

**参数：**
```javascript
{
  action: 'read',
  path: '/path/to/file.txt',
  encoding: 'utf-8'  // 可选，默认utf-8
}
```

**返回：**
```javascript
{
  success: true,
  content: '文件内容',
  size: 1024,        // 字节数
  encoding: 'utf-8'
}
```

**支持的编码：**
- utf-8（默认）
- utf-16
- base64（用于二进制文件）

**使用场景：**
- 读取配置文件
- 读取文本内容
- 读取代码文件
- 读取日志文件

### 2. 文件写入（write）

**功能说明：**
写入内容到文件

**参数：**
```javascript
{
  action: 'write',
  path: '/path/to/file.txt',
  content: '要写入的内容',
  encoding: 'utf-8',  // 可选
  mode: 'overwrite'   // overwrite（覆盖） | append（追加）
}
```

**返回：**
```javascript
{
  success: true,
  path: '/path/to/file.txt',
  bytesWritten: 1024
}
```

**写入模式：**
- overwrite：覆盖原文件（默认）
- append：追加到文件末尾

**使用场景：**
- 创建新文件
- 更新配置文件
- 写入日志
- 保存生成的内容

### 3. 文件删除（delete）

**功能说明：**
删除文件

**参数：**
```javascript
{
  action: 'delete',
  path: '/path/to/file.txt'
}
```

**返回：**
```javascript
{
  success: true,
  path: '/path/to/file.txt'
}
```

**安全机制：**
- 不可恢复的操作
- 删除前会确认文件存在
- 不支持删除目录（需用deletedir）

**使用场景：**
- 清理临时文件
- 删除过期文件
- 移除不需要的文件

### 4. 目录列表（list）

**功能说明：**
列出目录下的文件和子目录

**参数：**
```javascript
{
  action: 'list',
  path: '/path/to/directory',
  recursive: false,   // 可选，是否递归
  pattern: '*.txt'    // 可选，文件名过滤模式
}
```

**返回：**
```javascript
{
  success: true,
  items: [
    {
      name: 'file1.txt',
      type: 'file',
      size: 1024,
      modified: '2024-01-01T12:00:00Z'
    },
    {
      name: 'subdir',
      type: 'directory',
      modified: '2024-01-01T12:00:00Z'
    }
  ],
  total: 2
}
```

**过滤模式：**
- `*.txt` - 所有txt文件
- `file*` - 以file开头的文件
- `*.{js,ts}` - js或ts文件

**使用场景：**
- 浏览目录内容
- 查找特定文件
- 统计文件数量
- 批量操作前的扫描

### 5. 目录创建（mkdir）

**功能说明：**
创建目录

**参数：**
```javascript
{
  action: 'mkdir',
  path: '/path/to/newdir',
  recursive: true     // 可选，是否创建中间目录
}
```

**返回：**
```javascript
{
  success: true,
  path: '/path/to/newdir'
}
```

**递归模式：**
- recursive: true - 自动创建中间目录
- recursive: false - 父目录必须存在

**使用场景：**
- 创建项目结构
- 创建临时目录
- 准备输出目录

### 6. 目录删除（deletedir）

**功能说明：**
删除目录

**参数：**
```javascript
{
  action: 'deletedir',
  path: '/path/to/directory',
  recursive: false    // 可选，是否递归删除
}
```

**返回：**
```javascript
{
  success: true,
  path: '/path/to/directory'
}
```

**安全机制：**
- 默认只能删除空目录
- recursive: true - 删除目录及其所有内容
- 危险操作，需谨慎使用

**使用场景：**
- 清理临时目录
- 删除空目录
- 项目清理

### 7. 文件/目录移动（move）

**功能说明：**
移动或重命名文件/目录

**参数：**
```javascript
{
  action: 'move',
  source: '/path/to/source',
  destination: '/path/to/destination'
}
```

**返回：**
```javascript
{
  success: true,
  source: '/path/to/source',
  destination: '/path/to/destination'
}
```

**使用场景：**
- 重命名文件
- 移动文件到其他目录
- 组织文件结构

### 8. 文件/目录复制（copy）

**功能说明：**
复制文件或目录

**参数：**
```javascript
{
  action: 'copy',
  source: '/path/to/source',
  destination: '/path/to/destination',
  recursive: true     // 目录复制必需
}
```

**返回：**
```javascript
{
  success: true,
  source: '/path/to/source',
  destination: '/path/to/destination'
}
```

**使用场景：**
- 备份文件
- 复制模板文件
- 创建副本

### 9. 文件信息（stat）

**功能说明：**
获取文件或目录的详细信息

**参数：**
```javascript
{
  action: 'stat',
  path: '/path/to/file'
}
```

**返回：**
```javascript
{
  success: true,
  type: 'file',           // 'file' | 'directory'
  size: 1024,             // 字节数
  created: '2024-01-01T12:00:00Z',
  modified: '2024-01-02T12:00:00Z',
  accessed: '2024-01-03T12:00:00Z',
  permissions: 'rw-r--r--'
}
```

**使用场景：**
- 检查文件是否存在
- 获取文件大小
- 检查文件修改时间
- 验证文件类型

### 10. 文件存在性检查（exists）

**功能说明：**
检查文件或目录是否存在

**参数：**
```javascript
{
  action: 'exists',
  path: '/path/to/file'
}
```

**返回：**
```javascript
{
  success: true,
  exists: true,
  type: 'file'  // 'file' | 'directory' | null
}
```

**使用场景：**
- 操作前的检查
- 条件判断
- 避免错误

## 使用场景详解

### 场景1：配置文件管理

**需求：**
读取和更新配置文件

**流程：**
```javascript
// 1. 检查配置文件是否存在
{
  action: 'exists',
  path: '/config/app.json'
}

// 2. 读取配置
{
  action: 'read',
  path: '/config/app.json'
}

// 3. 修改后写回
{
  action: 'write',
  path: '/config/app.json',
  content: '更新后的配置',
  mode: 'overwrite'
}
```

### 场景2：日志记录

**需求：**
追加日志到日志文件

**流程：**
```javascript
// 追加模式写入
{
  action: 'write',
  path: '/logs/app.log',
  content: '[2024-01-01 12:00:00] 日志信息\n',
  mode: 'append'
}
```

### 场景3：批量文件处理

**需求：**
处理目录下所有特定类型文件

**流程：**
```javascript
// 1. 列出所有txt文件
{
  action: 'list',
  path: '/data',
  pattern: '*.txt'
}

// 2. 逐个处理文件
// 读取 → 处理 → 写入
```

### 场景4：项目初始化

**需求：**
创建项目目录结构

**流程：**
```javascript
// 1. 创建主目录
{
  action: 'mkdir',
  path: '/project',
  recursive: true
}

// 2. 创建子目录
{
  action: 'mkdir',
  path: '/project/src'
}

{
  action: 'mkdir',
  path: '/project/docs'
}

// 3. 创建初始文件
{
  action: 'write',
  path: '/project/README.md',
  content: '# Project Name\n\n...'
}
```

### 场景5：文件备份

**需求：**
备份重要文件

**流程：**
```javascript
// 复制文件
{
  action: 'copy',
  source: '/data/important.json',
  destination: '/backup/important.json.bak'
}
```

### 场景6：临时文件清理

**需求：**
清理临时文件

**流程：**
```javascript
// 1. 列出临时文件
{
  action: 'list',
  path: '/tmp',
  pattern: '*.tmp'
}

// 2. 逐个删除
{
  action: 'delete',
  path: '/tmp/file1.tmp'
}
```

## 安全机制

### 1. 路径验证

**防止路径遍历攻击：**
- 检查路径中是否包含 `..`
- 验证路径是否在允许的范围内
- 禁止访问系统敏感目录

**示例：**
```javascript
// 危险路径会被拒绝
'/etc/../../root'  // ❌
'../../../etc/passwd'  // ❌

// 安全路径
'/home/user/documents/file.txt'  // ✅
'./data/config.json'  // ✅
```

### 2. 操作权限

**权限检查：**
- 读权限：文件必须可读
- 写权限：文件必须可写
- 执行权限：目录必须可访问

**操作限制：**
- 不能删除正在使用的文件
- 不能覆盖受保护的文件
- 不能在只读目录创建文件

### 3. 文件大小限制

**防止资源耗尽：**
- 读取大小限制：默认100MB
- 写入大小限制：默认100MB
- 可配置的大小限制

**大文件处理：**
- 使用流式读取
- 分块处理
- 进度反馈

### 4. 并发控制

**防止竞态条件：**
- 文件锁机制
- 原子操作
- 事务性写入

### 5. 错误处理

**友好的错误信息：**
```javascript
{
  success: false,
  error: {
    code: 'FILE_NOT_FOUND',
    message: '文件不存在: /path/to/file',
    path: '/path/to/file'
  }
}
```

**常见错误码：**
- `FILE_NOT_FOUND` - 文件不存在
- `PERMISSION_DENIED` - 权限不足
- `PATH_INVALID` - 路径无效
- `FILE_TOO_LARGE` - 文件过大
- `DISK_FULL` - 磁盘空间不足
- `DIRECTORY_NOT_EMPTY` - 目录非空

## 最佳实践

### 1. 操作前检查

**建议流程：**
```javascript
// 先检查文件是否存在
const exists = await tool.execute({
  action: 'exists',
  path: '/path/to/file'
});

if (exists.exists) {
  // 再进行操作
  await tool.execute({
    action: 'read',
    path: '/path/to/file'
  });
}
```

### 2. 错误处理

**完整的错误处理：**
```javascript
try {
  const result = await tool.execute({
    action: 'read',
    path: '/path/to/file'
  });
  
  if (result.success) {
    // 处理成功
  } else {
    // 处理错误
    console.error(result.error.message);
  }
} catch (error) {
  // 处理异常
  console.error('操作失败', error);
}
```

### 3. 路径规范

**使用绝对路径：**
```javascript
// 推荐
'/home/user/documents/file.txt'

// 避免
'../documents/file.txt'
```

**路径拼接：**
```javascript
// 使用工具的路径拼接方法
const path = joinPath('/home/user', 'documents', 'file.txt');
// 结果: /home/user/documents/file.txt
```

### 4. 批量操作优化

**并行处理：**
```javascript
// 并行读取多个文件
const files = ['file1.txt', 'file2.txt', 'file3.txt'];

const results = await Promise.all(
  files.map(file => tool.execute({
    action: 'read',
    path: `/data/${file}`
  }))
);
```

### 5. 资源清理

**临时文件清理：**
```javascript
// 使用完临时文件后删除
await tool.execute({
  action: 'write',
  path: '/tmp/temp.txt',
  content: 'temporary data'
});

// ... 使用文件 ...

// 清理
await tool.execute({
  action: 'delete',
  path: '/tmp/temp.txt'
});
```

## 与其他工具的配合

### 与pdf-reader配合

**场景：**处理PDF文件
```javascript
// 1. 列出所有PDF文件
const pdfs = await filesystem.execute({
  action: 'list',
  path: '/documents',
  pattern: '*.pdf'
});

// 2. 逐个使用pdf-reader处理
for (const pdf of pdfs.items) {
  const content = await pdfReader.execute({
    action: 'extract',
    path: `/documents/${pdf.name}`
  });
  
  // 3. 保存提取的文本
  await filesystem.execute({
    action: 'write',
    path: `/text/${pdf.name}.txt`,
    content: content.text
  });
}
```

### 与excel-tool配合

**场景：**批量处理Excel文件
```javascript
// 1. 读取Excel文件列表
const files = await filesystem.execute({
  action: 'list',
  path: '/reports',
  pattern: '*.xlsx'
});

// 2. 处理每个Excel文件
for (const file of files.items) {
  const data = await excelTool.execute({
    action: 'read',
    path: `/reports/${file.name}`
  });
  
  // 处理数据...
}
```

### 与word-tool配合

**场景：**Word文档转换
```javascript
// 1. 读取Word文档
const content = await wordTool.execute({
  action: 'read',
  path: '/documents/report.docx'
});

// 2. 转换为Markdown并保存
await filesystem.execute({
  action: 'write',
  path: '/documents/report.md',
  content: content.markdown
});
```

## 常见问题

### Q1: 支持哪些操作系统？
**A:** 
- Windows
- macOS
- Linux
- 跨平台，自动处理路径差异

### Q2: 如何处理大文件？
**A:** 
- 使用流式读取
- 分块处理
- 设置合适的大小限制
- 考虑使用专门的大文件工具

### Q3: 文件操作是否安全？
**A:** 
- 有完善的安全机制
- 路径验证
- 权限检查
- 但仍需谨慎使用删除等危险操作

### Q4: 如何处理编码问题？
**A:** 
- 默认使用UTF-8
- 可指定其他编码
- 二进制文件使用base64
- 自动检测常见编码

### Q5: 可以操作网络文件吗？
**A:** 
- 仅支持本地文件系统
- 网络文件需先下载
- 可配合其他工具使用

### Q6: 如何提高批量操作性能？
**A:** 
- 使用并行处理
- 批量操作API
- 缓存频繁访问的文件
- 避免重复读取

## 总结

filesystem是PromptX的文件系统操作工具，核心特点：

**功能完整：**
- 读写文件
- 管理目录
- 移动复制
- 信息查询

**安全可靠：**
- 路径验证
- 权限检查
- 错误处理
- 资源限制

**易于使用：**
- 标准化接口
- 清晰的参数
- 友好的错误提示
- 丰富的返回信息

**适用场景：**
- 配置管理
- 日志记录
- 批量处理
- 项目初始化
- 文件备份

**最佳实践：**
- 操作前检查
- 完整错误处理
- 使用绝对路径
- 及时清理资源

filesystem让AI能够安全、便捷地操作文件系统，是PromptX工具生态的基础工具之一。

