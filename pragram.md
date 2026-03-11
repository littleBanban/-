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
```
```mermaid
flowchart TB
    %% BERT 模型总体结构示意图

    subgraph L0["输入表示层（Input Representation）"]
        direction TB
        X["原始文本序列"]
        TK["分词 / 子词切分"]
        ID["Token IDs"]
        SID["Segment IDs"]
        PID["Position IDs"]
        X --> TK --> ID
        ID --> SID
        ID --> PID
    end

    subgraph L1["嵌入层（Embedding Layer）"]
        direction TB
        TE["词嵌入"]
        SE["句段嵌入"]
        PE["位置嵌入"]
        SUM["嵌入向量相加 + LayerNorm"]
        ID --> TE
        SID --> SE
        PID --> PE
        TE --> SUM
        SE --> SUM
        PE --> SUM
    end

    subgraph L2["编码层（L 层 Transformer Encoder）"]
        direction TB
        ENC1["第 1 层 Encoder Layer"]
        ENC2["第 2 层 Encoder Layer"]
        MID["⋮"]
        ENCL["第 L 层 Encoder Layer"]
        ENC1 --> ENC2 --> MID --> ENCL
    end

    subgraph L3["预训练任务与下游任务层"]
        direction TB
        CLS["[CLS] 句级表示"]
        TOK["Token 级表示"]
        MLM["掩码语言模型头"]
        NSP["句对 / 文本分类头"]
        NER["序列标注任务头"]
        CLS --> NSP
        CLS --> MLM
        TOK --> MLM
        TOK --> NER
    end

    L0 --> L1 --> L2
    L2 --> CLS
    L2 --> TOK
