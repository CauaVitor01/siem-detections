# Detection Engineering: Padronização e Automação com Linguagem Sigma

Este repositório apresenta a implementação de metodologias de detecção agnósticas utilizando a linguagem Sigma. O foco é a criação de assinaturas estruturadas para eventos de log, permitindo a portabilidade de detecções entre diferentes plataformas SIEM (Elasticsearch, Splunk, QRadar, etc.).

---

## Visão Geral da Linguagem Sigma
O Sigma atua como um padrão de mercado para logs, equivalente ao que o Snort representa para tráfego de rede. Sua principal função é permitir que analistas de SOC descrevam comportamentos maliciosos em um formato estruturado (YAML), facilitando o compartilhamento de inteligência e evitando a dependência de fornecedores (Vendor Agnostic).



---

## Arquitetura de Regras e Sintaxe
As detecções são construídas sobre fundamentos de serialização YAML, garantindo legibilidade e precisão técnica.

### Componentes Críticos
* **Logsource:** Definição precisa do escopo de aplicação (Produto, Categoria e Serviço).
* **Detection Logic:** Uso de identificadores em listas (OR) e mapas (AND) para descrever a atividade suspeita.
* **Value Modifiers:** Aplicação de transformadores como `contains`, `startswith` e expressões regulares (`re`) para maior refinamento.
* **Condition:** Definição da lógica final de correspondência (Ex: `selection and not filter`).



---

## Ciclo de Vida do Desenvolvimento (Case Study: AnyDesk)
O projeto documenta o processo analítico de transformar inteligência bruta em detecção ativa, utilizando o cenário de instalação não autorizada da ferramenta AnyDesk para persistência e C2.

### Etapas de Implementação
1. **Intel Analysis:** Identificação de flags de instalação (`--install`, `--silent`) e caminhos de execução suspeitos (`C:\ProgramData\`).
2. **Rule Authoring:** Tradução dos artefatos para sintaxe Sigma com foco em `process_creation`.
3. **Conversion:** Utilização de ferramentas como **Sigmac CLI** e **Uncoder.io** para tradução para sintaxes específicas (Lucene, SPL, KQL).
4. **Validation:** Execução de Threat Hunting no SIEM para validação de Hits e ajuste de Falsos Positivos.



---

## Conversão e Fidelidade de Backend
O repositório aborda as nuances entre diferentes conversores e a importância do mapeamento correto de campos (Field Mapping).
* **Sigmac CLI:** Foco em mapeamentos de configuração locais e customizados.
* **Uncoder.io:** Foco em padrões modernos como o ECS (Elastic Common Schema).



---

## Tecnologias Aplicadas
* **Languages:** Sigma (YAML), Lucene, KQL, SPL.
* **Tools:** Sigmac CLI, Uncoder.io, Python.
* **SIEM Platforms:** Elastic Stack, Splunk, QRadar.
* **Frameworks:** MITRE ATT&CK.

---
**Blue Team Portfolio | Detection Engineering & Threat Intelligence**
