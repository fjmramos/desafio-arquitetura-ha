Documentação da Arquitetura - Solução On-Premises

1. Visão Geral

Este documento detalha a arquitetura de alta disponibilidade proposta para uma aplicação de e-commerce fictícia, utilizando componentes on-premises para garantir resiliência e segurança.


2. Componentes da Solução

Nginx (API Gateway): Gerencia o roteamento das requisições e a segurança na borda (SSL/TLS).


Redis (Read Cache): Camada de cache em memória para acelerar a leitura de dados frequentes.


Kafka (Messaging): Barramento de mensagens para processamento assíncrono e comunicação entre serviços.


PostgreSQL (Data Persistence): Banco de dados relacional para persistência de dados críticos.

3. Estratégias de Alta Disponibilidade (HA) Para garantir que não haja um ponto único de falha:

Nginx: Implementado em par (Ativo/Passivo) com Keepalived e um IP Virtual (VIP).

Redis: Configurado com um nó Primary e um Replica, monitorados por um cluster de Redis Sentinel para failover automático.

Kafka: Cluster distribuído com 3 Brokers, garantindo a disponibilidade das mensagens mesmo com a queda de um nó.

PostgreSQL: Utilização de Streaming Replication entre um nó Master e um nó Standby.

4. Segurança

Segregação de Rede: Divisão entre zona pública (DMZ) para o Gateway e rede privada (Internal Private Network) para os dados.

Firewall: Implementação de ACLs permitindo apenas o tráfego necessário entre as camadas.

Criptografia: Todo o tráfego externo é protegido via HTTPS/TLS.

5. Operações de Dia 2 (Ansible)

Foi desenvolvido um Playbook Ansible para automatizar tarefas de rotina do PostgreSQL, incluindo:

Verificação de status do serviço.

Execução de backup completo do banco de dados (pg_dump).

Limpeza automática de logs e backups antigos.
