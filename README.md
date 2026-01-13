# Wazuh MCP 協議之對話式威脅狩獵系統

## 專案目的

這次目標展示如何透過 **Model Context Protocol (MCP)** 結合 **AI Agent**，
實現對 Wazuh SIEM 的對話式威脅狩獵（Threat Hunting）。



## 系統架構說明

整體系統由三個主要元件組成：

### AI Agent
AI Agent 接收使用者的自然語言指令，
並透過 Function Calling 判斷需調用的 MCP 工具，
最終將回傳資料轉換為可讀的安全分析結果。

### MCP Server
MCP Server 將 Wazuh REST API 抽象化為標準化的 JSON-RPC 工具介面，
提供告警查詢、Agent 狀態與合規性資料存取功能。

### Wazuh SIEM
Wazuh 作為資安事件來源，
負責收集端點事件、產生安全告警，
並提供 CIS Benchmark（SCA）合規性評估資料。

---

## 技術重點

- Model Context Protocol (MCP)：標準化 AI 與工具之間的溝通方式
- Function Calling：由 AI 自動決定所需調用的底層安全工具
- Docker 與容器化設計（概念）：模擬 MCP Server 的部署環境
- JSON 資料轉譯：將原始告警轉換為結構化安全分析結果

---

## 操作情境

### 資產清查與威脅初步分析

使用者操作指令：
列出目前所有 Wazuh Agents 的連線狀態，並分析是否存在異常登入行為。

AI Agent 透過 MCP Server 呼叫告警摘要工具後，回傳分析結果如下：

```json
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
```

使用者操作指令：
檢查目前系統的 CIS Benchmark 合規性狀況，找出潛在的安全配置錯誤。
回傳：
```json{
  "compliance_issues": [
    {
      "category": "Password Policy",
      "issue": "最小密碼長度不足 12 字元"
    },
    {
      "category": "SSH Configuration",
      "issue": "允許 root 遠端登入"
    },
    {
      "category": "Firewall",
      "issue": "部分端口未依 CIS 建議封鎖"
    }
  ],
  "recommendation": "建議調整密碼策略、停用 root 遠端登入，並修正防火牆規則以符合 CIS Benchmark。"
}
```
這次實作透過MCP與AI Agent的整合，
展示未來Chat-based SecOps的實務應用可能性。
