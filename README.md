# PoC — Teste de Desempenho de API REST com Apache JMeter

## 1. Objetivo

Este projeto consiste em uma Prova de Conceito (PoC) para avaliar o comportamento de uma API REST desenvolvida com Java e Spring Boot, utilizando o Apache JMeter para simular múltiplos usuários e coletar métricas de desempenho.

O objetivo foi configurar cenários de teste, executar requisições simultâneas, analisar os tempos de resposta e documentar os resultados obtidos.

## 2. Tecnologias utilizadas

- Java 24
- Spring Boot
- API REST
- MySQL
- Apache JMeter 5.6.3
- CSV para armazenamento dos resultados

## 3. Ambiente de testes

| Componente | Configuração |
|---|---|
| Processador | Intel Core i3-N305 |
| Memória RAM | 8 GB |
| Sistema operacional | Windows 11 64 bits |
| Java | 24.0.2 |
| Apache JMeter | 5.6.3 |

**Observação:** a API e o JMeter foram executados no mesmo computador. Portanto, os recursos da máquina foram compartilhados durante os testes.

## 4. Endpoint avaliado

Foi utilizado o endpoint responsável por consultar os contatos cadastrados:

```http
GET http://localhost:8080/contatos
```

## 5. Configuração dos cenários

Foram definidos três cenários de teste:

| Cenário | Usuários simultâneos | Requisições por usuário | Total |
|---|---:|---:|---:|
| Teste 1 | 1 | 10 | 10 |
| Teste 2 | 5 | 10 | 50 |
| Teste 3 | 10 | 10 | 100 |

Nos cenários com 5 e 10 usuários, foi utilizado o **Synchronizing Timer** para sincronizar a liberação das requisições em grupos.

O objetivo foi observar o comportamento da API diante de diferentes níveis de concorrência.

O cenário com 10 usuários foi executado três vezes para permitir a comparação entre execuções.

## 6. Resultados obtidos

Os resultados foram extraídos dos arquivos CSV gerados pelo JMeter e conferidos com os relatórios agregados.

### 6.1. Teste com 1 usuário

| Métrica | Resultado |
|---|---:|
| Requisições | 10 |
| Média | 23,5 ms |
| Mínimo | 11 ms |
| Máximo | 105 ms |
| Erros | 0% |

### 6.2. Teste com 5 usuários simultâneos

| Métrica | Resultado |
|---|---:|
| Requisições | 50 |
| Média | 33,50 ms |
| Mediana | 14 ms |
| P90 | 32 ms |
| P95 | 189 ms |
| P99 | 202 ms |
| Mínimo | 10 ms |
| Máximo | 202 ms |
| Erros | 0% |

### 6.3. Teste com 10 usuários simultâneos

Foram realizadas três execuções, cada uma com 100 requisições.

| Métrica | Execução 1 | Execução 2 | Execução 3 |
|---|---:|---:|---:|
| Média | 36,69 ms | 40,23 ms | 25,82 ms |
| Mediana | 21 ms | 18 ms | 22 ms |
| P90 | 31 ms | 35 ms | 40 ms |
| P95 | 183 ms | 242 ms | 67 ms |
| P99 | 189 ms | 246 ms | 77 ms |
| Mínimo | 11 ms | 9 ms | 10 ms |
| Máximo | 192 ms | 246 ms | 91 ms |
| Erros | 0% | 0% | 0% |

## 7. Análise dos resultados

Nos três cenários, todas as requisições receberam resposta HTTP 200, sem erros registrados pelo JMeter.

Nos testes com 5 e 10 usuários, foi possível observar que a maior parte das requisições apresentou tempos de resposta baixos. Entretanto, algumas respostas iniciais tiveram tempos significativamente maiores, influenciando a média e os percentis superiores.

As três execuções com 10 usuários também apresentaram variações entre si, principalmente nos percentis P95 e P99. Isso demonstra a importância de repetir os testes e analisar diferentes métricas, em vez de considerar apenas a média.

Não foi possível determinar, somente com esses dados, a causa das variações observadas.

## 8. Limitações da PoC

- Os testes foram realizados em ambiente local.
- A API e o JMeter compartilharam os recursos do mesmo computador.
- Foi avaliado apenas o endpoint GET `/contatos`.
- A quantidade de usuários e requisições foi limitada aos cenários definidos.
- Os resultados não representam, isoladamente, a capacidade de atendimento em ambiente de produção.

## 9. Estrutura do projeto

```text
POC_JMETER/
│
├── poc-api-contatos.jmx
│
├── resultados/
│   ├── baseline_1_usuario.csv
│   ├── teste_5_usuarios_final.csv
│   ├── teste_10_usuarios.csv
│   ├── teste_10_usuarios_repeticao_2.csv
│   └── teste_10_usuarios_repeticao_3.csv
│
└── README.md
```

## 10. Conclusão

A PoC permitiu aplicar conceitos de testes de desempenho em uma API REST, utilizando o Apache JMeter para simular usuários simultâneos e coletar métricas de resposta.

Além da execução dos cenários, o trabalho envolveu configuração do plano de testes, armazenamento dos resultados em CSV, comparação entre execuções e análise das métricas obtidas.

O projeto demonstra uma aplicação prática de testes de desempenho e documentação técnica, servindo como base para experimentos futuros com cenários mais amplos e ambientes controlados.