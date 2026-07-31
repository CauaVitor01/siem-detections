# Ransomware Detection Engineering Lab

Este repositório documenta um laboratório prático de **engenharia de detecção**, focado na análise da cadeia de ataque de um ransomware e na criação de regras de detecção utilizando **Sigma**.

O objetivo do projeto é demonstrar como eventos de segurança podem ser analisados, correlacionados e transformados em **detecções acionáveis** dentro de um ambiente de Security Operations Center (SOC).

---

# Objetivo do Projeto

Este laboratório simula o trabalho de um **Analista de Segurança ou Detection Engineer** após um incidente de ransomware.

A partir da análise da atividade do adversário, foram identificadas técnicas maliciosas e criadas regras de detecção baseadas em comportamento para identificar atividades semelhantes em ambientes corporativos.

O projeto demonstra:

* Reconstrução da cadeia de ataque
* Identificação de Indicadores de Comprometimento (IOCs)
* Criação de regras de detecção
* Mapeamento de técnicas adversárias
* Padronização de detecções utilizando Sigma

---

# Cenário do Ataque

O laboratório simula um incidente de ransomware iniciado por **phishing**, seguido pela execução de um payload malicioso no endpoint da vítima.

A partir desse ponto, o atacante executa uma sequência de ações para manter persistência, escalar privilégios, coletar dados e finalmente criptografar os arquivos da vítima.

A análise do incidente permitiu identificar as seguintes etapas do ataque:

1. Phishing inicial
2. Execução do payload malicioso
3. Download de malware adicional
4. Estabelecimento de shell reverso
5. Escalação de privilégios
6. Persistência no sistema
7. Coleta de informações do sistema
8. Exfiltração de dados
9. Criptografia de arquivos (ransomware)

---

# Técnicas Observadas

Durante a análise do incidente foram identificadas diversas ferramentas e técnicas utilizadas pelo atacante.

Algumas delas incluem:

* `certutil` para download de payloads
* `netcat` para criação de shell reverso
* `PowerUp.ps1` para escalonamento de privilégios
* `curl` para transferência de dados
* `7zip` para compressão de arquivos antes da exfiltração

Essas técnicas são frequentemente classificadas como **Living off the Land**, onde o adversário utiliza ferramentas nativas do sistema operacional para evitar detecção.

---

# Engenharia de Detecção

A partir da análise da atividade maliciosa, foram desenvolvidas regras de detecção utilizando **Sigma**, uma linguagem padronizada que permite a criação de regras portáveis entre diferentes plataformas de SIEM.

As regras foram projetadas para identificar comportamentos suspeitos como:

* downloads de arquivos executáveis
* execução de utilitários administrativos com parâmetros suspeitos
* criação de conexões de shell reverso
* acesso incomum a arquivos sensíveis

As regras Sigma podem ser convertidas para diferentes plataformas de monitoramento de segurança.

---

# Mapeamento MITRE ATT&CK

As técnicas observadas durante o ataque foram mapeadas para o framework **MITRE ATT&CK**, permitindo contextualizar o comportamento do adversário dentro de um modelo amplamente utilizado em operações de segurança.

Exemplos de técnicas identificadas:

| Técnica                                       | ID    |
| --------------------------------------------- | ----- |
| Ingress Tool Transfer                         | T1105 |
| Command and Scripting Interpreter             | T1059 |
| Exfiltration Over Command and Control Channel | T1041 |
| Data Encrypted for Impact                     | T1486 |

---

# Detecções Implementadas

As seguintes detecções foram desenvolvidas durante o laboratório:

* Detecção de download de payload via certutil
* Detecção de execução de netcat para shell reverso
* Detecção de uso de ferramentas de privilege escalation
* Detecção de compressão de arquivos para exfiltração
* Detecção de atividade relacionada a ransomware

Cada detecção inclui:

* regra Sigma
* mapeamento MITRE ATT&CK
* evidências do laboratório
* análise do comportamento malicioso

---

# Ferramentas Utilizadas

O laboratório utilizou as seguintes tecnologias e ferramentas de segurança:

* SIEM para análise de logs
* Sigma para criação de regras de detecção
* Ambiente Windows para geração de eventos
* Ferramentas de análise de logs

---

# Estrutura do Projeto

```
ransomware-detection-lab

docs
 ├── incident-analysis
 ├── sigma-rules
 ├── detection-notes

images
 ├── detection-evidence
 ├── logs
 ├── rules
```

---

# Conclusão

Este laboratório demonstra como eventos de segurança podem ser analisados e transformados em detecções práticas dentro de um ambiente SOC.

A combinação de análise de incidentes, mapeamento de técnicas adversárias e desenvolvimento de regras de detecção permite fortalecer a capacidade defensiva de uma organização e melhorar a identificação precoce de atividades maliciosas.

---

# Autor

Analista de Segurança (Blue Team)
Foco em:

* Security Operations Center (SOC)
* Threat Hunting
* Detection Engineering
* Análise de Logs
