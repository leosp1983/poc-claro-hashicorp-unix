# poc-claro-hashicorp-unix

POC de automação Ansible para servidores Unix, integrada ao **HashiCorp Vault** e ao **Ansible Automation Platform (AAP)**.

## Visão Geral

Este projeto automatiza a criação de usuários, diretórios e validação de serviços em servidores Linux/Unix. As credenciais são injetadas pelo AAP por meio de integração nativa com o HashiCorp Vault.

## O que o Playbook Faz

1. **Cria um usuário** no servidor (`usr_poc_unix`).
2. **Cria um diretório** de relatórios (`/opt/reports/poc_hashicorp`).
3. **Valida o serviço do Vault** (verifica se está rodando via `service_facts`).
4. **Gera um arquivo de relatório** detalhado.
5. **Exibe o relatório** no console do Ansible de forma organizada.

## Estrutura do Projeto

```
poc-claro-hashicorp-unix/
├── roles/
│   └── unix_server/
│       ├── tasks/
│       │   └── main.yaml      # Tarefas principais
│       └── vars/
│           └── main.yaml      # Variaveis (usuario, caminhos, servico)
├── ansible.cfg                # Configuração do Ansible
├── unix_automation.yaml       # Playbook principal
└── README.md
```

## Execução no AAP

1. Importe este repositório no AAP.
2. Utilize uma credencial do tipo **Machine** configurada com lookup no HashiCorp Vault.
3. Crie um **Job Template** usando o playbook `unix_automation.yaml`.
4. Defina o host alvo (`10.10.10.7`).
5. Execute o Job.