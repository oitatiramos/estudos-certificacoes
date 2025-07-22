# 📚 Plano de Estudos – AWS Certified Developer Associate (DVA-C02)

## 🗓️ Período: 21 de julho até 28 de novembro de 2025  
## 🧠 Carga diária: 1h de estudo  
## 🚀 Projeto prático: Amigas na Estrada (API em Java)

---

## 🎯 Objetivo
Conquistar a certificação **AWS Developer Associate (DVA-C02)** com domínio prático sobre:

- **API Gateway**
- **AWS Lambda**
- **DynamoDB**
- **SQS e SNS**
- **AWS SAM**
- **AWS SDK (Java)**

---

## 📘 Curso Base: Stephane Maarek  
## 🧪 Simulados: Stephane Maarek + Tutorials Dojo (12 no total)  
## 🛠️ Projeto de apoio: *Amigas na Estrada* – API para conectar mulheres que viajam sozinhas

---

## ✅ Semana 1 – 21 a 27 de julho  
**Foco:** Introdução à AWS, IAM, CLI, SDK

| Dia | Aula (Stephane Maarek) | Tarefa prática no projeto |
|-----|------------------------|----------------------------|
| 21/07 (seg) | Visão geral do exame | Criar README com o plano de estudos |
| 22/07 (ter) | IAM: usuários, grupos e políticas | Criar política de IAM para uso no projeto |
| 23/07 (qua) | IAM: roles, security best practices | Criar Role para execução de Lambda |
| 24/07 (qui) | AWS CLI | Instalar CLI e testar comandos `aws sts get-caller-identity` |
| 25/07 (sex) | AWS SDK (Java) parte 1 | Configurar SDK no projeto e listar buckets S3 |
| 26/07 (sab) | AWS SDK (Java) parte 2 | Criar chamada Java para DynamoDB local (stub) |
| 27/07 (dom) | Revisão semanal | Escrever no README o que aprendeu na semana |

---

## ✅ Semana 2 – 28 de julho a 3 de agosto  
**Foco:** Lambda + primeiros testes com SAM

| Dia | Aula (Stephane Maarek) | Tarefa prática no projeto |
|-----|------------------------|----------------------------|
| 28/07 (seg) | AWS Lambda – visão geral | Criar função Lambda simples com retorno estático |
| 29/07 (ter) | Lambda – triggers e runtime | Criar trigger por API Gateway |
| 30/07 (qua) | Lambda com Java | Criar Lambda usando Maven e Java no projeto |
| 31/07 (qui) | Lambda com SDK | Invocar Lambda via SDK no seu projeto |
| 01/08 (sex) | Lambda e variáveis de ambiente | Usar env vars para configuração de ambiente |
| 02/08 (sab) | AWS SAM – primeiros passos | Instalar SAM CLI e rodar `sam init` |
| 03/08 (dom) | Revisão + commit semanal | Testar `sam build` e `sam local invoke` |

---

## ✅ Semana 3 – 4 a 10 de agosto  
**Foco:** API Gateway

| Dia | Aula (Stephane Maarek) | Tarefa prática no projeto |
|-----|------------------------|----------------------------|
| 04/08 | API Gateway - tipos e integração | Criar API REST no API Gateway |
| 05/08 | API Gateway + Lambda | Integrar endpoint com Lambda |
| 06/08 | API Gateway com autenticação | Configurar Auth por chave de API |
| 07/08 | Mapping templates | Criar template para entrada/saída de JSON |
| 08/08 | SDK + chamada externa | Chamar o endpoint via Java SDK |
| 09/08 | SAM + deploy de API | Criar template SAM com Lambda + API Gateway |
| 10/08 | Revisão semanal | Desenhar arquitetura no README |

---

## ✅ Semana 4 – 11 a 17 de agosto  
**Foco:** DynamoDB

| Dia | Aula (Stephane Maarek) | Tarefa prática no projeto |
|-----|------------------------|----------------------------|
| 11/08 | DynamoDB – conceitos básicos | Criar tabela `usuarios_viagem` |
| 12/08 | Operações CRUD | Criar função para salvar usuário |
| 13/08 | Index secundário | Criar GSI para buscas por destino |
| 14/08 | Paginação e limites | Adicionar busca paginada |
| 15/08 | SDK Java com DynamoDB | Criar camada DAO usando o SDK |
| 16/08 | SAM com DynamoDB | Atualizar template SAM com permissão à tabela |
| 17/08 | Revisão semanal | Testar integração fim a fim |

---

## ✅ Semana 5 – 18 a 24 de agosto  
**Foco:** SQS e SNS

| Dia | Aula (Stephane Maarek) | Tarefa prática no projeto |
|-----|------------------------|----------------------------|
| 18/08 | SQS – visão geral | Criar fila para mensagens entre usuárias |
| 19/08 | Envio e leitura de mensagens | Criar Lambda para consumir mensagens da fila |
| 20/08 | Dead Letter Queue | Criar DLQ para a fila principal |
| 21/08 | SNS – visão geral | Criar tópico para notificações |
| 22/08 | Subscrição por e-mail | Testar envio real de e-mail |
| 23/08 | SDK Java com SQS e SNS | Criar classe que publica e lê mensagens via SDK |
| 24/08 | Revisão semanal | Atualizar arquitetura do projeto |

---

## ✅ Semana 6 – 25 a 31 de agosto  
**Foco:** SAM avançado + Deploy

| Dia | Aula (Stephane Maarek) | Tarefa prática no projeto |
|-----|------------------------|----------------------------|
| 25/08 | SAM + variáveis e profiles | Adicionar `env.json` ao projeto |
| 26/08 | SAM Deploy + Pipeline | Fazer deploy manual com `sam deploy` |
| 27/08 | Debugging com SAM | Rodar `sam logs` e interpretar erros |
| 28/08 | Templates avançados | Modularizar templates |
| 29/08 | API + Lambda + DynamoDB com SAM | Juntar tudo em um único template |
| 30/08 | Simulado 1 (Maarek) | Analisar erros e revisar aulas |
| 31/08 | Revisão + pequenos ajustes | Documentar funções no projeto |

---

## ✅ Setembro a novembro – 1º set a 24 nov  
**Foco:** Simulados, revisões, ajustes no projeto

- Fazer **3 simulados por mês**
- Intercalar entre **Maarek** e **Tutorials Dojo**
- Revisar os erros no dia seguinte
- Corrigir e aprimorar o projeto com base nos tópicos fracos
- Preparar documentação técnica da aplicação

---

## 📅 Semana Final – 25 a 28 de novembro

| Dia | Atividade |
|-----|-----------|
| 25/11 (ter) | Simulado final |
| 26/11 (qua) | Revisão de erros comuns |
| 27/11 (qui) | Revisão geral de arquitetura e conceitos-chave |
| 28/11 (sex) | ✅ Prova DVA-C02 🎉 |

---

## 🧠 Dicas finais

- Foque na prática constante com seu projeto.
- Use o SDK Java para reforçar o entendimento de cada serviço.
- Marque os erros recorrentes nos simulados e revise sempre.
- Atualize este README semanalmente com aprendizados.

---

Vamos com tudo, Tati! 🚀💜  
Você vai arrasar na prova e levar seu projeto a outro nível!
