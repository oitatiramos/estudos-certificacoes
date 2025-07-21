

# AWS Certified Developer – Associate (DVA-C02) Study Plan

Bem-vinda ao **Cronograma de Estudo para a Certificação AWS Certified Developer – Associate (DVA-C02)**! Este repositório contém um plano detalhado de 19 semanas (21 de julho a 28 de novembro de 2025) para ajudá-la a alcançar a certificação AWS Certified Developer – Associate com confiança. O foco está em desenvolver habilidades práticas com serviços AWS e reforçar o aprendizado com simulados.

## 🎯 Objetivo
Obter a certificação **AWS Certified Developer – Associate (DVA-C02)** no dia **28 de novembro de 2025**, dominando os principais serviços AWS e aplicando-os em um projeto prático.

## 🕒 Carga Horária
- **1 hora por dia**, de segunda a sexta (sugestão: 19h às 20h para consistência).
- **Período**: 21 de julho a 28 de novembro de 2025 (19 semanas).

## 💻 Tecnologias e Ferramentas
- **Linguagem**: Java
- **Serviços AWS**: API Gateway, Lambda, DynamoDB, SQS, SNS, AWS SAM, SDK Java
- **Ferramentas**:
  - AWS CLI
  - AWS SAM CLI
  - SDK Java (Maven/Gradle)
  - IDE (ex.: IntelliJ)
  - Postman
- **Conta AWS**: Nível gratuito (monitore custos com AWS Budgets)
- **Recursos de Estudo**:
  - Curso: Stephane Maarek (Udemy)
  - Simulados: Stephane Maarek (5 simulados), Tutorials Dojo (5 simulados)

## 🚀 Projeto Prático: Amigas na Estrada
Uma **API REST** para conectar mulheres que viajam sozinhas e desejam encontrar companheiras de viagem. Este projeto prático integra os principais serviços AWS e será desenvolvido ao longo do cronograma.

### Funcionalidades
- **POST /users**: Criar perfil de usuária.
- **GET /users/{destination}**: Consultar perfis compatíveis por destino/data.
- **POST /connections**: Enviar solicitação de conexão (enfileirada no SQS).
- **Notificações**: Enviar notificações de conexões via SNS.

### Serviços AWS Utilizados
- **API Gateway**: Endpoints REST
- **Lambda**: Lógica de backend
- **DynamoDB**: Banco de dados para perfis e viagens
- **SQS**: Fila para notificações assíncronas
- **SNS**: Notificações por e-mail
- **AWS SAM**: Implantação serverless

## 🛠️ Configuração do Ambiente
Antes de começar, configure o ambiente de desenvolvimento:

1. **AWS CLI**:
   - Instale a AWS CLI v2: [Download](https://aws.amazon.com/cli/)
   - Configure: `aws configure`
   - Teste: `aws sts get-caller-identity`

2. **AWS SAM CLI**:
   - Instale: [Instruções](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)
   - Teste: `sam --version`

3. **SDK Java**:
   - Configure um projeto Maven/Gradle com a dependência:
     ```xml
     <dependency>
         <groupId>software.amazon.awssdk</groupId>
         <artifactId>dynamodb</artifactId>
     </dependency>
     ```
   - Use a versão `software.amazon.awssdk:bom:2.20.0` ou mais recente.

4. **IDE**: IntelliJ (recomendado) ou outra IDE compatível com Java.

5. **Postman**: Instale para testar APIs.

6. **Conta AWS**: Crie uma conta gratuita em [aws.amazon.com/free](https://aws.amazon.com/free). Configure AWS Budgets para monitorar custos.

7. **Repositório Git**:
   - Crie um repositório local ou no GitHub: `git init aws-certification-projects`
   - Faça commits regulares para versionar o código.

## 📅 Estrutura do Cronograma
O cronograma está dividido em **19 semanas**, com foco em aprendizado teórico, prática hands-on e simulados para reforçar o conhecimento.

### Resumo das Semanas
- **Semanas 1–2**: Fundamentos AWS, configuração do ambiente e início do projeto Amigas na Estrada (Lambda, API Gateway, DynamoDB).
- **Semanas 3–4**: Integração com SQS, SNS e AWS SAM, finalizando o projeto.
- **Semanas 5–6**: Segurança (IAM, Cognito, KMS, Secrets Manager) e CI/CD (CodePipeline, CodeBuild).
- **Semana 7**: Otimização de Lambda e DynamoDB.
- **Semana 8**: Revisão geral do projeto e serviços.
- **Semanas 9–18**: Simulados intensivos (Maarek e Tutorials Dojo, alternados), com revisão de tópicos fracos.
- **Semana 19**: Preparação final, revisão do projeto e descanso antes da prova.

### Exemplo de Tarefa Semanal
**Semana 1, Segunda (21/07/2025)**:
- **Objetivo**: Entender fundamentos da AWS e configurar ambiente.
- **Tarefa**: Assista à seção "AWS Fundamentals" do curso de Maarek (20 min). Instale AWS CLI e SDK Java, configure um projeto Maven/Gradle (40 min).
- **Entregável**: AWS CLI configurado, projeto Java com SDK funcional.

Consulte o arquivo [aws-developer-study-plan.md](aws-developer-study-plan.md) para o cronograma completo.

## 📝 Simulados
- **Recursos**: 5 simulados de Stephane Maarek e 5 de Tutorials Dojo.
- **Estratégia**: A partir da Semana 9, faça um simulado por dia, alternando entre Maarek e Tutorials Dojo. Revise erros e estude tópicos fracos até atingir **≥80% de acertos** em cada simulado.
- **Cronograma de Simulados**:
  - Semanas 9–10: Primeira rodada (Simulados 1 a 5).
  - Semanas 11–18: Repetição dos simulados, focando em melhorar a pontuação.
  - Semana 19: Revisão final com questões selecionadas.

## 💡 Dicas Finais
- **Monitoramento de Custos**: Use AWS Budgets para evitar encargos. Exclua recursos após o projeto.
- **Versionamento**: Mantenha o código no Git. Exemplo: `git commit -m "Projeto Amigas na Estrada concluído"`.
- **Prova**: Confirme o exame para **28/11/2025** via Pearson VUE. Escolha o idioma português, mas esteja familiarizada com termos técnicos em inglês.
- **Descanso**: Reserve o fim de semana de 22–23/11 para descansar e revisar anotações leves.
- **Exportação**: Salve o cronograma como `aws-developer-study-plan.md`. Transfira via USB, e-mail ou GitHub. Converta para PDF com Pandoc, se desejar: `pandoc aws-developer-study-plan.md -o cronograma.pdf`.

## 📚 Como Usar Este Repositório
1. Clone o repositório: `git clone <URL-do-repositório>`
2. Abra o arquivo `aws-developer-study-plan.md` em um editor Markdown (VS Code, Obsidian).
3. Siga o cronograma semanal, salvando o progresso do projeto no diretório `projects/`.
4. Adicione notas e códigos no repositório e faça commits regulares.

Boa sorte na sua jornada para a certificação AWS! 🚴‍♀️ **Amigas na Estrada** está pronta para decolar, e você também!

