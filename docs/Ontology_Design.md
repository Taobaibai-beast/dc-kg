DC-KG 本体设计文档



项目概述



本项目构建了一套可发表级别的模块化本体体系，覆盖碳排放/碳汇时序、政策、碳测算模型，并完整导入Neo4j图数据库。



&nbsp;模块化架构



核心模块



| 模块 | 功能 | 引用标准/业界实践 |

|------|------|------------------|

| Core-Geo | 城市、区县、地块 + 空间索引 | GeoSPARQL 1.1 |

| Policy | 多层级政策 → 目标 → 行业 | SDG Ontology, SKOS |

| Model | 多种碳排放/碳汇模型，公式、输入、系数、适用范围、结果不确定性 | ISO 14064-1, IPCC 2006, LCSA-Core |

| TimeSeries | 年/月/日排放 \& 碳汇值，单位、置信区间 | SDMX, QUDT |



采用的国际标准



语义网标准

\- OWL 2 Web Ontology Language: 本体建模语言

\- RDF/XML: 本体序列化格式

\- QUDT (Quantities, Units, Dimensions and Types): 单位和量值标准化



领域标准

\- GeoSPARQL 1.1: 地理空间数据表示

\- ISO 14064-1: 温室气体核算标准

\- IPCC 2006: 国际碳排放计算方法

\- SDG Ontology: 可持续发展目标本体

\- SKOS: 简单知识组织系统



关键技术特性



unit属性设计

\- 类型: 对象属性（ObjectProperty）

\- Range: qudt:Unit

\- 语义价值: 支持单位层次和换算，实现"语义可查询"功能

\- 标准对齐: 与QUDT官方标准兼容



模块化设计原则

\- 松耦合: 各模块独立维护

\- 强内聚: 模块内语义关系清晰

\- 标准化: 遵循W3C和领域标准

\- 可扩展: 支持未来模块添加



实施架构



开发工具

\- Protégé 5.6.6: 本体编辑器

\- OntoGraf插件: 本体可视化

\- AutoIRIMapper: IRI自动映射（内置功能）



图数据库

\- Neo4j 5.26 Community: 图数据库引擎

\- neosemantics (n10s) 5.20: RDF导入插件

\- APOC 5.26.1: 数据处理插件



导入统计



本体导入结果

\- Core模块: 16个三元组

\- Policy模块: 14个三元组

\- Model模块: 22个三元组

\- TimeSeries模块: 20个三元组

\- 总计: 72个三元组成功导入



Neo4j图结构

\- 总节点数: 47个

\- 类节点: 20个（含QUDT导入）

\- 关系节点: 12个

\- 属性节点: 7个



文件结构



```

ontologies/

├── core.owl (5 KB)

├── policy.owl (4 KB)

├── model.owl (5 KB)

├── timeseries.owl (5 KB)

├── dc-kg.owl (1 KB) - 汇总文件

└── catalog-v001.xml - Auto IRI Mapper配置

```



验证与质量保证



本体验证

\- ✅ HermiT推理器一致性检查

\- ✅ 所有模块成功导入Neo4j

\- ✅ unit属性语义查询功能验证



标准合规性

\- ✅ OWL 2 DL语法合规

\- ✅ QUDT单位标准对接

\- ✅ GeoSPARQL空间数据支持



\## 使用指南



查询示例

```cypher

/ 查询所有Resource节点（查看导入内容）

MATCH (n:Resource) RETURN labels(n), n.uri, n.name LIMIT 10;



// 查询本体中的类（使用标签检查）

MATCH (c:Resource) 

WHERE "n4sch\_\_Class" IN labels(c)

RETURN c.uri, c.name LIMIT 10;



// 查询特定的本体类

MATCH (n:Resource) 

WHERE n.uri CONTAINS 'timeseries' OR n.uri CONTAINS 'EmissionRecord' OR n.uri CONTAINS 'CarbonModel'

RETURN labels(n), n.uri, n.name LIMIT 10;



// 查询本体中的类（属性方式）

MATCH (c:Resource) WHERE c.`n4sch\_\_Class` IS NOT NULL 

RETURN c.uri, c.name;



// 查询QUDT单位支持

MATCH (n:Resource) WHERE n.uri CONTAINS 'unit' 

RETURN n.uri;

```



扩展方法

1\. 在Protégé中编辑相应模块

2\. 保存并提交到Git

3\. 重新导入到Neo4j进行验证



参考文献



1\. W3C OWL 2 Web Ontology Language Document Overview (2012)

2\. GeoSPARQL - A Geographic Query Language for RDF Data (2012)

3\. QUDT - Quantities, Units, Dimensions and Types Ontologies (2021)

4\. ISO 14064-1:2018 Greenhouse gases

5\. IPCC 2006 Guidelines for National Greenhouse Gas Inventories



版本信息



\- 项目版本: 1.0.0

\- 最后更新: 2025-07-01

\- 维护状态: 活跃开发中



---

本文档随项目演进持续更新

