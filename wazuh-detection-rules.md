# Engenharia de Deteção com Wazuh: Implementação de Regras e Decodificadores

Este repositório documenta a configuração técnica do Wazuh para monitorização de endpoints Windows e Linux. O foco reside na normalização de logs e na criação de uma hierarquia de regras para deteção de ameaças e redução de falsos positivos.

## Sumário
* Estruturação de Dados: Decodificadores Sysmon
* Motor de Regras: Deteção de Anomalias em Processos
* Hierarquia e Supressão: Uso de if_sid
* Custom Rules: Monitorização de Integridade Linux (Auditd)
* Cadeia de Deteção Progressiva e Exceções

---

## 1. Estruturação de Dados: Decodificadores Sysmon

**Objetivo:** Normalizar logs brutos do Windows Sysmon para análise estruturada.

Para que o Wazuh possa tomar decisões, utilizei decodificadores baseados em Regex que extraem metadados críticos. No caso do Sysmon (Event ID 1), o sistema executa:

* **Fase de Pré-decodificação:** Extração de cabeçalhos (Timestamp, Hostname).
* **Fase de Decodificação:** Identificação do ID do evento e mapeamento de campos como `sysmon.image`, `sysmon.processId` e `sysmon.commandLine`.

Esta estruturação permite que o analista realize buscas granulares no Dashboard, filtrando por processos específicos em vez de analisar texto bruto.

---

## 2. Motor de Regras: Deteção de Anomalias em Processos

**Objetivo:** Identificar técnicas de Evasão de Defesa através de lógica condicional.

Implementei regras para identificar o uso indevido de processos legítimos do sistema.

* **Exemplo Analítico:** O processo `svchost.exe` é um alvo comum para Masquerading. Configurei regras para validar o "Processo Pai".
* **Deteção:** Se o `svchost.exe` for iniciado pelo `explorer.exe` (utilizador), o Wazuh dispara um alerta de Nível 12 (Crítico), mapeado para a técnica T1055 (Process Injection) do MITRE ATT&CK.

---

## 3. Hierarquia e Supressão: Uso de if_sid

**Objetivo:** Otimizar a fidelidade dos alertas através do Tuning de SIEM.

Para evitar a "fadiga de alertas", utilizei a condição `if_sid` (If Signature ID). Esta técnica permite que uma regra dependa do disparo de outra para ser validada, criando uma árvore de decisão.

* **Lógica de Supressão:** Desenvolvi uma regra de exceção que identifica quando o `svchost.exe` é iniciado pelo `services.exe`.
* **Resultado:** O evento é classificado como Nível 0, sendo registado mas não gerando alerta visual para o SOC, garantindo foco apenas em anomalias reais.

```xml
<rule id="184667" level="0">
    <if_sid>184666</if_sid> 
    <field name="sysmon.parentImage">\\services.exe</field>
    <description>Supressão: Comportamento legítimo do svchost.exe via services.exe.</description>
</rule>
