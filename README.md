# SIEM Detections: Sigma, Wazuh & Windows Telemetry

![Wazuh](https://img.shields.io/badge/Wazuh-005E85?style=flat-square)
![Sigma](https://img.shields.io/badge/Rules-Sigma-blue?style=flat-square)
![CVE Research](https://img.shields.io/badge/Research-CVE%20Analysis-critical?style=flat-square)

**🇬🇧 [English](#-english) &nbsp;|&nbsp; 🇧🇷 [Português](#-português)**

---

## 🇬🇧 English

Detection rule engineering and SIEM alerting logic — writing Sigma rules, building custom Wazuh alerts, analyzing Windows endpoint telemetry, and researching vulnerabilities in the SIEM stack itself.

### 🔎 Highlighted work

| Project | What it covers |
|---|---|
| [**CVE-2026-25769 — Wazuh Cluster RCE Analysis**](./Wazuh-SIEM-and-Custom-Alerts/Wazuh-CVE-2026-25769.md) | Deep-dive vulnerability research: root cause analysis (CWE-502, insecure deserialization via `as_wazuh_object()`), proof-of-concept, detection strategy, and hardening recommendations for a remote code execution flaw in Wazuh's cluster communication protocol |
| [**Ransomware Detection Engineering Lab**](./Sigma-Rules-and-Detection-as-Code/sigma-ransomware-detection-lab.md) | Reconstructing a ransomware attack chain, extracting IOCs, and authoring behavior-based Sigma detection rules mapped to adversary techniques |
| [**Sigma Language**](./Sigma-Rules-and-Detection-as-Code/sigma-language.md) | Sigma rule syntax and detection-as-code fundamentals |
| [**Custom Wazuh Alert Rules**](./Wazuh-SIEM-and-Custom-Alerts/wazuh-detection-rules.md) | Building custom alerting logic in Wazuh |
| [**Windows Event Viewer Investigation**](./Windows-Endpoint-Telemetry/investigating-with-the-event-viewer.md) | Investigating Windows Event Logs for suspicious activity |
| [**Windows Process Analysis**](./Windows-Endpoint-Telemetry/process-windows.md) | Analyzing Windows process telemetry |

### Skills demonstrated
Vulnerability research (CWE analysis, PoC development) · Sigma rule authoring · Wazuh SIEM administration & custom alerting · Windows endpoint telemetry analysis · Detection-as-Code

---

## 🇧🇷 Português

Engenharia de regras de detecção e lógica de alertas em SIEM — escrevendo regras Sigma, construindo alertas customizados no Wazuh, analisando telemetria de endpoint Windows e pesquisando vulnerabilidades na própria stack de SIEM.

### 🔎 Trabalho em destaque

| Projeto | O que cobre |
|---|---|
| [**CVE-2026-25769 — Análise de RCE no Cluster do Wazuh**](./Wazuh-SIEM-and-Custom-Alerts/Wazuh-CVE-2026-25769.md) | Pesquisa aprofundada de vulnerabilidade: análise de causa raiz (CWE-502, desserialização insegura via `as_wazuh_object()`), prova de conceito, estratégia de detecção e recomendações de hardening para uma falha de execução remota de código no protocolo de comunicação de cluster do Wazuh |
| [**Laboratório de Engenharia de Detecção de Ransomware**](./Sigma-Rules-and-Detection-as-Code/sigma-ransomware-detection-lab.md) | Reconstrução de uma cadeia de ataque de ransomware, extração de IOCs e criação de regras de detecção Sigma baseadas em comportamento, mapeadas a técnicas de adversário |
| [**Linguagem Sigma**](./Sigma-Rules-and-Detection-as-Code/sigma-language.md) | Sintaxe de regras Sigma e fundamentos de detection-as-code |
| [**Regras de Alerta Customizadas no Wazuh**](./Wazuh-SIEM-and-Custom-Alerts/wazuh-detection-rules.md) | Construção de lógica de alertas customizada no Wazuh |
| [**Investigação com o Visualizador de Eventos do Windows**](./Windows-Endpoint-Telemetry/investigating-with-the-event-viewer.md) | Investigação de logs de eventos do Windows em busca de atividade suspeita |
| [**Análise de Processos do Windows**](./Windows-Endpoint-Telemetry/process-windows.md) | Análise de telemetria de processos do Windows |

### Habilidades demonstradas
Pesquisa de vulnerabilidades (análise CWE, desenvolvimento de PoC) · Criação de regras Sigma · Administração e alertas customizados no Wazuh SIEM · Análise de telemetria de endpoint Windows · Detection-as-Code
