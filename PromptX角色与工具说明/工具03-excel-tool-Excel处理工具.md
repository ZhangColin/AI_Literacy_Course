# excel-tool - Excel处理工具

## 工具概述

excel-tool是PromptX的Excel处理工具，让AI能够读取、编辑和创建Excel文件。

**核心定位：**
- Excel文件读写器
- 数据处理工具
- 表格操作助手
- 跨平台Excel方案

**设计理念：**
- 功能完整：支持常见Excel操作
- 数据准确：保持数据完整性
- 格式兼容：支持多种Excel格式
- 易于使用：简洁的API设计

## 核心功能

### 1. 读取Excel（read）

**功能说明：**
读取Excel文件内容

**参数：**
```javascript
{
  action: 'read',
  path: '/path/to/file.xlsx',
  sheet: 'Sheet1',      // 工作表名称，可选
  range: 'A1:C10',      // 读取范围，可选
  headers: true         // 第一行是否为表头
}
```

**返回：**
```javascript
{
  success: true,
  data: [
    ['Name', 'Age', 'City'],
    ['Alice', 25, 'Beijing'],
    ['Bob', 30, 'Shanghai']
  ],
  sheets: ['Sheet1', 'Sheet2'],  // 所有工作表名称
  rowCount: 3,
  columnCount: 3
}
```

**使用场景：**
- 读取数据表
- 数据分析
- 数据导入

### 2. 写入Excel（write）

**功能说明：**
写入数据到Excel文件

**参数：**
```javascript
{
  action: 'write',
  path: '/path/to/output.xlsx',
  data: [
    ['Name', 'Age', 'City'],
    ['Alice', 25, 'Beijing'],
    ['Bob', 30, 'Shanghai']
  ],
  sheet: 'Sheet1',      // 工作表名称
  startCell: 'A1',      // 起始单元格
  overwrite: true       // 是否覆盖已存在的文件
}
```

**返回：**
```javascript
{
  success: true,
  path: '/path/to/output.xlsx',
  rowsWritten: 3,
  columnsWritten: 3
}
```

**使用场景：**
- 创建Excel文件
- 导出数据
- 生成报表

### 3. 修改单元格（updateCell）

**功能说明：**
修改指定单元格的值

**参数：**
```javascript
{
  action: 'updateCell',
  path: '/path/to/file.xlsx',
  sheet: 'Sheet1',
  cell: 'B2',           // 单元格位置
  value: 'New Value'
}
```

**返回：**
```javascript
{
  success: true,
  cell: 'B2',
  oldValue: 'Old Value',
  newValue: 'New Value'
}
```

**使用场景：**
- 更新特定数据
- 修正错误
- 动态更新

### 4. 添加工作表（addSheet）

**功能说明：**
添加新的工作表

**参数：**
```javascript
{
  action: 'addSheet',
  path: '/path/to/file.xlsx',
  sheetName: 'NewSheet',
  data: [
    ['Header1', 'Header2'],
    ['Value1', 'Value2']
  ]
}
```

**返回：**
```javascript
{
  success: true,
  sheetName: 'NewSheet',
  sheets: ['Sheet1', 'Sheet2', 'NewSheet']
}
```

**使用场景：**
- 添加新的数据表
- 分类存储数据
- 多表格报告

### 5. 删除工作表（deleteSheet）

**功能说明：**
删除指定工作表

**参数：**
```javascript
{
  action: 'deleteSheet',
  path: '/path/to/file.xlsx',
  sheetName: 'Sheet2'
}
```

**返回：**
```javascript
{
  success: true,
  deletedSheet: 'Sheet2',
  remainingSheets: ['Sheet1', 'Sheet3']
}
```

**使用场景：**
- 清理不需要的工作表
- 文件整理

### 6. 格式化单元格（format）

**功能说明：**
设置单元格格式

**参数：**
```javascript
{
  action: 'format',
  path: '/path/to/file.xlsx',
  sheet: 'Sheet1',
  range: 'A1:C1',
  format: {
    bold: true,
    fontSize: 14,
    fontColor: '#FF0000',
    bgColor: '#FFFF00',
    align: 'center',
    numberFormat: '#,##0.00'
  }
}
```

**返回：**
```javascript
{
  success: true,
  range: 'A1:C1',
  formatApplied: true
}
```

**格式选项：**
- bold: 粗体
- italic: 斜体
- underline: 下划线
- fontSize: 字号
- fontColor: 字体颜色
- bgColor: 背景色
- align: 对齐方式（left/center/right）
- numberFormat: 数字格式

**使用场景：**
- 美化报表
- 突出重要数据
- 格式化输出

### 7. 公式计算（formula）

**功能说明：**
设置和计算单元格公式

**参数：**
```javascript
{
  action: 'formula',
  path: '/path/to/file.xlsx',
  sheet: 'Sheet1',
  cell: 'D2',
  formula: '=SUM(A2:C2)'
}
```

**返回：**
```javascript
{
  success: true,
  cell: 'D2',
  formula: '=SUM(A2:C2)',
  value: 150  // 计算结果
}
```

**支持的公式：**
- SUM: 求和
- AVERAGE: 平均值
- COUNT: 计数
- MAX/MIN: 最大最小值
- IF: 条件判断
- VLOOKUP: 查找
- 等等...

**使用场景：**
- 自动计算
- 数据统计
- 复杂运算

### 8. 数据筛选（filter）

**功能说明：**
筛选符合条件的数据

**参数：**
```javascript
{
  action: 'filter',
  path: '/path/to/file.xlsx',
  sheet: 'Sheet1',
  column: 'Age',
  condition: {
    operator: '>',
    value: 25
  }
}
```

**返回：**
```javascript
{
  success: true,
  filteredData: [
    ['Name', 'Age', 'City'],
    ['Bob', 30, 'Shanghai'],
    ['Charlie', 28, 'Guangzhou']
  ],
  matchCount: 2
}
```

**支持的操作符：**
- `>`, `<`, `>=`, `<=`, `=`, `!=`
- contains: 包含
- startsWith: 以...开始
- endsWith: 以...结束

**使用场景：**
- 数据查询
- 条件筛选
- 数据分析

### 9. 数据排序（sort）

**功能说明：**
对数据进行排序

**参数：**
```javascript
{
  action: 'sort',
  path: '/path/to/file.xlsx',
  sheet: 'Sheet1',
  column: 'Age',
  order: 'asc',         // 'asc' | 'desc'
  range: 'A1:C10'
}
```

**返回：**
```javascript
{
  success: true,
  sortedData: [
    ['Name', 'Age', 'City'],
    ['Alice', 25, 'Beijing'],
    ['Charlie', 28, 'Guangzhou'],
    ['Bob', 30, 'Shanghai']
  ]
}
```

**使用场景：**
- 数据排序
- 排名统计
- 数据整理

### 10. 图表创建（createChart）

**功能说明：**
在Excel中创建图表

**参数：**
```javascript
{
  action: 'createChart',
  path: '/path/to/file.xlsx',
  sheet: 'Sheet1',
  type: 'bar',          // bar, line, pie, scatter
  dataRange: 'A1:B10',
  title: '销售数据',
  position: 'D2'        // 图表位置
}
```

**返回：**
```javascript
{
  success: true,
  chartId: 'chart1',
  type: 'bar',
  position: 'D2'
}
```

**支持的图表类型：**
- bar: 柱状图
- line: 折线图
- pie: 饼图
- scatter: 散点图

**使用场景：**
- 数据可视化
- 报表生成
- 趋势分析

## 使用场景详解

### 场景1：数据报表生成

**需求：**
从数据库读取数据生成Excel报表

**流程：**
```javascript
// 1. 获取数据（假设从数据库）
const salesData = await database.query('SELECT * FROM sales');

// 2. 转换为Excel格式
const excelData = [
  ['日期', '产品', '销量', '金额'],
  ...salesData.map(row => [
    row.date,
    row.product,
    row.quantity,
    row.amount
  ])
];

// 3. 创建Excel文件
await excelTool.execute({
  action: 'write',
  path: '/reports/sales_report.xlsx',
  data: excelData,
  sheet: '销售数据'
});

// 4. 格式化表头
await excelTool.execute({
  action: 'format',
  path: '/reports/sales_report.xlsx',
  sheet: '销售数据',
  range: 'A1:D1',
  format: {
    bold: true,
    bgColor: '#4472C4',
    fontColor: '#FFFFFF'
  }
});

// 5. 添加求和公式
const lastRow = excelData.length;
await excelTool.execute({
  action: 'formula',
  path: '/reports/sales_report.xlsx',
  sheet: '销售数据',
  cell: `D${lastRow + 1}`,
  formula: `=SUM(D2:D${lastRow})`
});

// 6. 创建图表
await excelTool.execute({
  action: 'createChart',
  path: '/reports/sales_report.xlsx',
  sheet: '销售数据',
  type: 'bar',
  dataRange: `A1:D${lastRow}`,
  title: '销售趋势',
  position: 'F2'
});
```

### 场景2：Excel数据导入处理

**需求：**
读取Excel文件并导入数据库

**流程：**
```javascript
// 1. 读取Excel
const result = await excelTool.execute({
  action: 'read',
  path: '/uploads/data.xlsx',
  sheet: 'Sheet1',
  headers: true
});

// 2. 数据验证
const validData = [];
const errors = [];

for (let i = 1; i < result.data.length; i++) {
  const row = result.data[i];
  
  // 验证数据
  if (validateRow(row)) {
    validData.push(row);
  } else {
    errors.push({ row: i, data: row });
  }
}

// 3. 导入数据库
for (const row of validData) {
  await database.insert('users', {
    name: row[0],
    age: row[1],
    city: row[2]
  });
}

// 4. 如果有错误，生成错误报告
if (errors.length > 0) {
  await excelTool.execute({
    action: 'write',
    path: '/reports/import_errors.xlsx',
    data: [
      ['行号', '姓名', '年龄', '城市', '错误原因'],
      ...errors.map(e => [e.row, ...e.data, '数据格式错误'])
    ]
  });
}
```

### 场景3：批量数据处理

**需求：**
批量处理多个Excel文件

**流程：**
```javascript
// 1. 列出所有Excel文件
const files = await filesystem.execute({
  action: 'list',
  path: '/data',
  pattern: '*.xlsx'
});

// 2. 合并所有数据
let allData = [['姓名', '年龄', '城市']]; // 表头

for (const file of files.items) {
  const result = await excelTool.execute({
    action: 'read',
    path: `/data/${file.name}`,
    sheet: 'Sheet1'
  });
  
  // 跳过表头，添加数据
  allData = allData.concat(result.data.slice(1));
}

// 3. 写入合并后的文件
await excelTool.execute({
  action: 'write',
  path: '/output/merged.xlsx',
  data: allData,
  sheet: '合并数据'
});
```

### 场景4：数据分析与统计

**需求：**
对Excel数据进行统计分析

**流程：**
```javascript
// 1. 读取数据
const result = await excelTool.execute({
  action: 'read',
  path: '/data/sales.xlsx',
  sheet: '销售数据'
});

// 2. 数据分析
const analysis = analyzeData(result.data);

// 3. 创建分析报告
await excelTool.execute({
  action: 'write',
  path: '/reports/analysis.xlsx',
  data: [
    ['指标', '数值'],
    ['总销售额', analysis.totalSales],
    ['平均销售额', analysis.avgSales],
    ['最高销售额', analysis.maxSales],
    ['最低销售额', analysis.minSales]
  ],
  sheet: '统计分析'
});

// 4. 添加原始数据
await excelTool.execute({
  action: 'addSheet',
  path: '/reports/analysis.xlsx',
  sheetName: '原始数据',
  data: result.data
});
```

### 场景5：动态模板填充

**需求：**
基于模板填充数据

**流程：**
```javascript
// 1. 读取模板
const template = await excelTool.execute({
  action: 'read',
  path: '/templates/invoice_template.xlsx',
  sheet: 'Invoice'
});

// 2. 填充数据
await excelTool.execute({
  action: 'updateCell',
  path: '/templates/invoice_template.xlsx',
  sheet: 'Invoice',
  cell: 'B2',
  value: 'INV-2024-001'  // 发票号
});

await excelTool.execute({
  action: 'updateCell',
  path: '/templates/invoice_template.xlsx',
  sheet: 'Invoice',
  cell: 'B3',
  value: '2024-01-01'  // 日期
});

// ... 填充更多数据 ...

// 3. 另存为新文件
await filesystem.execute({
  action: 'copy',
  source: '/templates/invoice_template.xlsx',
  destination: '/invoices/invoice_001.xlsx'
});
```

## 技术细节

### 支持的Excel格式

**文件格式：**
- .xlsx (Excel 2007+)
- .xls (Excel 97-2003)
- .xlsm (带宏的Excel)
- .csv (逗号分隔值)

**推荐格式：**
- 新文件使用.xlsx
- 简单数据可用.csv
- 需要宏功能用.xlsm

### 数据类型处理

**支持的数据类型：**
- 字符串（String）
- 数字（Number）
- 日期（Date）
- 布尔值（Boolean）
- 公式（Formula）

**类型转换：**
```javascript
// 自动识别类型
'123' → 123 (数字)
'2024-01-01' → Date对象
'=SUM(A1:A10)' → 公式
```

### 大文件处理

**性能优化：**
- 流式读取
- 分批处理
- 内存管理
- 进度反馈

**示例：**
```javascript
// 分批读取大文件
const batchSize = 1000;
let currentRow = 1;

while (true) {
  const batch = await excelTool.execute({
    action: 'read',
    path: '/data/large.xlsx',
    sheet: 'Sheet1',
    range: `A${currentRow}:Z${currentRow + batchSize - 1}`
  });
  
  if (batch.rowCount === 0) break;
  
  // 处理这一批数据
  processBatch(batch.data);
  
  currentRow += batchSize;
}
```

## 最佳实践

### 1. 数据验证

```javascript
// 读取前检查文件是否存在
const exists = await filesystem.execute({
  action: 'exists',
  path: '/data/file.xlsx'
});

if (!exists.exists) {
  console.error('文件不存在');
  return;
}

// 读取数据并验证
const result = await excelTool.execute({
  action: 'read',
  path: '/data/file.xlsx'
});

// 检查数据完整性
if (result.rowCount === 0) {
  console.error('文件为空');
  return;
}
```

### 2. 错误处理

```javascript
try {
  const result = await excelTool.execute({
    action: 'write',
    path: '/output/data.xlsx',
    data: myData
  });
  
  if (result.success) {
    console.log('写入成功');
  }
} catch (error) {
  console.error('操作失败', error);
  
  // 清理临时文件
  await cleanup();
}
```

### 3. 性能优化

```javascript
// 批量操作优于多次单个操作
// ❌ 不推荐：多次updateCell
for (const [cell, value] of updates) {
  await excelTool.execute({
    action: 'updateCell',
    cell, value
  });
}

// ✅ 推荐：一次性写入
await excelTool.execute({
  action: 'write',
  path: '/data/file.xlsx',
  data: updatedData
});
```

### 4. 内存管理

```javascript
// 处理大文件时及时释放内存
const result = await excelTool.execute({
  action: 'read',
  path: '/data/large.xlsx'
});

// 处理数据
processData(result.data);

// 清空引用，帮助垃圾回收
result.data = null;
```

## 常见问题

### Q1: 支持哪些Excel版本？
**A:** 
- Excel 2007及以上（.xlsx）
- Excel 97-2003（.xls）
- 推荐使用.xlsx格式

### Q2: 能处理多大的文件？
**A:** 
- 理论上无限制
- 实际受内存限制
- 大文件建议分批处理
- 一般建议不超过100MB

### Q3: 公式是否会自动计算？
**A:** 
- 写入时会设置公式
- 需要Excel打开才会计算
- 或使用formula action计算

### Q4: 如何保持原有格式？
**A:** 
- 使用updateCell而非重写整个文件
- 读取时会保留格式信息
- 写入时可指定格式

### Q5: 支持合并单元格吗？
**A:** 
- 支持读取合并单元格
- 支持创建合并单元格
- 需要使用format操作

### Q6: CSV和Excel有什么区别？
**A:** 
- CSV只存储纯文本
- Excel支持格式、公式、多表
- CSV更轻量，Excel更强大

## 总结

excel-tool是PromptX的Excel处理工具，核心特点：

**功能全面：**
- 读写操作
- 格式设置
- 公式计算
- 数据筛选排序
- 图表创建

**数据准确：**
- 类型识别
- 格式保持
- 公式支持
- 验证机制

**性能优化：**
- 大文件支持
- 批量处理
- 内存管理
- 流式操作

**适用场景：**
- 报表生成
- 数据导入导出
- 批量处理
- 数据分析
- 模板填充

excel-tool让AI能够高效处理Excel文件，是PromptX数据处理的核心工具。

