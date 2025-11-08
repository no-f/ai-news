# 全球安防企业AI产品发布监控工作流

## 工作流概述

这个n8n工作流旨在自动监控全球主要安防企业的AI产品发布动态，通过Serper搜索API定期获取最新消息，并将结果结构化保存为JSON文件。

## 核心功能

1. **定时触发** - 每日自动执行搜索任务
2. **多源搜索** - 同时监控多个安防企业
3. **智能解析** - 提取产品发布的关键信息
4. **结构化输出** - 生成标准化的JSON数据文件
5. **通知机制** - 发现新产品时发送通知

## 目标企业清单

### 一线安防企业
- **Hikvision** - 海康威视
- **Dahua Technology** - 大华技术
- **Axis Communications** - 安讯士
- **Bosch Security Systems** - 博世安防
- **Honeywell Security** - 霍尼韦尔安防

### 二线安防企业
- **Hanwha Techwin** - 韩华Techwin
- **Avigilon (Motorola)** - 摩托罗拉旗下
- **Pelco (Motorola)** - 摩托罗拉旗下
- **Panasonic i-PRO** - 松下安防
- **VIVOTEK** - 晶睿通讯

### AI安全企业
- **Johnson Controls** - 江森自控
- **Palo Alto Networks** - 帕洛阿尔托网络
- **SentinelOne** - 哨兵科技
- **CrowdStrike** - 克罗安全
- **Trend Micro** - 趋势科技

## 搜索关键词组合

### 主要搜索模式
1. `{company_name} AI product release {year}`
2. `{company_name} artificial intelligence announcement`
3. `{company_name} AI security camera launch`
4. `{company_name} AI surveillance technology`
5. `{company_name} machine learning security`

### 时间范围
- 当前年份：2024-2025
- 回溯搜索：最近6个月

## JSON输出格式

```json
{
  "search_date": "2025-01-08",
  "total_results": 15,
  "companies_searched": [
    "Hikvision",
    "Dahua Technology",
    "Axis Communications"
  ],
  "ai_releases": [
    {
      "company": "Johnson Controls",
      "product_name": "Illustra Edge AI License Plate Recognition Camera",
      "release_date": "2024-09-23",
      "announcement_type": "product_launch",
      "event_source": "GSX 2024",
      "key_features": [
        "Edge AI-based ANPR application",
        "Frictionless access control",
        "Real-time analytics",
        "Comprehensive cybersecurity"
      ],
      "target_markets": [
        "Enterprise",
        "Commercial",
        "Residential"
      ],
      "availability": "North America and Europe",
      "source_url": "https://www.johnsoncontrols.com/media-center/news/press-releases/2024/09/23/johnson-controls-global-security-products-unveils-ai-powered-integrations-at-gsx-2024",
      "search_query": "Johnson Controls AI product release 2024"
    }
  ],
  "metadata": {
    "search_queries_used": 45,
    "successful_extractions": 15,
    "failed_extractions": 3,
    "processing_time_seconds": 127
  }
}
```

## 工作流节点配置

### 1. 定时触发器 (Cron)
```json
{
  "triggerTimes": {
    "item": [{
      "hour": 9,
      "minute": 0
    }]
  }
}
```

### 2. 搜索配置 (Serper API)
```json
{
  "url": "https://google.serper.dev/search",
  "method": "POST",
  "headers": {
    "X-API-KEY": "{{$credentials.serperApiKey}}",
    "Content-Type": "application/json"
  },
  "body": {
    "q": "{{$node['Set'].json['searchQuery']}}",
    "num": 20,
    "tbm": "nws",
    "tbs": "qdr:d"
  }
}
```

### 3. 数据解析和结构化
- 提取产品名称
- 识别发布日期
- 分类产品类型
- 解析关键特性
- 确定目标市场

### 4. JSON文件生成
- 聚合所有搜索结果
- 格式化输出结构
- 保存到指定路径
- 版本控制和备份

## 部署和监控

### 环境要求
- n8n实例 (推荐云端部署)
- Serper API密钥
- 文件存储权限
- 邮件/SMTP配置（可选）

### 监控指标
- 每日搜索结果数量
- 数据提取成功率
- API调用频率
- 错误日志追踪

## 扩展功能

### 高级特性
1. **情感分析** - 分析媒体对产品的反应
2. **竞争分析** - 对比不同企业的AI产品策略
3. **趋势预测** - 基于历史数据预测未来发布
4. **多语言支持** - 支持中文、英文等多语言搜索
5. **API集成** - 与企业内部系统对接

### 数据源扩展
- 社交媒体监控（Twitter、LinkedIn）
- 行业论坛和博客
- 企业官方新闻发布
- 展会和活动信息
- 专利数据库

这个工作流将帮助您建立一个全面的安防企业AI产品发布监控系统，为您的市场分析和竞争情报提供及时、准确的数据支持。