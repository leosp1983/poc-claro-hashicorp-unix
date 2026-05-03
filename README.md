# poc-claro-hashicorp-unix

POC de automação Ansible para servidores Unix, integrada ao **HashiCorp Vault SSH Secrets Engine** e ao **Ansible Automation Platform (AAP)**.

## Visão Geral

Este projeto automatiza a configuração de servidores Linux usando **Chaves SSH Assinadas** pelo Vault. Diferente do método tradicional, o projeto gera uma chave temporária, solicita ao Vault que a assine e usa o certificado resultante para conexão root, eliminando a necessidade de gerenciar senhas ou chaves estáticas no AAP.

## O que o Playbook Faz

1.  **Geração de Chave**: Cria um par de chaves SSH temporário no runner do AAP.
2.  **Assinatura Vault**: Autentica via AppRole e assina a chave pública no backend `ssh/sign/ansible-role`.
3.  **Configuração de Conexão**: Define dinamicamente o uso do certificado assinado para o grupo `unix`.
4.  **Criação de Usuário**: Cria o usuário definido em `vars/main.yaml`.
5.  **Criação de Diretório**: Garante a existência da pasta de relatórios.
6.  **Validação de Serviço**: Verifica o status do serviço `sshd.service`.
7.  **Relatório**: Gera um log formatado e o exibe no console (sem acentos para compatibilidade).

## Estrutura do Projeto

```
poc-claro-hashicorp-unix/
├── roles/
│   └── unix_server/
│       ├── tasks/
│       │   └── main.yaml      # Configuração do servidor e relatórios
│       └── vars/
│           └── main.yaml      # Variáveis (User, AppRole, SSH Service)
├── ansible.cfg                # Configuração do Ansible
├── unix_automation.yaml       # Playbook (Play 1: SSH Sign | Play 2: Role)
└── README.md
```

## Configurações Necessárias (vars/main.yaml)

Para que a assinatura funcione, o AppRole deve estar configurado no Vault:

- `vault_role_id`: ID da role no Vault.
- `vault_secret_id`: Secret ID da role.
- `unix_service_name`: Definido como `sshd`.

## Execução no AAP

1.  Importe este repositório no AAP.
2.  **Importante**: Não é necessária uma credencial de Machine/SSH do AAP para o host final, pois as chaves são geradas e assinadas em tempo de execução.
3.  O inventário deve conter um grupo chamado `unix`.
4.  Execute o Job Template usando o playbook `unix_automation.yaml`.