# Ruptura Zero
Gestão de Estoque em Tempo Real

**Status:** Em Desenvolvimento
**Stack:** Kafka | PySpark | SQL Server | Power BI

## Visão Geral
Sistema de alta performance para prevenção de ruptura de estoque em eventos de varejo com alto tráfego. O projeto sincroniza vendas de múltiplos canais (App, Site, Lojas Físicas) em tempo real, garantindo a precisão do saldo de estoque em um Data Warehouse SQL Server.

## O Problema de Negócio
A venda de produtos sem estoque físico (overbooking) gera cancelamentos, insatisfação do cliente e prejuízo financeiro. Sistemas legados baseados em processamento batch (D-1) não suportam a velocidade de vendas de uma Black Friday, exigindo uma solução de baixa latência.

## Arquitetura da Solução
1.  **Ingestão (Producer):** Simulador Python gerando transações de 3 canais simultâneos.
2.  **Mensageria (Kafka):** Tópico "retail.checkout" atuando como buffer de alta vazão.
3.  **Processamento (PySpark Structured Streaming):**
    * Leitura de Stream e deserialização JSON.
    * Agregação de vendas por produto em janelas de tempo.
    * Cálculo de abatimento de estoque.
4.  **Serving (SQL Server):**
    * Modelagem Star Schema (Fato e Dimensão).
    * Atualização das tabelas Fato_Vendas e Dim_Produto via JDBC.

## Tecnologias
* **Python 3.10:** Geração de dados sintéticos.
* **Apache Kafka:** Broker de mensagens.
* **PySpark:** Processamento distribuído e streaming.
* **Microsoft SQL Server:** Armazenamento dimensional (Container Linux).
* **Docker Compose:** Orquestração do ambiente.

## Estrutura do Banco de Dados
* **Dim_Produto:** id, nome, categoria, preco_unitario, estoque_atual
* **Dim_Canal:** id, nome_canal
* **Fato_Vendas:** id_transacao, fk_produto, fk_canal, qtd, timestamp
