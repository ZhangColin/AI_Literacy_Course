# pdf-reader - PDF文档处理工具

## 工具概述

pdf-reader是PromptX的PDF文档处理工具，让AI能够读取、解析和提取PDF文件的内容。

**核心定位：**
- PDF内容提取器
- 文档结构解析器
- 文本和元数据读取器
- 跨平台PDF处理方案

**设计理念：**
- 准确提取：保持文本结构和格式
- 智能解析：识别文档结构
- 高效处理：支持大文件和批量处理
- 易于使用：简单的API接口

## 核心功能

### 1. 文本提取（extract）

**功能说明：**
提取PDF文档的文本内容

**参数：**
```javascript
{
  action: 'extract',
  path: '/path/to/document.pdf',
  pages: 'all',           // 'all' | [1, 2, 3] | '1-5'
  preserveLayout: true    // 保持布局
}
```

**返回：**
```javascript
{
  success: true,
  text: '提取的文本内容',
  pages: 10,
  metadata: {
    title: '文档标题',
    author: '作者',
    subject: '主题',
    keywords: '关键词',
    creator: 'PDF创建工具',
    producer: 'PDF生成器',
    creationDate: '2024-01-01',
    modificationDate: '2024-01-02'
  }
}
```

**页面选择方式：**
- `'all'` - 所有页面
- `[1, 2, 3]` - 指定页面数组
- `'1-5'` - 页面范围
- `'1,3,5-7'` - 混合格式

**使用场景：**
- 提取文档内容
- 文本分析
- 内容搜索
- 文档转换

### 2. 元数据读取（metadata）

**功能说明：**
读取PDF文档的元数据信息

**参数：**
```javascript
{
  action: 'metadata',
  path: '/path/to/document.pdf'
}
```

**返回：**
```javascript
{
  success: true,
  metadata: {
    title: '文档标题',
    author: '作者',
    subject: '主题',
    keywords: '关键词',
    creator: 'Microsoft Word',
    producer: 'Adobe PDF Library',
    creationDate: '2024-01-01T12:00:00Z',
    modificationDate: '2024-01-02T12:00:00Z',
    pages: 10,
    fileSize: 1048576,
    pdfVersion: '1.7',
    encrypted: false
  }
}
```

**元数据字段：**
- title: 标题
- author: 作者
- subject: 主题
- keywords: 关键词
- creator: 创建软件
- producer: PDF生成器
- creationDate: 创建时间
- modificationDate: 修改时间
- pages: 总页数
- fileSize: 文件大小（字节）
- pdfVersion: PDF版本
- encrypted: 是否加密

**使用场景：**
- 文档信息查询
- 文件分类
- 批量处理前的检查

### 3. 页面信息（pageinfo）

**功能说明：**
获取PDF文档的页面详细信息

**参数：**
```javascript
{
  action: 'pageinfo',
  path: '/path/to/document.pdf',
  page: 1  // 页码，从1开始
}
```

**返回：**
```javascript
{
  success: true,
  page: 1,
  width: 612,      // 点（pt）
  height: 792,
  rotation: 0,     // 旋转角度
  textLength: 1500 // 文本长度
}
```

**使用场景：**
- 检查页面尺寸
- 判断页面方向
- 评估内容量

### 4. 表格提取（extractTable）

**功能说明：**
提取PDF中的表格数据

**参数：**
```javascript
{
  action: 'extractTable',
  path: '/path/to/document.pdf',
  pages: [1, 2],
  format: 'json'   // 'json' | 'csv'
}
```

**返回（JSON格式）：**
```javascript
{
  success: true,
  tables: [
    {
      page: 1,
      data: [
        ['Header1', 'Header2', 'Header3'],
        ['Row1Col1', 'Row1Col2', 'Row1Col3'],
        ['Row2Col1', 'Row2Col2', 'Row2Col3']
      ],
      rows: 3,
      columns: 3
    }
  ]
}
```

**使用场景：**
- 提取报表数据
- 数据分析
- 表格转换

### 5. 图片提取（extractImages）

**功能说明：**
提取PDF中的图片

**参数：**
```javascript
{
  action: 'extractImages',
  path: '/path/to/document.pdf',
  pages: 'all',
  outputDir: '/path/to/output',
  format: 'png',   // 'png' | 'jpg'
  minSize: 100     // 最小尺寸（像素）
}
```

**返回：**
```javascript
{
  success: true,
  images: [
    {
      page: 1,
      index: 0,
      path: '/path/to/output/page1_image0.png',
      width: 800,
      height: 600,
      format: 'png',
      size: 204800
    }
  ],
  total: 5
}
```

**使用场景：**
- 提取文档中的图片
- 图片分析
- 内容归档

### 6. 文档搜索（search）

**功能说明：**
在PDF文档中搜索文本

**参数：**
```javascript
{
  action: 'search',
  path: '/path/to/document.pdf',
  query: '搜索关键词',
  caseSensitive: false,  // 区分大小写
  wholeWord: false       // 全词匹配
}
```

**返回：**
```javascript
{
  success: true,
  results: [
    {
      page: 1,
      text: '...包含关键词的上下文...',
      position: {
        x: 100,
        y: 200
      }
    }
  ],
  total: 10
}
```

**使用场景：**
- 关键词定位
- 内容检索
- 文档分析

### 7. 页面渲染（render）

**功能说明：**
将PDF页面渲染为图片

**参数：**
```javascript
{
  action: 'render',
  path: '/path/to/document.pdf',
  page: 1,
  outputPath: '/path/to/output/page1.png',
  scale: 2.0,      // 缩放比例
  format: 'png'    // 'png' | 'jpg'
}
```

**返回：**
```javascript
{
  success: true,
  outputPath: '/path/to/output/page1.png',
  width: 1224,
  height: 1584,
  format: 'png'
}
```

**使用场景：**
- 生成预览图
- PDF转图片
- 内容展示

### 8. 文档合并（merge）

**功能说明：**
合并多个PDF文档

**参数：**
```javascript
{
  action: 'merge',
  sources: [
    '/path/to/doc1.pdf',
    '/path/to/doc2.pdf',
    '/path/to/doc3.pdf'
  ],
  output: '/path/to/merged.pdf'
}
```

**返回：**
```javascript
{
  success: true,
  output: '/path/to/merged.pdf',
  pages: 30,
  size: 2097152
}
```

**使用场景：**
- 合并多个PDF文件
- 文档整理
- 报告汇总

### 9. 页面分割（split）

**功能说明：**
将PDF文档分割为多个文件

**参数：**
```javascript
{
  action: 'split',
  path: '/path/to/document.pdf',
  mode: 'range',   // 'range' | 'size' | 'pages'
  ranges: [
    '1-5',
    '6-10'
  ],
  outputDir: '/path/to/output',
  namePattern: 'part_{index}.pdf'
}
```

**返回：**
```javascript
{
  success: true,
  files: [
    '/path/to/output/part_1.pdf',
    '/path/to/output/part_2.pdf'
  ]
}
```

**分割模式：**
- range: 按页码范围
- size: 按文件大小
- pages: 每N页一个文件

**使用场景：**
- 文档分割
- 提取特定页面
- 批量处理

### 10. 文档信息汇总（info）

**功能说明：**
获取PDF文档的完整信息

**参数：**
```javascript
{
  action: 'info',
  path: '/path/to/document.pdf'
}
```

**返回：**
```javascript
{
  success: true,
  metadata: { /* 元数据 */ },
  pages: 10,
  fileSize: 1048576,
  encrypted: false,
  hasImages: true,
  hasTables: true,
  hasBookmarks: true,
  pagesSummary: [
    { page: 1, textLength: 1500, imageCount: 2 },
    { page: 2, textLength: 1200, imageCount: 1 }
  ]
}
```

**使用场景：**
- 文档概览
- 处理前评估
- 文档分类

## 使用场景详解

### 场景1：文档内容提取与分析

**需求：**
提取PDF文档内容并进行文本分析

**流程：**
```javascript
// 1. 先获取文档信息
const info = await pdfReader.execute({
  action: 'info',
  path: '/documents/report.pdf'
});

console.log(`文档有 ${info.pages} 页`);

// 2. 提取文本内容
const content = await pdfReader.execute({
  action: 'extract',
  path: '/documents/report.pdf',
  pages: 'all',
  preserveLayout: true
});

// 3. 分析文本
const keywords = analyzeText(content.text);

// 4. 保存提取的文本
await filesystem.execute({
  action: 'write',
  path: '/output/report.txt',
  content: content.text
});
```

### 场景2：批量PDF处理

**需求：**
批量处理目录中的所有PDF文件

**流程：**
```javascript
// 1. 列出所有PDF文件
const files = await filesystem.execute({
  action: 'list',
  path: '/documents',
  pattern: '*.pdf'
});

// 2. 逐个处理
for (const file of files.items) {
  const path = `/documents/${file.name}`;
  
  // 提取元数据
  const metadata = await pdfReader.execute({
    action: 'metadata',
    path: path
  });
  
  // 提取文本
  const content = await pdfReader.execute({
    action: 'extract',
    path: path
  });
  
  // 保存结果
  await filesystem.execute({
    action: 'write',
    path: `/output/${file.name}.txt`,
    content: JSON.stringify({
      metadata: metadata.metadata,
      text: content.text
    })
  });
}
```

### 场景3：PDF表格数据提取

**需求：**
提取PDF中的表格数据并转换为Excel

**流程：**
```javascript
// 1. 提取表格
const tables = await pdfReader.execute({
  action: 'extractTable',
  path: '/reports/data.pdf',
  pages: [1, 2, 3],
  format: 'json'
});

// 2. 转换为Excel格式
const excelData = convertTablesToExcel(tables.tables);

// 3. 保存为Excel文件
await excelTool.execute({
  action: 'write',
  path: '/output/data.xlsx',
  data: excelData
});
```

### 场景4：PDF文档搜索

**需求：**
在多个PDF中搜索特定关键词

**流程：**
```javascript
const keyword = 'AI技术';
const results = [];

// 1. 列出所有PDF
const files = await filesystem.execute({
  action: 'list',
  path: '/documents',
  pattern: '*.pdf'
});

// 2. 逐个搜索
for (const file of files.items) {
  const searchResult = await pdfReader.execute({
    action: 'search',
    path: `/documents/${file.name}`,
    query: keyword
  });
  
  if (searchResult.total > 0) {
    results.push({
      file: file.name,
      matches: searchResult.results
    });
  }
}

// 3. 生成搜索报告
const report = generateSearchReport(results);
await filesystem.execute({
  action: 'write',
  path: '/output/search_report.txt',
  content: report
});
```

### 场景5：PDF转图片

**需求：**
将PDF的每一页转换为图片

**流程：**
```javascript
// 1. 获取页数
const info = await pdfReader.execute({
  action: 'metadata',
  path: '/documents/presentation.pdf'
});

// 2. 创建输出目录
await filesystem.execute({
  action: 'mkdir',
  path: '/output/images',
  recursive: true
});

// 3. 逐页渲染
for (let i = 1; i <= info.metadata.pages; i++) {
  await pdfReader.execute({
    action: 'render',
    path: '/documents/presentation.pdf',
    page: i,
    outputPath: `/output/images/page_${i}.png`,
    scale: 2.0,
    format: 'png'
  });
}
```

### 场景6：PDF文档合并

**需求：**
合并多个PDF报告为一个文件

**流程：**
```javascript
// 1. 收集要合并的PDF文件
const sources = [
  '/reports/jan.pdf',
  '/reports/feb.pdf',
  '/reports/mar.pdf'
];

// 2. 合并
const result = await pdfReader.execute({
  action: 'merge',
  sources: sources,
  output: '/reports/q1_summary.pdf'
});

console.log(`合并完成，共 ${result.pages} 页`);
```

### 场景7：PDF页面提取

**需求：**
从大PDF中提取特定页面

**流程：**
```javascript
// 1. 分割PDF，提取第10-20页
await pdfReader.execute({
  action: 'split',
  path: '/documents/large_doc.pdf',
  mode: 'range',
  ranges: ['10-20'],
  outputDir: '/output',
  namePattern: 'extract.pdf'
});
```

## 技术细节

### PDF解析技术

**使用的库：**
- pdf-parse: 文本提取
- pdf-lib: PDF操作
- pdfjs-dist: 页面渲染
- tabula: 表格提取

**解析流程：**
1. 读取PDF文件
2. 解析PDF结构
3. 提取内容对象
4. 转换为文本/数据
5. 返回结果

### 布局保持

**preserveLayout选项：**
- true: 保持原始布局（包括空格、换行）
- false: 简化文本（去除多余空格）

**实现原理：**
- 分析文本位置坐标
- 计算字符间距
- 推断段落和换行
- 重建文本结构

### 表格识别

**识别算法：**
1. 检测表格边界
2. 识别单元格
3. 提取单元格内容
4. 构建表格数据结构

**局限性：**
- 复杂表格识别率较低
- 跨页表格需特殊处理
- 无边框表格识别困难

### 图片提取

**提取流程：**
1. 遍历PDF对象
2. 识别图片对象
3. 解码图片数据
4. 转换为标准格式
5. 保存到文件

**支持的格式：**
- JPEG
- PNG
- TIFF
- 其他常见格式

## 性能优化

### 1. 大文件处理

**策略：**
- 分页处理而非一次性加载
- 流式读取
- 内存管理
- 进度反馈

**示例：**
```javascript
// 分批处理大PDF
const totalPages = 1000;
const batchSize = 10;

for (let i = 1; i <= totalPages; i += batchSize) {
  const content = await pdfReader.execute({
    action: 'extract',
    path: '/documents/large.pdf',
    pages: `${i}-${Math.min(i + batchSize - 1, totalPages)}`
  });
  
  // 处理这一批页面
  processBatch(content);
}
```

### 2. 批量处理优化

**并行处理：**
```javascript
// 并行处理多个PDF
const files = ['doc1.pdf', 'doc2.pdf', 'doc3.pdf'];

const results = await Promise.all(
  files.map(file => 
    pdfReader.execute({
      action: 'extract',
      path: `/documents/${file}`
    })
  )
);
```

### 3. 缓存机制

**缓存策略：**
- 缓存文档元数据
- 缓存已提取的页面
- 缓存渲染结果
- 设置合理的过期时间

## 错误处理

### 常见错误

**1. 文件不存在：**
```javascript
{
  success: false,
  error: {
    code: 'FILE_NOT_FOUND',
    message: 'PDF文件不存在'
  }
}
```

**2. 文件损坏：**
```javascript
{
  success: false,
  error: {
    code: 'INVALID_PDF',
    message: 'PDF文件损坏或格式不正确'
  }
}
```

**3. 加密PDF：**
```javascript
{
  success: false,
  error: {
    code: 'ENCRYPTED_PDF',
    message: 'PDF文件已加密，需要密码'
  }
}
```

**4. 权限问题：**
```javascript
{
  success: false,
  error: {
    code: 'PERMISSION_DENIED',
    message: '没有读取PDF文件的权限'
  }
}
```

### 错误处理最佳实践

```javascript
try {
  const result = await pdfReader.execute({
    action: 'extract',
    path: '/documents/file.pdf'
  });
  
  if (result.success) {
    // 处理成功
    processContent(result.text);
  } else {
    // 处理错误
    switch (result.error.code) {
      case 'FILE_NOT_FOUND':
        console.log('文件不存在');
        break;
      case 'ENCRYPTED_PDF':
        console.log('需要密码');
        break;
      default:
        console.log('未知错误');
    }
  }
} catch (error) {
  console.error('操作异常', error);
}
```

## 最佳实践

### 1. 选择合适的操作

```javascript
// 只需要元数据
const metadata = await pdfReader.execute({
  action: 'metadata',  // 而非 'extract'
  path: '/documents/file.pdf'
});

// 只需要特定页面
const content = await pdfReader.execute({
  action: 'extract',
  path: '/documents/file.pdf',
  pages: [1, 2, 3]  // 而非 'all'
});
```

### 2. 合理使用preserveLayout

```javascript
// 需要精确布局（如表格、代码）
const structured = await pdfReader.execute({
  action: 'extract',
  path: '/documents/table.pdf',
  preserveLayout: true
});

// 普通文本提取
const simple = await pdfReader.execute({
  action: 'extract',
  path: '/documents/article.pdf',
  preserveLayout: false
});
```

### 3. 批量处理的错误处理

```javascript
const files = ['doc1.pdf', 'doc2.pdf', 'doc3.pdf'];
const results = [];

for (const file of files) {
  try {
    const result = await pdfReader.execute({
      action: 'extract',
      path: `/documents/${file}`
    });
    results.push({ file, success: true, data: result });
  } catch (error) {
    results.push({ file, success: false, error });
    // 继续处理下一个文件
  }
}
```

## 常见问题

### Q1: 支持哪些PDF版本？
**A:** 
- 支持PDF 1.0 到 1.7
- 支持大部分PDF 2.0特性
- 不支持某些特殊加密方式

### Q2: 能处理扫描版PDF吗？
**A:** 
- 扫描版PDF是图片，无法直接提取文字
- 需要先OCR识别
- 可以提取图片后再OCR

### Q3: 表格提取准确度如何？
**A:** 
- 规则表格：准确度高
- 复杂表格：可能需要人工校验
- 无边框表格：识别困难

### Q4: 如何处理加密PDF？
**A:** 
- 需要提供密码
- 某些加密方式可能不支持
- 建议先解密再处理

### Q5: 性能如何优化？
**A:** 
- 按需提取（不要一次性提取所有）
- 并行处理多个文件
- 使用缓存
- 分批处理大文件

### Q6: 能修改PDF吗？
**A:** 
- 当前主要是读取功能
- 简单的合并、分割支持
- 复杂的编辑需要专门工具

## 总结

pdf-reader是PromptX的PDF文档处理工具，核心特点：

**功能丰富：**
- 文本提取
- 表格识别
- 图片提取
- 文档合并分割
- 页面渲染

**智能解析：**
- 保持文档结构
- 识别表格
- 提取元数据
- 搜索功能

**高效处理：**
- 支持大文件
- 批量处理
- 并行优化
- 缓存机制

**适用场景：**
- 文档内容提取
- 数据分析
- 文档转换
- 批量处理
- 内容搜索

pdf-reader让AI能够理解和处理PDF文档，是PromptX处理文档的核心工具之一。

