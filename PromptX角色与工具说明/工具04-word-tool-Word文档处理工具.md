# word-tool - Word文档处理工具

## 工具概述

word-tool是PromptX的Word文档处理工具，让AI能够读取、编辑和创建Word文档。

**核心定位：**
- Word文档读写器
- 文档格式处理器
- 内容编辑工具
- 跨平台Word方案

**设计理念：**
- 格式完整：保持文档格式
- 编辑灵活：支持多种编辑操作
- 转换准确：支持格式转换
- 易于使用：简洁的API

## 核心功能

### 1. 读取Word文档（read）

**功能说明：**
读取Word文档的内容和结构

**参数：**
```javascript
{
  action: 'read',
  path: '/path/to/document.docx',
  format: 'text'  // 'text' | 'markdown' | 'html' | 'json'
}
```

**返回（text格式）：**
```javascript
{
  success: true,
  content: '文档的纯文本内容...',
  metadata: {
    title: '文档标题',
    author: '作者',
    created: '2024-01-01T12:00:00Z',
    modified: '2024-01-02T12:00:00Z',
    pages: 5,
    words: 1500,
    characters: 8000
  }
}
```

**返回（markdown格式）：**
```javascript
{
  success: true,
  content: '# 标题\n\n正文内容...',
  metadata: { /* 元数据 */ }
}
```

**输出格式：**
- text: 纯文本
- markdown: Markdown格式
- html: HTML格式
- json: 结构化JSON

**使用场景：**
- 提取文档内容
- 格式转换
- 内容分析
- 数据提取

### 2. 创建Word文档（create）

**功能说明：**
创建新的Word文档

**参数：**
```javascript
{
  action: 'create',
  path: '/path/to/new.docx',
  content: '文档内容',
  format: 'markdown',  // 'text' | 'markdown' | 'html'
  metadata: {
    title: '文档标题',
    author: '作者',
    subject: '主题'
  }
}
```

**返回：**
```javascript
{
  success: true,
  path: '/path/to/new.docx',
  pages: 2,
  words: 500
}
```

**输入格式：**
- text: 纯文本
- markdown: Markdown（自动转换）
- html: HTML（自动转换）

**使用场景：**
- 生成报告
- 创建文档
- 自动化输出

### 3. 修改文档（update）

**功能说明：**
修改现有Word文档

**参数：**
```javascript
{
  action: 'update',
  path: '/path/to/document.docx',
  operations: [
    {
      type: 'replace',
      search: '旧文本',
      replace: '新文本',
      all: true  // 替换所有匹配
    },
    {
      type: 'append',
      content: '\n\n附加内容'
    },
    {
      type: 'insert',
      position: 'after',  // 'after' | 'before'
      target: '某个段落',
      content: '插入的内容'
    }
  ]
}
```

**返回：**
```javascript
{
  success: true,
  operationsApplied: 3,
  modified: true
}
```

**操作类型：**
- replace: 替换文本
- append: 追加内容
- insert: 插入内容
- delete: 删除内容

**使用场景：**
- 内容更新
- 批量替换
- 模板填充

### 4. 格式设置（format）

**功能说明：**
设置文档格式

**参数：**
```javascript
{
  action: 'format',
  path: '/path/to/document.docx',
  target: '要格式化的文本',
  format: {
    bold: true,
    italic: false,
    underline: false,
    fontSize: 14,
    fontColor: '#000000',
    fontFamily: '宋体',
    alignment: 'left',  // left, center, right, justify
    highlight: '#FFFF00'
  }
}
```

**返回：**
```javascript
{
  success: true,
  formatted: true,
  matchCount: 3
}
```

**格式选项：**
- 字体：bold, italic, underline
- 大小：fontSize
- 颜色：fontColor, highlight
- 字体：fontFamily
- 对齐：alignment

**使用场景：**
- 美化文档
- 突出重点
- 统一格式

### 5. 添加图片（insertImage）

**功能说明：**
在文档中插入图片

**参数：**
```javascript
{
  action: 'insertImage',
  path: '/path/to/document.docx',
  imagePath: '/path/to/image.png',
  position: 'end',  // 'start' | 'end' | 'after:text'
  width: 400,       // 像素
  height: 300,
  caption: '图片说明'
}
```

**返回：**
```javascript
{
  success: true,
  imageInserted: true,
  position: 'page 2'
}
```

**使用场景：**
- 插入图表
- 添加截图
- 图文混排

### 6. 添加表格（insertTable）

**功能说明：**
在文档中插入表格

**参数：**
```javascript
{
  action: 'insertTable',
  path: '/path/to/document.docx',
  position: 'end',
  data: [
    ['列1', '列2', '列3'],
    ['数据1', '数据2', '数据3'],
    ['数据4', '数据5', '数据6']
  ],
  style: 'grid',  // 'grid' | 'plain' | 'modern'
  headerRow: true
}
```

**返回：**
```javascript
{
  success: true,
  tableInserted: true,
  rows: 3,
  columns: 3
}
```

**表格样式：**
- grid: 网格样式
- plain: 简单样式
- modern: 现代样式

**使用场景：**
- 数据展示
- 对比表格
- 报表生成

### 7. 添加列表（insertList）

**功能说明：**
在文档中插入列表

**参数：**
```javascript
{
  action: 'insertList',
  path: '/path/to/document.docx',
  position: 'end',
  type: 'bullet',  // 'bullet' | 'number'
  items: [
    '第一项',
    '第二项',
    {
      text: '第三项',
      subItems: ['子项1', '子项2']
    }
  ]
}
```

**返回：**
```javascript
{
  success: true,
  listInserted: true,
  itemCount: 3
}
```

**列表类型：**
- bullet: 无序列表（项目符号）
- number: 有序列表（数字编号）

**使用场景：**
- 要点列举
- 步骤说明
- 层次结构

### 8. 添加页眉页脚（setHeaderFooter）

**功能说明：**
设置文档的页眉页脚

**参数：**
```javascript
{
  action: 'setHeaderFooter',
  path: '/path/to/document.docx',
  header: {
    left: '文档标题',
    center: '',
    right: '日期: {date}'
  },
  footer: {
    left: '',
    center: '第 {page} 页 共 {pages} 页',
    right: '公司名称'
  }
}
```

**返回：**
```javascript
{
  success: true,
  headerSet: true,
  footerSet: true
}
```

**变量支持：**
- {page}: 当前页码
- {pages}: 总页数
- {date}: 当前日期
- {time}: 当前时间

**使用场景：**
- 文档标识
- 页码添加
- 版权信息

### 9. 转换格式（convert）

**功能说明：**
将Word文档转换为其他格式

**参数：**
```javascript
{
  action: 'convert',
  source: '/path/to/document.docx',
  target: '/path/to/output.pdf',
  format: 'pdf'  // 'pdf' | 'html' | 'markdown' | 'txt'
}
```

**返回：**
```javascript
{
  success: true,
  source: '/path/to/document.docx',
  target: '/path/to/output.pdf',
  format: 'pdf'
}
```

**支持的格式：**
- PDF: 便于分享
- HTML: 网页显示
- Markdown: 轻量级文本
- TXT: 纯文本

**使用场景：**
- 格式转换
- 文档分享
- 网页发布

### 10. 合并文档（merge）

**功能说明：**
合并多个Word文档

**参数：**
```javascript
{
  action: 'merge',
  sources: [
    '/path/to/doc1.docx',
    '/path/to/doc2.docx',
    '/path/to/doc3.docx'
  ],
  output: '/path/to/merged.docx',
  pageBreak: true  // 每个文档后插入分页符
}
```

**返回：**
```javascript
{
  success: true,
  output: '/path/to/merged.docx',
  sourceCount: 3,
  totalPages: 15
}
```

**使用场景：**
- 文档合并
- 报告汇总
- 章节整合

## 使用场景详解

### 场景1：自动生成报告

**需求：**
根据数据自动生成Word报告

**流程：**
```javascript
// 1. 准备数据
const reportData = {
  title: '2024年度销售报告',
  author: '销售部',
  summary: '本年度销售业绩良好...',
  salesData: [
    ['月份', '销售额', '增长率'],
    ['1月', '100万', '10%'],
    ['2月', '120万', '20%']
  ],
  chartImage: '/path/to/chart.png'
};

// 2. 创建报告（使用Markdown）
const content = `
# ${reportData.title}

## 概述
${reportData.summary}

## 销售数据
（表格将在下一步插入）

## 趋势图
（图片将在下一步插入）

## 结论
本年度销售目标达成情况良好。
`;

await wordTool.execute({
  action: 'create',
  path: '/reports/annual_report.docx',
  content: content,
  format: 'markdown',
  metadata: {
    title: reportData.title,
    author: reportData.author
  }
});

// 3. 插入表格
await wordTool.execute({
  action: 'insertTable',
  path: '/reports/annual_report.docx',
  position: 'after:销售数据',
  data: reportData.salesData,
  style: 'modern',
  headerRow: true
});

// 4. 插入图片
await wordTool.execute({
  action: 'insertImage',
  path: '/reports/annual_report.docx',
  imagePath: reportData.chartImage,
  position: 'after:趋势图',
  width: 500,
  caption: '销售趋势图'
});

// 5. 设置页眉页脚
await wordTool.execute({
  action: 'setHeaderFooter',
  path: '/reports/annual_report.docx',
  header: {
    left: reportData.title,
    right: '{date}'
  },
  footer: {
    center: '第 {page} 页'
  }
});
```

### 场景2：批量模板填充

**需求：**
基于模板批量生成个性化文档

**流程：**
```javascript
// 1. 读取人员列表
const people = [
  { name: '张三', department: '销售部', score: 95 },
  { name: '李四', department: '技术部', score: 88 }
];

// 2. 为每个人生成证书
for (const person of people) {
  // 复制模板
  await filesystem.execute({
    action: 'copy',
    source: '/templates/certificate_template.docx',
    destination: `/output/certificate_${person.name}.docx`
  });
  
  // 替换占位符
  await wordTool.execute({
    action: 'update',
    path: `/output/certificate_${person.name}.docx`,
    operations: [
      {
        type: 'replace',
        search: '{{NAME}}',
        replace: person.name
      },
      {
        type: 'replace',
        search: '{{DEPARTMENT}}',
        replace: person.department
      },
      {
        type: 'replace',
        search: '{{SCORE}}',
        replace: person.score.toString()
      }
    ]
  });
  
  // 格式化姓名（加粗）
  await wordTool.execute({
    action: 'format',
    path: `/output/certificate_${person.name}.docx`,
    target: person.name,
    format: { bold: true, fontSize: 16 }
  });
}
```

### 场景3：文档内容提取与分析

**需求：**
从Word文档提取内容进行分析

**流程：**
```javascript
// 1. 列出所有Word文档
const files = await filesystem.execute({
  action: 'list',
  path: '/documents',
  pattern: '*.docx'
});

// 2. 提取所有文档内容
const allContent = [];

for (const file of files.items) {
  const result = await wordTool.execute({
    action: 'read',
    path: `/documents/${file.name}`,
    format: 'text'
  });
  
  allContent.push({
    file: file.name,
    content: result.content,
    words: result.metadata.words
  });
}

// 3. 分析内容（例如：关键词提取）
const analysis = analyzeContent(allContent);

// 4. 生成分析报告
const report = generateAnalysisReport(analysis);

await wordTool.execute({
  action: 'create',
  path: '/reports/content_analysis.docx',
  content: report,
  format: 'markdown'
});
```

### 场景4：Word转PDF批量处理

**需求：**
批量将Word文档转换为PDF

**流程：**
```javascript
// 1. 列出所有Word文档
const files = await filesystem.execute({
  action: 'list',
  path: '/documents',
  pattern: '*.docx'
});

// 2. 批量转换
for (const file of files.items) {
  const fileName = file.name.replace('.docx', '');
  
  await wordTool.execute({
    action: 'convert',
    source: `/documents/${file.name}`,
    target: `/pdf/${fileName}.pdf`,
    format: 'pdf'
  });
  
  console.log(`已转换: ${file.name} -> ${fileName}.pdf`);
}
```

### 场景5：文档合并与章节整理

**需求：**
合并多个章节为完整文档

**流程：**
```javascript
// 1. 定义章节顺序
const chapters = [
  '/chapters/chapter1.docx',
  '/chapters/chapter2.docx',
  '/chapters/chapter3.docx'
];

// 2. 合并文档
await wordTool.execute({
  action: 'merge',
  sources: chapters,
  output: '/books/complete_book.docx',
  pageBreak: true
});

// 3. 添加封面页
await wordTool.execute({
  action: 'update',
  path: '/books/complete_book.docx',
  operations: [
    {
      type: 'insert',
      position: 'start',
      content: '# 完整书籍标题\n\n作者：XXX\n\n出版日期：2024'
    }
  ]
});

// 4. 设置页眉页脚
await wordTool.execute({
  action: 'setHeaderFooter',
  path: '/books/complete_book.docx',
  header: {
    center: '书籍标题'
  },
  footer: {
    center: '第 {page} 页'
  }
});
```

## 技术细节

### 支持的Word格式

**文件格式：**
- .docx (Word 2007+) ✅ 推荐
- .doc (Word 97-2003) ✅ 支持
- .docm (带宏) ⚠️ 部分支持
- .dot/.dotx (模板) ✅ 支持

### Markdown到Word的转换

**支持的Markdown语法：**
- 标题：# ## ### ####
- 粗体：**text**
- 斜体：*text*
- 列表：- 或 1.
- 链接：[text](url)
- 图片：![alt](url)
- 代码：`code`
- 引用：>
- 表格：| col1 | col2 |

### 性能优化

**大文档处理：**
```javascript
// 分段处理大文档
const content = await wordTool.execute({
  action: 'read',
  path: '/documents/large.docx',
  format: 'text'
});

// 分段处理
const chunks = splitIntoChunks(content.content, 1000);
for (const chunk of chunks) {
  await processChunk(chunk);
}
```

## 最佳实践

### 1. 保持格式完整性

```javascript
// 修改内容时保持格式
// ❌ 不推荐：重新创建（会丢失格式）
await wordTool.execute({
  action: 'create',
  content: newContent
});

// ✅ 推荐：使用update操作
await wordTool.execute({
  action: 'update',
  operations: [
    { type: 'replace', search: 'old', replace: 'new' }
  ]
});
```

### 2. 合理使用格式

```javascript
// 使用Markdown创建文档
const content = `
# 主标题

## 二级标题

正文内容，支持**粗体**和*斜体*。

- 列表项1
- 列表项2
`;

await wordTool.execute({
  action: 'create',
  path: '/output/doc.docx',
  content: content,
  format: 'markdown'  // 自动转换格式
});
```

### 3. 错误处理

```javascript
try {
  const result = await wordTool.execute({
    action: 'read',
    path: '/documents/file.docx'
  });
  
  if (result.success) {
    processContent(result.content);
  }
} catch (error) {
  console.error('读取失败', error);
}
```

## 常见问题

### Q1: 支持哪些Word版本？
**A:** 
- Word 2007及以上（.docx）✅
- Word 97-2003（.doc）✅
- 推荐使用.docx格式

### Q2: 格式会丢失吗？
**A:** 
- 读取会保留原格式
- 更新操作保留格式
- 创建时可指定格式
- 转换可能有格式差异

### Q3: 能处理多大的文档？
**A:** 
- 一般文档无限制
- 超大文档建议分段处理
- 取决于可用内存

### Q4: Markdown转Word的限制？
**A:** 
- 基本语法都支持
- 复杂表格可能有差异
- 建议测试验证

### Q5: 如何批量处理？
**A:** 
- 使用filesystem列出文件
- 循环处理每个文档
- 注意错误处理
- 考虑并行处理

### Q6: Word转PDF质量如何？
**A:** 
- 质量良好
- 保持原有格式
- 图片清晰度保持
- 适合分享使用

## 总结

word-tool是PromptX的Word文档处理工具，核心特点：

**功能完整：**
- 读写操作
- 格式设置
- 图片表格
- 格式转换
- 文档合并

**格式保持：**
- 保留原格式
- 支持多种格式
- Markdown转换
- 样式设置

**易于使用：**
- 简洁API
- 批量处理
- 模板支持
- 错误友好

**适用场景：**
- 报告生成
- 模板填充
- 格式转换
- 批量处理
- 文档合并

word-tool让AI能够灵活处理Word文档，是PromptX文档处理的重要工具。

