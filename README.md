# 📱 基于计算机视觉和自然语言处理的生活垃圾分类检测系统

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Java](https://img.shields.io/badge/Java-24-orange.svg)](https://openjdk.java.net/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.5.4-green.svg)](https://spring.io/projects/spring-boot)
[![Vue.js](https://img.shields.io/badge/Vue.js-3.0-4FC08D.svg)](https://vuejs.org/)
[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)

本系统是一个结合了计算机视觉和自然语言处理的智能垃圾分类检测系统。系统通过YOLOv10目标检测技术识别生活垃圾类型，并使用大语言模型提供环保建议和分类指导，帮助用户正确进行垃圾分类。

## 📋 目录

- [🏗️ 系统架构](#️-系统架构)
  - [📋 架构概览](#-架构概览)
- [🎯 主要功能](#-主要功能)
- [🗄️ 数据模型设计](#️-数据模型设计)
  - [📊 实体关系图](#-实体关系图)
- [🔄 系统流程设计](#-系统流程设计)
  - [⏱️ 垃圾分类检测流程时序图](#️-垃圾分类检测流程时序图)
- [🚀 部署架构](#-部署架构)
- [💻 技术栈](#-技术栈)
- [⚡ 快速开始](#-快速开始)
- [🤖 大模型部署与使用](#-大模型部署与使用)
- [🔗 系统访问](#-系统访问)
- [📖 使用说明](#-使用说明)
- [🎯 模型信息](#-模型信息)
- [📄 许可证](#-许可证)

---

## 🏗️ 系统架构

### 📋 架构概览

```mermaid
graph TD
    %% 前端层
    subgraph Frontend["🖥️ 前端层 (Vue.js)"]
        direction TB
        UI[用户界面]
        UploadPage[图像上传页面]
        ClassifyPage[分类结果页面]
        HistoryPage[历史记录页面]
        EducationPage[环保教育页面]
        UserPage[用户管理页面]
        
        UI --- UploadPage
        UI --- ClassifyPage
        UI --- HistoryPage
        UI --- EducationPage
        UI --- UserPage
    end

    %% 业务逻辑层
    subgraph Backend["⚙️ 业务逻辑层 (Spring Boot)"]
        direction TB
        subgraph Controllers["控制器层"]
            PredController[预测控制器]
            UserController[用户控制器]
            FileController[文件控制器]
            ClassifyController[分类控制器]
        end
        
        subgraph Services["服务层"]
            ClassifyService[分类服务]
            UserService[用户服务]
            FileService[文件服务]
            EducationService[教育服务]
        end
        
        subgraph Mappers["数据访问层"]
            UserMapper[用户映射器]
            ClassifyMapper[分类记录映射器]
            WasteMapper[垃圾类型映射器]
        end
        
        Controllers --- Services
        Services --- Mappers
    end

    %% AI推理层
    subgraph AILayer["🤖 AI推理层 (Flask)"]
        direction TB
        FlaskApp[Flask应用]
        
        subgraph ModelEngine["模型引擎"]
            YOLOv10[YOLOv10模型]
            WasteClassifier[垃圾分类器]
            FeatureExtractor[特征提取器]
        end
        
        subgraph AIAssistant["AI助手服务"]
            ChatAPI[ChatAPI]
            DeepSeek[DeepSeek API]
            Qwen[Qwen API]
            LocalModels[本地模型服务]
        end
        
        FlaskApp --- ModelEngine
        FlaskApp --- AIAssistant
    end

    %% 数据存储层
    subgraph Storage["💾 数据存储层"]
        direction LR
        MySQL[(MySQL数据库)]
        FileStorage[(文件存储)]
        
        subgraph Tables["数据表"]
            UserTable[用户表]
            ClassifyTable[分类记录表]
            WasteTable[垃圾类型表]
            EducationTable[教育内容表]
        end
        
        MySQL --- Tables
    end

    %% 外部服务
    subgraph External["🌐 外部服务"]
        direction TB
        OnlineAI[在线AI服务]
        LMStudio[本地LM-Studio]
        LANAI[局域网AI服务]
        EnvironmentAPI[环保数据API]
    end

    %% 连接关系
    Frontend -.-> Backend
    Backend -.-> AILayer
    Backend -.-> Storage
    AILayer -.-> External
    AILayer -.-> Storage
    
    %% 样式
    classDef frontendStyle fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    classDef backendStyle fill:#e8f5e8,stroke:#388e3c,stroke-width:2px
    classDef aiStyle fill:#fce4ec,stroke:#c2185b,stroke-width:2px
    classDef storageStyle fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    classDef externalStyle fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    
    class Frontend,UI,UploadPage,ClassifyPage,HistoryPage,EducationPage,UserPage frontendStyle
    class Backend,Controllers,Services,Mappers,PredController,UserController,FileController,ClassifyController,ClassifyService,UserService,FileService,EducationService,UserMapper,ClassifyMapper,WasteMapper backendStyle
    class AILayer,FlaskApp,ModelEngine,AIAssistant,YOLOv10,WasteClassifier,FeatureExtractor,ChatAPI,DeepSeek,Qwen,LocalModels aiStyle
    class Storage,MySQL,FileStorage,Tables,UserTable,ClassifyTable,WasteTable,EducationTable storageStyle
    class External,OnlineAI,LMStudio,LANAI,EnvironmentAPI externalStyle
```

---

## 🎯 主要功能

- **📸 垃圾图像识别**：支持上传垃圾图像进行智能分类识别
- **🗂️ 多类别分类**：识别可回收物、有害垃圾、湿垃圾、干垃圾等多种类型
- **🎬 视频流检测**：支持实时视频流垃圾分类
- **🧠 AI环保建议**：使用大语言模型生成环保知识和分类指导
- **📋 分类记录管理**：垃圾分类历史记录管理和统计
- **📚 环保教育**：提供垃圾分类知识和环保教育内容
- **👥 用户管理**：用户系统和权限控制

---

## 🗄️ 数据模型设计

### 📊 实体关系图

```mermaid
classDiagram
    %% 实体类
    class User {
        +Integer id
        +String username
        +String password
        +String name
        +String email
        +String phone
        +String avatar
        +Date createTime
    }
    
    class ClassifyRecord {
        +Integer id
        +String recordId
        +String imagePath
        +String resultPath
        +String wasteType
        +String wasteCategory
        +Double confidence
        +String suggestion
        +String username
        +DateTime classifyTime
        +String aiModel
        +String ecoAdvice
    }
    
    class WasteCategory {
        +Integer id
        +String categoryName
        +String description
        +String color
        +String disposalMethod
        +String examples
        +Boolean isRecyclable
    }
    
    class EducationContent {
        +Integer id
        +String title
        +String content
        +String category
        +String imageUrl
        +Integer views
        +Date createTime
    }
    
    %% 控制器类
    class PredictionController {
        -ClassifyService classifyService
        -RestTemplate restTemplate
        +classifyWaste() Result
        +uploadImage() Result
        +getClassifyHistory() Result
    }
    
    class ClassifyController {
        -ClassifyService classifyService
        +createRecord() Result
        +updateRecord() Result
        +getStatistics() Result
        +exportReport() Result
    }
    
    %% 服务类
    class FlaskAIService {
        -YOLOv10Model model
        -ChatAPI chatAPI
        +classifyWasteImage() Dict
        +generateEcoAdvice() String
        +extractFeatures() Array
        +analyzeWasteComposition() Dict
    }
    
    class YOLOv10Model {
        -String weightsPath
        -String configPath
        +loadModel() Boolean
        +predictWaste() Array
        +preprocessImage() Tensor
        +postprocessResults() Dict
    }
    
    class ChatAPI {
        -String apiKey
        -String endpoint
        +generateEcoAdvice() String
        +getRecyclingTips() String
        +explainClassification() String
    }
    
    %% 关系定义
    User "1" --> "*" ClassifyRecord : creates
    WasteCategory "1" --> "*" ClassifyRecord : belongs_to
    
    PredictionController --> FlaskAIService : calls
    ClassifyController --> ClassifyService : uses
    
    FlaskAIService --> YOLOv10Model : uses
    FlaskAIService --> ChatAPI : uses
    
    YOLOv10Model --> WasteCategory : classifies_into
```

---

## 🔄 系统流程设计

### ⏱️ 垃圾分类检测流程时序图

```mermaid
sequenceDiagram
    participant U as 🧑‍💼 用户
    participant V as 💻 Vue前端
    participant S as ⚙️ Spring Boot
    participant F as 🤖 Flask AI服务
    participant Y as 🎯 YOLOv10模型
    participant A as 🧠 AI助手
    participant D as 💾 数据库
    participant FS as 📁 文件存储
    
    %% 垃圾图像上传阶段
    Note over U,FS: 📤 垃圾图像上传阶段
    U->>V: 1. 上传垃圾图像
    V->>FS: 2. 保存图像到文件系统
    FS-->>V: 3. 返回图像访问URL
    
    %% 分类配置阶段
    Note over U,V: ⚙️ 分类参数配置阶段
    U->>V: 4. 选择检测模型
    U->>V: 5. 设置置信度阈值
    U->>V: 6. 选择AI助手
    U->>V: 7. 启用环保建议(可选)
    
    %% 垃圾分类阶段
    Note over U,A: 🔍 AI分类阶段
    U->>V: 8. 点击开始分类
    V->>S: 9. POST /api/waste/classify
    Note right of V: 包含所有配置参数
    
    S->>F: 10. 转发请求到Flask
    F->>Y: 11. 加载模型并预测
    Y->>Y: 12. 垃圾类型识别
    Y->>Y: 13. 分类置信度计算
    Y-->>F: 14. 返回分类结果
    
    F->>FS: 15. 保存标注图片
    FS-->>F: 16. 返回图片URL
    
    %% AI环保建议阶段
    Note over F,A: 🌿 环保建议阶段
    alt 用户开启了环保建议
        F->>A: 17. 发送分类结果
        Note right of F: 根据选择调用对应AI服务
        A->>A: 18. 生成环保建议
        A->>A: 19. 提供分类指导
        A-->>F: 20. 返回环保建议
    end
    
    %% 数据保存阶段
    Note over S,D: 💾 数据保存阶段
    F-->>S: 21. 返回完整结果
    S->>D: 22. 保存分类记录
    D-->>S: 23. 确认保存成功
    
    %% 结果展示阶段
    Note over U,V: 📊 结果展示阶段
    S-->>V: 24. 返回分类结果
    V-->>U: 25. 展示垃圾分类
    V-->>U: 26. 显示环保建议
    
    %% 教育内容推荐阶段
    Note over U,V: 📚 教育推荐(可选)
    U->>V: 27. 查看相关教育内容
    V-->>U: 28. 推荐环保知识
```

---

## 🚀 部署架构

### 🖥️ 部署架构图

```mermaid
graph TB
    %% 客户端层
    subgraph Client["👥 客户端层"]
        direction LR
        WebBrowser[🌐 Web浏览器]
        MobileApp[📱 移动应用]
        PublicKiosk[🏢 公共垃圾分类终端]
    end
    
    %% 负载均衡层
    subgraph LoadBalancer["⚖️ 负载均衡层"]
        Nginx[🔄 Nginx反向代理<br/>端口: 80/443<br/>静态资源缓存]
    end
    
    %% 应用服务层
    subgraph AppServer["🖥️ 应用服务层"]
        direction TB
        
        subgraph Frontend["🎨 前端服务"]
            Vue[Vue.js应用<br/>📦 Node.js<br/>🔗 端口: 3000]
        end
        
        subgraph Backend["⚙️ 后端服务"]
            SpringBoot[Spring Boot应用<br/>☕ JDK 24<br/>🔗 端口: 9999]
        end
        
        subgraph AIService["🤖 AI分类服务"]
            Flask[Flask应用<br/>🐍 Python 3.8+<br/>🔗 端口: 5000]
            YOLOv10[🎯 YOLOv10模型<br/>🔥 PyTorch/CUDA<br/>♻️ 垃圾分类专用]
        end
    end
    
    %% AI环保助手服务层
    subgraph EcoService["🌿 AI环保服务层"]
        direction TB
        
        subgraph Local["💻 本地服务"]
            LMStudio[🏠 LM-Studio<br/>🔗 端口: 1234<br/>🌱 环保知识模型]
        end
        
        subgraph Cloud["☁️ 云端服务"]
            DeepSeek[🌊 DeepSeek环保版<br/>🔑 API密钥认证<br/>♻️ 环保建议专用]
            Qwen[🔮 Qwen助手<br/>🔑 API密钥认证<br/>📚 分类教育生成]
        end
        
        subgraph LAN["🏢 局域网服务"]
            LANModels[🌐 局域网环保AI<br/>🔗 192.168.1.108:1234<br/>🏢 企业内部署]
        end
    end
    
    %% 数据存储层
    subgraph DataLayer["💾 数据存储层"]
        direction LR
        MySQL[🗄️ MySQL数据库<br/>🔗 端口: 3306<br/>📊 分类记录/用户数据]
        FileStorage[📁 图像存储系统<br/>📷 垃圾图片/标注结果]
        BackupStorage[💾 备份存储<br/>📋 定期数据备份]
    end
    
    %% 外部环保服务
    subgraph EcoSystems["🌍 环保系统集成"]
        GovAPI[🏛️ 政府环保API<br/>📊 政策法规数据]
        RecycleAPI[♻️ 回收系统API<br/>📍 回收点位信息]
        EduAPI[📚 环保教育API<br/>📖 环保知识库]
        WeatherAPI[🌤️ 环境监测API<br/>🌡️ 空气质量数据]
    end
    
    %% 连接关系
    Client -.->|HTTPS/HTTP| LoadBalancer
    LoadBalancer -.->|反向代理| Frontend
    LoadBalancer -.->|API转发| Backend
    
    Frontend -.->|REST API| Backend
    Backend -.->|HTTP调用| AIService
    Backend -.->|JDBC| DataLayer
    
    AIService -.->|HTTP请求| EcoService
    AIService -.->|文件操作| DataLayer
    
    Backend -.->|API调用| EcoSystems
    
    %% 样式定义
    classDef clientStyle fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    classDef proxyStyle fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    classDef appStyle fill:#e8f5e8,stroke:#388e3c,stroke-width:2px
    classDef ecoStyle fill:#fce4ec,stroke:#c2185b,stroke-width:2px
    classDef dataStyle fill:#fff8e1,stroke:#f9a825,stroke-width:2px
    classDef systemStyle fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    
    class WebBrowser,MobileApp,PublicKiosk clientStyle
    class Nginx proxyStyle
    class Vue,SpringBoot,Flask,YOLOv10 appStyle
    class LMStudio,DeepSeek,Qwen,LANModels ecoStyle
    class MySQL,FileStorage,BackupStorage dataStyle
    class GovAPI,RecycleAPI,EduAPI,WeatherAPI systemStyle
```

---

## 💻 技术栈

### 🎨 前端技术
- **框架**: Vue 3 + TypeScript
- **UI库**: Element Plus
- **构建工具**: Vite
- **状态管理**: Pinia
- **HTTP客户端**: Axios

### ⚙️ 后端技术
- **框架**: Spring Boot 3.5.4
- **Java版本**: JDK 24 (最新版本)
- **数据库**: MySQL 8.0+
- **ORM**: MyBatis-Plus
- **文档**: Swagger/OpenAPI

### 🤖 AI技术栈
- **推理框架**: Flask + SocketIO
- **深度学习**: YOLOv10
- **模型库**: Ultralytics, PyTorch
- **大语言模型**: DeepSeek, Qwen, 本地LLM
- **GPU加速**: CUDA

### 🛠️ 工具与部署
- **视频处理**: FFmpeg
- **数据库**: MySQL
- **本地AI**: LM-Studio
- **代理服务**: Nginx (可选)
- **容器化**: Docker (可选)

---

## ⚡ 快速开始

### 前提条件

- **JDK 24** (已升级到最新版本，包含性能优化和安全改进)
- Node.js 16+
- Python 3.8+
- MySQL 8.0+
- CUDA支持的GPU (推荐用于模型推理)
- LM-Studio (用于本地部署大模型)

### 数据库配置

1. 创建名为`ai`的数据库
2. 运行`database.sql`脚本初始化数据库结构

### 后端服务启动

1. 进入springboot目录
2. 使用Maven构建项目：`mvn clean package`
3. 运行生成的jar文件：`java -jar target/Kcsj-0.0.1-SNAPSHOT.jar`

### AI服务启动

1. 进入flask目录
2. 安装依赖：`pip install -r requirements.txt`
3. 启动Flask服务：
   ```bash
   python main\(YOLO\).py  # YOLOv10模型
   # 或
   python main\(DETR\).py  # RT-DETR模型
   ```

### 前端启动

1. 进入vue目录
2. 安装依赖：`npm install`
3. 启动开发服务器：`npm run dev`
4. 构建生产版本：`npm run build`

---

## 🤖 大模型部署与使用

本系统支持多种大模型部署方式，用于生成环保建议和分类指导：

### 支持的模型

- **云端API模型**
  - Deepseek-R1
  - Qwen

- **局域网部署模型**
  - Deepseek-R1-LAN
  - Qwen3-LAN
  - Qwen2.5-VL-LAN
  - Qwen2.5-Omni-LAN
  - Gemma3-LAN

- **本地部署模型**
  - Deepseek-R1-Local
  - Qwen3-Local
  - Qwen2.5-VL-Local
  - Qwen2.5-Omni-Local
  - Gemma3-Local

### 使用LM-Studio进行本地部署

1. 下载并安装 [LM-Studio](https://lmstudio.ai/)
2. 从Hugging Face或其他来源下载所需模型（如Deepseek-R1、Qwen等）
3. 在LM-Studio中加载模型
4. 启动本地API服务器（通常在http://localhost:1234）
5. 在系统设置中选择对应的"本地"模型选项

---

## 🔗 系统访问

- 前端页面：http://localhost:3000
- Spring Boot 后端：http://localhost:9999
- Flask AI 服务：http://localhost:5000

### 默认登录账号

- **管理员账号**：admin
- **密码**：admin123

---

## 📖 使用说明

1. 访问系统前端界面
2. 使用默认管理员账号登录：admin/admin123
3. 进入系统后可以使用：
   - 垃圾分类：上传垃圾图片进行智能分类
   - 实时检测：使用摄像头进行实时垃圾分类
   - 历史记录：查看分类历史和统计数据
   - 环保教育：学习垃圾分类知识和环保常识
4. 选择合适的大模型获取个性化环保建议

---

## 🎯 模型信息

### 垃圾分类模型

本系统使用YOLOv10模型对垃圾进行分类检测。预训练模型存储在`flask/weights/`目录下，支持以下垃圾类别：

- 🗑️ **干垃圾**：餐具、包装袋、卫生纸等
- 🥬 **湿垃圾**：果皮、菜叶、剩饭剩菜等  
- ♻️ **可回收物**：塑料瓶、纸张、金属等
- ⚠️ **有害垃圾**：电池、化学品、灯管等

### 大语言模型

支持多种大语言模型，既可以通过API密钥访问云端模型，也可以通过LM-Studio在本地部署运行。系统会根据选定的模型自动配置请求参数，提供个性化的环保建议和分类指导。

---

## 📄 许可证

MIT

---

## 🤝 贡献

欢迎提交问题和贡献代码，请通过创建Issue或Pull Request参与项目开发。

---

## 🙏 致谢

感谢所有为本项目提供支持和贡献的人员，以及为环保事业做出贡献的每一个人。让我们一起为地球的未来努力！ 🌍♻️
