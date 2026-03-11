```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'primaryColor': '#fff', 'primaryBorderColor': '#000', 'lineColor': '#000', 'tertiaryColor': '#f5f5f5'}}}%%
flowchart TB
    subgraph 模式层_Schema_Layer
        direction LR
        A1[确定本体构建范围] --> A2[确定核心实体、属性与关系] --> A3[复用与自定义本体] --> A4[形成领域本体]
    end

    subgraph 数据层_Data_Layer
        direction LR
        B1[“多源数据获取<br>（半/非/结构化）”] --> B2[数据清洗与预处理] --> B3[“知识抽取<br>（实体/关系/属性）”] --> B4[知识融合与对齐] --> B5[构建知识图谱]
    end

    subgraph 应用层_Application_Layer
        direction LR
        C1[“知识存储<br>（Neo4j图形数据库）”]
        C2[语义检索与智能问答]
        C3[图谱可视化展示]
    end

    A4 -. 指导与约束 .-> B3
    B5 --> C1
    C1 -. 支撑 .-> C2
    C1 -. 支撑 .-> C3
    end
```
