flowchart TD
    subgraph 阶段一：设备控制 [Phase 1: Device Control via go-ios]
        A[🎯 输入 UDID & Bundle ID] --> B[go-ios list 校验设备]
        B --> C[go-ios runwda 启动 WDA]
        C --> D[go-ios launch 启动目标 App]
    end

    subgraph 阶段二：流量捕获 [Phase 2: Traffic Capture]
        D --> E[🧿 启动 mitmproxy 监听 8080]
        E --> F[iOS 配置 Wi-Fi 代理]
        F --> G[🤖 UI 自动化触发 API 请求]
        G -->|产生流量| H[💾 保存 traffic.mitm]
        H --> I[停止 mitmproxy]
    end

    subgraph 阶段三：情报分析与扫描 [Phase 3: Intel & Scanning]
        I --> J[🔄 mitmdump: mitm 转 HAR]
        J --> K[🧠 ios-traffic-intel 解析 HAR]
        K -->|提取 IDOR/敏感目标| L[🔫 Nuclei 扫描引擎]
    end

    subgraph 身份与网络层 [Identity & Network Layer]
        R[⚙️ Rota 代理池 / ProxyHat] -.->|提供住宅出口 IP| L
        P[🐍 Python 辅助脚本] -.->|调用 go-ios REST API| A
    end

    L --> M((📦 输出漏洞结果 JSONL))
    M --> N[📊 生成最终赏金报告]
    
    style A fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    style E fill:#fff3e0,stroke:#e65100,stroke-width:2px
    style K fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px
    style L fill:#ffebee,stroke:#c62828,stroke-width:2px
    style R fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
