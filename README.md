Wazuh MCP協議之對話式威脅狩獵系統
專案目的

展示如何透過 **Model Context Protocol (MCP) + AI Agent**進行對話式威脅狩獵（Threat Hunting）。  

AI Agent 可透過自然語言查詢 Wazuh 資安事件，實現以下目標：

- 資產清查（Asset Inventory）  
- 實時威脅狩獵（Threat Hunting）  
- 自動化合規性評估（Compliance Review）  

> 本專案以理論設計方式呈現 Demo，不依賴真實 Wazuh Manager 或 MCP Server，但可完整描述操作流程。

 系統架構

- **AI Agent**：根據自然語言輸入，自動調用 MCP Server API，產生可讀 Threat Hunting 報告。  
- **MCP Server**：將 Wazuh REST API 封裝為標準化 JSON-RPC 工具。  
- **Wazuh SIEM**：收集端點事件，提供告警、代理狀態與合規性數據。

---

## 技術重點

- **Model Context Protocol (MCP)**：標準化 AI 與工具間的溝通協議  
- **Function Calling**：AI 自動判斷需要調用的底層工具  
- **Docker / 容器化設計**（理論）：模擬高效能 MCP Server 環境  
- **JSON 數據轉換**：將 Wazuh 告警轉換為 MITRE ATT&CK 框架分析報告

---




**操作指令：**
列出目前所有 Wazuh Agents 的連線狀態，並回報受監控主機的 OS 資訊。

回傳結果：
{
  "alerts": [
    {"id": 101, "severity": "critical", "message": "Failed login attempts from 192.168.1.50"},
    {"id": 102, "severity": "critical", "message": "Multiple authentication failures on host2"}
  ],
  "analysis": "疑似遭受暴力破解攻擊，目標帳號為 admin 與 root，建議立即鎖定來源 IP 並通知資安團隊。"
}
檢查目前系統的 CIS Benchmark 合規性狀況，找出潛在的安全配置錯誤。
{
  "compliance_issues": [
    {"category": "Password Policy", "issue": "最小密碼長度不足 12 字元"},
    {"category": "SSH Configuration", "issue": "允許 root 遠端登入"},
    {"category": "Firewall", "issue": "部分端口未依 CIS 建議封鎖"}
  ],
  "recommendation": "調整密碼策略、禁用 root 遠端登入、修正防火牆規則"
}

成果與價值

將繁雜 SIEM 維運操作轉化為對話式自然語言操作

AI 即時分析告警，降低資安人員操作門檻

模擬展示 Threat Hunting、Compliance Review 的全流程

即使不實作 Wazuh，也可清楚呈現系統設計與操作結果
