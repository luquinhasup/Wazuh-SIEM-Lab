# 🛡️ SOC Lab: Monitoramento de Endpoints e Detecção de Intrusão com Wazuh SIEM

## 🎯 Objetivo do Projeto
Este laboratório prático foi desenvolvido para demonstrar a implementação de um ambiente de Security Information and Event Management (SIEM) utilizando o **Wazuh**. O foco do projeto é a configuração de arquitetura de monitoramento, ingestão de logs e a detecção de anomalias focadas em tentativas de escalonamento de privilégio local.

## 🏗️ Topologia e Arquitetura do Laboratório
[cite_start]A fim de evitar problemas de *crash* por emulação aninhada (virtualização dentro de virtualização) ao utilizar imagens do Windows, o ambiente foi planejado com sistemas baseados em Linux em máquinas virtuais distintas[cite: 524, 556]:
* [cite_start]**Wazuh Manager / Indexer / Dashboard:** Hospedado em um servidor Ubuntu Server 26.04[cite: 69, 522].
* [cite_start]**Wazuh Agent (Endpoint Monitorado):** Hospedado em uma máquina virtual Kali Linux[cite: 236, 523].

## ⚔️ Simulação de Ataque (Red Team)
Para validar a eficácia do monitoramento, simulei um ataque de Força Bruta / Escalonamento de Privilégios no endpoint monitorado.
* **Vetor:** Acesso via terminal local no Kali Linux.
* [cite_start]**Ação:** Execução do comando `su root` com a injeção proposital de senhas incorretas consecutivas (4 tentativas falhas)[cite: 558].

## 🔎 Detecção e Análise de Logs (Blue Team)
[cite_start]O agente do Wazuh no Kali Linux capturou as tentativas de intrusão em tempo real e encaminhou os logs para o Manager, disparando os seguintes alertas no dashboard [cite: 379-446]:
* [cite_start]**Rule ID 5503:** `PAM: User login failed`[cite: 386, 393].
* [cite_start]**Rule ID 5301:** `User missed the password to change UID`[cite: 385, 390].
* [cite_start]**Rule ID 5557:** `unix_chkpwd: Password check failed`[cite: 395, 397].

### Mapeamento MITRE ATT&CK
[cite_start]O SIEM correlacionou automaticamente os eventos de falha de autenticação com o framework global de ameaças MITRE ATT&CK[cite: 449]:
* [cite_start]**Tática Identificada:** Credential Access[cite: 520].

<img width="1710" height="1107" alt="Captura de Tela 2026-06-09 às 02 03 25" src="https://github.com/user-attachments/assets/45eac756-e715-43a0-b954-28d2b41435aa" />


## 🛠️ Troubleshooting e Lições Aprendidas
[cite_start]Durante a implantação, enfrentei um desafio técnico com um `ERROR 403: Forbidden` ao tentar instalar o Wazuh Manager em uma infraestrutura Linux pré-existente[cite: 554, 555]. 
[cite_start]Para solucionar o incidente e não travar o projeto, apliquei conceitos práticos de administração de sistemas: isolei o ambiente, provisionei uma nova máquina virtual limpa especificamente para o SIEM e subi os serviços do zero com sucesso[cite: 557]. [cite_start]O processo completo de *troubleshooting* e configuração levou cerca de 2h30[cite: 559], consolidando fortemente os conceitos de arquitetura de servidores e *log management*.
