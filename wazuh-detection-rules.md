Engenharia de Deteção com Wazuh: Implementação de Regras e Decodificadores
Este repositório documenta a configuração técnica do Wazuh para monitorização de endpoints Windows e Linux. O foco reside na normalização de logs e na criação de uma hierarquia de regras para deteção de ameaças e redução de falsos positivos.

Sumário
Estruturação de Dados: Decodificadores Sysmon

Motor de Regras: Deteção de Anomalias em Processos

Hierarquia e Supressão: Uso de if_sid

Custom Rules: Monitorização de Integridade Linux (Auditd)

Cadeia de Deteção Progressiva e Exceções

1. Estruturação de Dados: Decodificadores Sysmon
Objetivo: Normalizar logs brutos do Windows Sysmon para análise estruturada.

Para que o Wazuh possa tomar decisões, utilizei decodificadores baseados em Regex que extraem metadados críticos. No caso do Sysmon (Event ID 1), o sistema executa:

Fase de Pré-decodificação: Extração de cabeçalhos (Timestamp, Hostname).

Fase de Decodificação: Identificação do ID do evento e mapeamento de campos como sysmon.image, sysmon.processId e sysmon.commandLine.

Esta estruturação permite que o analista realize buscas granulares no Dashboard, filtrando por processos específicos em vez de analisar texto bruto.

2. Motor de Regras: Deteção de Anomalias em Processos
Objetivo: Identificar técnicas de Evasão de Defesa através de lógica condicional.

Implementei regras para identificar o uso indevido de processos legítimos do sistema.

Exemplo Analítico: O processo svchost.exe é um alvo comum para Masquerading. Configurei regras para validar o "Processo Pai".

Deteção: Se o svchost.exe for iniciado pelo explorer.exe (utilizador), o Wazuh dispara um alerta de Nível 12 (Crítico), mapeado para a técnica T1055 (Process Injection) do MITRE ATT&CK.

3. Hierarquia e Supressão: Uso de if_sid
Objetivo: Otimizar a fidelidade dos alertas através do Tuning de SIEM.

Para evitar a "fadiga de alertas", utilizei a condição if_sid (If Signature ID). Esta técnica permite que uma regra dependa do disparo de outra para ser validada, criando uma árvore de decisão.

Lógica de Supressão: Desenvolvi uma regra de exceção que identifica quando o svchost.exe é iniciado pelo services.exe.

Resultado: O evento é classificado como Nível 0, sendo registado mas não gerando alerta visual para o SOC, garantindo foco apenas em anomalias reais.

XML
<rule id="184667" level="0">
    <if_sid>184666</if_sid> 
    <field name="sysmon.parentImage">\\services.exe</field>
    <description>Supressão: Comportamento legítimo do svchost.exe via services.exe.</description>
</rule>
4. Custom Rules: Monitorização de Integridade Linux (Auditd)
Objetivo: Extender a monitorização para o kernel Linux através de regras locais.

Personalizei o ficheiro local_rules.xml para processar logs do Auditd. O foco foi a monitorização de syscalls de criação de ficheiros em diretórios de alto risco.

Mapeamento: Técnica T1105 (Ingress Tool Transfer).

Implementação: Criei regras que monitorizam o campo audit.cwd à procura de palavras-chave como tmp, temp ou downloads, frequentemente utilizadas por atacantes para alojar binários maliciosos.

5. Cadeia de Deteção Progressiva e Exceções
Objetivo: Implementar um funil de filtragem multinível para resposta rápida.

Estabeleci uma cadeia de regras em local_rules.xml que escala a severidade conforme a evidência se torna mais específica:

Deteção de Localização: Alerta base para qualquer ficheiro criado em /tmp.

Deteção de Risco (Nível 12): Disparado se o ficheiro possuir extensões como .sh, .py ou .elf.

Deteção de Intenção: Identificação de strings suspeitas como shell ou linpeas no nome do ficheiro.

Tratamento de Exceções: Implementei uma regra de Nível 0 para ferramentas aprovadas (ex: malware-checker.py), demonstrando a capacidade de manter a segurança sem interromper operações legítimas de auditoria.

Conclusão
A implementação destas regras e decodificadores demonstra uma abordagem proativa na Engenharia de Deteção. Ao dominar a hierarquia do Wazuh, é possível transformar o SIEM numa ferramenta de alta precisão, capaz de distinguir entre administração de sistemas e atividades adversárias reais.
