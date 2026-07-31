# Windows Event Logs: Detection Engineering & Threat Hunting

![Status](https://img.shields.io/badge/Status-Completed-success)
![Focus](https://img.shields.io/badge/Focus-Detection%20Engineering-blue)
![Platform](https://img.shields.io/badge/Platform-Windows%20OS-lightgrey)

## Visão Geral
Este projeto documenta uma análise profunda sobre a arquitetura de processos do Windows e a engenharia de detecção baseada em logs de eventos (`.evtx`). O objetivo é demonstrar a capacidade de transformar dados brutos em inteligência operacional para um SOC (Security Operations Center), utilizando frameworks de mercado como o **MITRE ATT&CK**.

---

## 1. Baseline de Processos do Sistema
A detecção de anomalias começa pelo entendimento do comportamento legítimo. Esta matriz detalha os processos core do Windows e seus comportamentos esperados.

| Processo | PID Fixo | Parent Process | Path Padrão | Contexto de Execução |
| :--- | :---: | :--- | :--- | :--- |
| **System** | 4 | None (0) | `ntoskrnl.exe` | Kernel Mode |
| **smss.exe** | No | System (4) | `\System32\smss.exe` | Session Manager |
| **csrss.exe** | No | smss.exe | `\System32\csrss.exe` | User-mode Subsystem |
| **services.exe** | No | wininit.exe | `\System32\services.exe` | Service Control Manager |
| **svchost.exe** | No | services.exe | `\System32\svchost.exe` | Host de Serviços DLL |
| **lsass.exe** | No | wininit.exe | `\System32\lsass.exe` | Local Security Authority |

### Principais Anomalias Monitoradas
* **Masquerading:** Processos como `svchost.exe` executando sem o parâmetro `-k` ou com nomes incorretos (`scvhost.exe`).
* **Abuso de Privilégios:** Múltiplas instâncias de `lsass.exe` ou `services.exe` fora do diretório `System32`.

---

## 2. Engenharia de Detecção com PowerShell & XPath
Demonstração de proficiência em extração de dados utilizando consultas estruturadas para alta performance em grandes volumes de logs.

### Consultas de Hunting (Exemplos)

**A. Detecção de Criação de Usuário Alvo:**
```powershell
Get-WinEvent -LogName Security -FilterXPath '*/EventData/Data[@Name="TargetUserName"]="Sam" and */System/EventID=4720'

```

**B. Auditoria de Provedores Específicos (WLMS):**

```powershell
Get-WinEvent -LogName Application -FilterXPath '*/System/Provider[@Name="WLMS"] and */System/TimeCreated[@SystemTime="2020-12-15T01:09:08.940277500Z"]'

```

---

## 3. Matriz Estratégica de Event IDs (SIEM)

Configuração de regras baseadas em táticas do **MITRE ATT&CK**.

| Tática MITRE | Técnica | Event ID | Descrição |
| --- | --- | --- | --- |
| **Persistence** | Create/Modify Process | 7045 | Novo serviço instalado |
| **Defense Evasion** | Indicator Removal | 1102/104 | Logs de auditoria limpos |
| **Execution** | PowerShell Scripting | 4104 | Execução de Script Block (v5+) |
| **Privilege Esc.** | Account Manipulation | 4720/4724 | Criação ou reset de senha |
| **Discovery** | Account Discovery | 4799 | Enumeração de grupos locais |

---

## 4. Análise de Caso: Investigação Forense (`merged.evtx`)

Investigação prática de artefatos externos para reconstrução da cadeia de ataque.

### Cenário 1: Downgrade Attack (PowerShell)

* **Tática:** Tentativa de evadir monitoramento moderno forçando a versão 2.0.
* **Evidência:** **Event ID 400** detectado em `12/18/2020 07:50:33 AM`.

### Cenário 2: Detecção de Malware Emotet

* **Indicador:** Payload ofuscado identificado no log **PowerShell Operational (4104)**.
* **Artefato:** Variável identificada: `$Va5w3n8`.
* **Process ID:** `6620`.

### Cenário 3: Insider Threat & Enumeração

* **Ação:** Enumeração do grupo de Administradores Locais (SID `S-1-5-32-544`).
* **Evidência:** **Event ID 4799** via utilitário `net1.exe`.

---

## 5. Recomendações de Hardening e Visibilidade

Para garantir a eficácia das detecções acima, as seguintes GPOs foram validadas:

1. **Enable Script Block Logging:** Essencial para visibilidade de payloads ofuscados (ID 4104).
2. **Audit Process Creation:** Configurado para incluir a linha de comando (Command Line) no Event ID 4688.

---

## 📑 Conclusão

A análise técnica demonstra que a detecção eficiente não depende apenas de ferramentas, mas da capacidade do analista em correlacionar artefatos de sistema, identificar desvios de baseline e aplicar filtros de alta precisão.

---

**Analista:** Cauã

**Data:** 11 de Março de 2026


