Wazuh MCP協議之對話式威脅狩獵系統
專案目的：
專案展示如何透過 Model Context Protocol (MCP) + AI Agent
實現對 Wazuh SIEM 的對話式威脅狩獵（Threat Hunting）

系統架構：
AI Agent
接收自然語言指令，透過 Function Calling 決定需調用的 MCP 工具，
並將結果轉換為可讀的安全分析報告。
MCP Server
將 Wazuh REST API 抽象為標準化 JSON-RPC 工具介面，
提供告警查詢、Agent 狀態與合規性資訊存取。
Wazuh SIEM
作為安全資料來源，提供事件告警、Agent 監控與 CIS Benchmark（SCA）數據。

威脅初步分析：
使用者：列出目前所有 Wazuh Agents 的連線狀態，並分析是否存在異常登入行為。

AI 工具調用：
get_wazuh_alert_summary
回傳：
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

