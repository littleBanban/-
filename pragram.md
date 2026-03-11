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
flowchart LR
    %% ===============================
    %% BERT 模型总体结构示意图
    %% ===============================

    %% 输入层
    subgraph INPUT["输入层（Input Layer）"]
        direction LR
        T["原始文本序列<br/>Raw Text"]
        S["分词与子词切分<br/>Tokenization / Subword Segmentation"]
        ID["词表映射<br/>Token IDs"]
        SEG["句子标记<br/>Segment IDs"]
        POS["位置索引<br/>Position Indices"]
        MASK["注意力掩码<br/>Attention Mask"]
        T --> S --> ID
        ID --> SEG
        ID --> POS
        ID --> MASK
    end

    %% 嵌入层
    subgraph EMB["嵌入层（Embedding Layer）"]
        direction LR
        WE["词嵌入<br/>Token Embeddings"]
        SE["句段嵌入<br/>Segment Embeddings"]
        PE["位置嵌入<br/>Position Embeddings"]
        ADD["向量相加与归一化<br/>Element-wise Sum & LayerNorm"]
        WE --> ADD
        SE --> ADD
        PE --> ADD
    end

    %% 多层 Transformer Encoder
    subgraph ENC["编码层（L 层 Transformer Encoder）"]
        direction TB

        subgraph L1["第 1 层 Encoder Layer"]
            direction TB
            A1["多头自注意力<br/>Multi-Head Self-Attention"]
            R1["残差连接 + 层归一化<br/>Residual + LayerNorm"]
            F1["前馈网络<br/>Position-wise FFN"]
            R1b["残差连接 + 层归一化<br/>Residual + LayerNorm"]
            A1 --> R1 --> F1 --> R1b
        end

        subgraph L2["第 2 层 Encoder Layer"]
            direction TB
            A2["多头自注意力"]
            R2["残差连接 + 层归一化"]
            F2["前馈网络"]
            R2b["残差连接 + 层归一化"]
            A2 --> R2 --> F2 --> R2b
        end

        %% 省略中间层，仅示意
        DOTS["⋮<br/>重复 L 次 Encoder 层结构"]

        subgraph LL["第 L 层 Encoder Layer"]
            direction TB
            AL["多头自注意力"]
            RL["残差连接 + 层归一化"]
            FL["前馈网络"]
            RLb["残差连接 + 层归一化"]
            AL --> RL --> FL --> RLb
        end

        L1 --> L2 --> DOTS --> LL
    end

    %% 预训练与下游任务头
    subgraph HEAD["预训练任务与下游任务头（Task Heads）"]
        direction TB
        CLS["[CLS] 向量<br/>Sentence-level Representation"]
        TOK["序列向量<br/>Token-level Representations"]

        MLM["掩码语言模型头<br/>Masked Language Modeling Head"]
        NSP["下一句预测 / 句对分类头<br/>Next Sentence Prediction / Sequence Classification Head"]
        NER["序列标注 / 下游任务头<br/>Token-level Classification Head<br/>(

