Wazuh MCP協議之對話式威脅狩獵系統
專案目的

本專案展示如何透過 Model Context Protocol (MCP) + AI Agent
實現對 Wazuh SIEM 的對話式威脅狩獵（Threat Hunting）。

AI Agent 可透過自然語言介面查詢安全事件，支援以下應用場景：

資產清查（Asset Inventory）

即時威脅狩獵（Threat Hunting）

自動化合規性評估（Compliance Review）

本專案以 模擬 MCP 工具鏈與抽象 API 介面 呈現完整操作流程，
用於驗證 AI 導向 SecOps 架構的可行性與設計完整性。

系統架構

AI Agent
接收自然語言指令，透過 Function Calling 決定需調用的 MCP 工具，
並將結果轉換為可讀的安全分析報告。

MCP Server
將 Wazuh REST API 抽象為標準化 JSON-RPC 工具介面，
提供告警查詢、Agent 狀態與合規性資訊存取。

Wazuh SIEM
作為安全資料來源，提供事件告警、Agent 監控與 CIS Benchmark（SCA）數據。

技術重點

Model Context Protocol (MCP)
建立 AI 與安全工具間的標準化互動模式

Function Calling
AI 自動判斷查詢意圖並選擇對應工具

Docker / 容器化架構（概念驗證）
模擬高效能 MCP Server 與 SIEM 服務的部署模式

JSON 資料轉譯
將 Wazuh 告警轉換為對應 MITRE ATT&CK 的分析結果

對話式操作展示（模擬）
🔹 資產清查 / 威脅初步分析

使用者指令

列出目前所有 Wazuh Agents 的連線狀態，並分析是否存在異常登入行為。


AI 工具調用

get_wazuh_alert_summary


模擬回傳結果

{
  "alerts": [
    {
      "alert_id": 101,
      "level": 12,
      "rule": "Multiple failed login attempts",
      "src_ip": "192.168.1.50",
      "target_user": "admin"
    },
    {
      "alert_id": 102,
      "level": 11,
      "rule": "Authentication failure",
      "agent": "host2",
      "target_user": "root"
    }
  ],
  "analysis": "偵測到多次登入失敗事件，具備暴力破解行為特徵，建議封鎖來源 IP 並檢查帳號安全性。"
}

🔹 自動化合規性評估（CIS Benchmark）

使用者指令

檢查目前系統的 CIS Benchmark 合規性狀況，找出潛在的安全配置錯誤。


AI 工具調用

get_wazuh_sca_report


模擬回傳結果

{
  "compliance_issues": [
    {
      "category": "Password Policy",
      "issue": "Minimum password length < 12"
    },
    {
      "category": "SSH Configuration",
      "issue": "Root login enabled"
    },
    {
      "category": "Firewall",
      "issue": "Unused ports not restricted"
    }
  ],
  "recommendation": "強化密碼政策、關閉 root 遠端登入、依 CIS 建議調整防火牆規則。"
}

成果與價值

將複雜的 SIEM 操作轉化為自然語言對話流程

AI 自動完成告警彙整與風險判斷，降低人工作業成本

清楚展示 Threat Hunting 與 Compliance Review 的完整流程

成果與價值

將繁雜 SIEM 維運操作轉化為對話式自然語言操作

AI 即時分析告警，降低資安人員操作門檻
