# Analise de Processos Nativos do Windows

Este repositorio contem a documentacao tecnica sobre a estrutura, comportamento e identificacao de anomalias nos processos essenciais do sistema operacional Windows. O foco e fornecer uma base solida para analise de endpoints e deteccao de ameacas (Threat Hunting) utilizando utilitarios nativos e ferramentas de monitoramento.

## 1. Ferramentas de Analise
Para a investigacao de processos, as seguintes ferramentas e comandos sao fundamentais:
* Gerenciador de Tarefas (Task Manager): Visualizacao de processos, PIDs, caminhos de imagem e linhas de comando.
* Linha de Comando: Uso de 'tasklist', 'Get-Process' (PowerShell) e 'wmic'.
* Analise de Hierarquia: Identificacao de relacoes pai-filho (PPID) para validar a legitimidade do processo.

## 2. Processos Core do Sistema e Comportamento Esperado

### System (PID 4)
* Descricao: Ambiente para threads de modo kernel.
* Caminho: ntoskrnl.exe
* Pai: Nenhum (System Idle Process 0).
* Anomalia: PID diferente de 4 ou multiplas instancias.

### smss.exe (Session Manager Subsystem)
* Descricao: Responsavel pela criacao de novas sessoes. E o primeiro processo em modo usuario iniciado pelo kernel.
* Pai: System (4).
* Anomalia: Processo pai diferente de System ou execucao fora de \System32.

### csrss.exe (Client Server Runtime Process)
* Descricao: Modo usuario do subsistema Windows. Gerencia janelas de console e threads.
* Pai: smss.exe (o pai se encerra apos o inicio).
* Sessoes: Uma instancia para a Sessao 0 e outra para a Sessao 1.

### wininit.exe (Windows Initialization)
* Descricao: Inicia o services.exe, lsass.exe e lsaiso.exe na Sessao 0.
* Anomalia: Multiplas instancias ou processo pai ativo.

### services.exe (Service Control Manager)
* Descricao: Gerencia o carregamento, interacao e finalizacao de servicos do sistema.
* Pai: wininit.exe.
* Anomalia: Nao ser executado como conta SYSTEM.

### svchost.exe (Service Host)
* Descricao: Hospeda servicos implementados como DLLs.
* Identificador: Deve sempre conter o parametro '-k' na linha de comando para agrupamento de servicos.
* Anomalia: Nome com erro ortografico (ex: scvhost.exe) ou ausencia do parametro '-k'.

### lsass.exe (Local Security Authority)
* Descricao: Responsavel pela aplicacao da politica de seguragem, autenticacao e tokens de acesso.
* Pai: wininit.exe.
* Anomalia: Multiplas instancias ou processo pai diferente de wininit.exe.

### winlogon.exe (Windows Logon)
* Descricao: Gerencia a sequencia de logon/logoff e a Sequencia de Atencao Segura (Ctrl+Alt+Del).
* Pai: smss.exe (pai inexistente em ferramentas de analise apos a inicializacao).

### explorer.exe (Windows Explorer)
* Descricao: Shell do usuario, fornecendo acesso a pastas, Menu Iniciar e Barra de Tarefas.
* Pai: userinit.exe (pai inexistente apos o inicio).
* Anomalia: Conexoes TCP/IP de saida incomuns ou execucao fora de \Windows.

## 3. Metodologia de Identificacao de Anomalias

Para validar a integridade de um processo, deve-se observar:
1. Nome do Processo: Verificar erros propositais de ortografia (Typosquatting).
2. Caminho da Imagem: Validar se o executavel esta no diretorio padrao do Windows.
3. Processo Pai: Confirmar se o ID do processo pai (PPID) condiz com a arvore genealogica do sistema.
4. Usuario: Verificar se processos criticos estao rodando sob a conta de servico correta (SYSTEM, Network Service, etc).
5. Linha de Comando: Analisar argumentos atipicos que iniciam scripts ou comandos maliciosos.

## 4. Estrutura do Projeto
* /network-investigation: Analise de trafego de rede associado a processos.
* /security-analytics: Dashboards e visualizacoes de comportamento de processos.
* /detection-engineering: Logica e queries para deteccao de anomalias.
* /siem-detections: Mapeamento de Event IDs de processos.
* /soc-lab-infrastructure: Detalhes do ambiente de laboratorio.
* /threat-intelligence: TTPs observados em processos maliciosos.

---
Documento: Relatorio_Analise_Processos_Windows.pdf
