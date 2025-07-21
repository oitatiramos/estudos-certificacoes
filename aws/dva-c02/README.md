

# Cronograma de Estudo para AWS Certified Developer – Associate (DVA-C02)

**Período**: 21 de julho a 28 de novembro de 2025 (19 semanas)  
**Objetivo**: Obter a certificação AWS Certified Developer – Associate (DVA-C02) em 28 de novembro de 2025  
**Carga Horária**: 1 hora por dia, de segunda a sexta (ex.: 19h às 20h, horário sugerido para consistência)  
**Linguagem**: Java  
**Foco**: API Gateway, Lambda, DynamoDB, SQS, SNS, AWS SAM, SDK Java  
**Recursos**:  
- **Curso**: Stephane Maarek (Udemy).  
- **Simulados**: Stephane Maarek (5 simulados), Tutorials Dojo (5 simulados).  
- **Conta AWS**: Nível gratuito (monitore custos via AWS Budgets).  
- **Ferramentas**: AWS CLI, AWS SAM CLI, SDK Java (Maven/Gradle), IDE (ex.: IntelliJ), Postman.  
- **Exportação**: Salve este arquivo como `aws-developer-study-plan.md`. Transfira para outro computador via USB, e-mail ou repositório Git (ex.: GitHub). Visualize em editores Markdown (VS Code, Obsidian) ou converta para PDF com Pandoc (`pandoc aws-developer-study-plan.md -o cronograma.pdf`).

## Pré-requisitos de Configuração
1. **AWS CLI**: Instale a AWS CLI v2 (`aws configure` para configurar credenciais).  
   - Download: https://aws.amazon.com/cli/  
   - Teste: `aws sts get-caller-identity`  
2. **AWS SAM CLI**: Instale para implantação de projetos serverless.  
   - Download: https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html  
   - Teste: `sam --version`  
3. **SDK Java**: Configure um projeto Maven/Gradle com dependência `software.amazon.awssdk:bom:2.20.0` (ou versão mais recente). Exemplo (Maven):  
   ```xml
   <dependency>
       <groupId>software.amazon.awssdk</groupId>
       <artifactId>dynamodb</artifactId>
   </dependency>
   ```  
4. **IDE**: Use IntelliJ ou outra IDE para desenvolvimento Java.  
5. **Postman**: Instale para testar APIs.  
6. **Conta AWS**: Crie uma conta gratuita (https://aws.amazon.com/free). Configure AWS Budgets para evitar custos.  
7. **Repositório**: Crie um repositório local ou no GitHub para organizar os projetos.  
   - Exemplo: `git init aws-certification-projects`  
   - Commit regularmente para facilitar transferência entre computadores.

## Projeto Prático
**Amigas na Estrada**: Uma API REST para conectar mulheres que viajam sozinhas e desejam encontrar companheiras de viagem.  
- **Serviços AWS**: API Gateway (endpoints REST), Lambda (lógica de backend), DynamoDB (banco de dados para perfis e viagens), SQS (fila para notificações assíncronas), SNS (notificações por e-mail), AWS SAM (implantação serverless).  
- **Funcionalidades**:  
  - Criar perfil de usuária (POST `/users`).  
  - Consultar perfis compatíveis por destino/data (GET `/users/{destination}`).
  - Enviar solicitação de conexão (POST `/connections`, enfileirada no SQS).  
  - Notificar usuárias sobre conexões via SNS.  

## Cronograma Detalhado

### Semana 1 (21 a 25 de julho de 2025): Fundamentos e Configuração
- **Segunda, 21/07**  
  **Objetivo**: Entender fundamentos da AWS e configurar ambiente.  
  **Tarefa**: Assista à seção "AWS Fundamentals" do curso de Stephane Maarek (20 min). Instale e configure AWS CLI e SDK Java (Maven/Gradle) em um projeto Java (40 min). Teste com `aws configure` e crie um projeto com dependência `software.amazon.awssdk:dynamodb`.  
  **Entregável**: AWS CLI configurado, projeto Java com SDK funcional.  

- **Terça, 22/07**  
  **Objetivo**: Aprender IAM e permissões.  
  **Tarefa**: Assista à seção "IAM" do curso de Maarek (20 min). Crie um usuário IAM com permissões para Lambda e DynamoDB via AWS Console (40 min). Teste com `aws sts get-caller-identity`.  
  **Entregável**: Usuário IAM configurado e testado.  

- **Quarta, 23/07**  
  **Objetivo**: Entender AWS Lambda.  
  **Tarefa**: Assista à seção "Lambda" do curso de Maarek (20 min). Crie uma função Lambda em Java ("Hello World") via AWS Console (40 min). Código:  
  ```java
  import com.amazonaws.services.lambda.runtime.Context;
  import com.amazonaws.services.lambda.runtime.RequestHandler;
  import java.util.Map;

  public class HelloWorldHandler implements RequestHandler<Map<String, String>, String> {
      @Override
      public String handleRequest(Map<String, String> input, Context context) {
          return "Hello, " + input.getOrDefault("name", "AWS");
      }
  }
  ```  
  **Entregável**: Função Lambda criada e testada.  

- **Quinta, 24/07**  
  **Objetivo**: Configurar API Gateway com Lambda.  
  **Tarefa**: Assista à seção "API Gateway" do curso de Maarek (20 min). Crie uma API REST no API Gateway com endpoint GET `/hello` que aciona a função Lambda (40 min). Teste via Postman.  
  **Entregável**: API Gateway configurada com endpoint funcional.  

- **Sexta, 25/07**  
  **Objetivo**: Iniciar Projeto Amigas na Estrada.  
  **Tarefa**: Revise integração API Gateway-Lambda no curso de Maarek (20 min). Crie uma tabela DynamoDB `Users` (chave de partição: `userId`, chave de ordenação: `destination`) via AWS Console (40 min). Insira um item manualmente.  
  **Entregável**: Tabela `Users` criada.  

### Semana 2 (28 de julho a 1 de agosto de 2025): Lambda e DynamoDB
- **Segunda, 28/07**  
  **Objetivo**: Usar SDK Java com Lambda.  
  **Tarefa**: Assista à seção "SDK Java" do curso de Maarek (20 min). Crie uma função Lambda para salvar perfis na tabela `Users` usando SDK Java (40 min). Código:  
  ```java
  import com.amazonaws.services.lambda.runtime.Context;
  import com.amazonaws.services.lambda.runtime.RequestHandler;
  import software.amazon.awssdk.services.dynamodb.DynamoDbClient;
  import software.amazon.awssdk.services.dynamodb.model.PutItemRequest;
  import java.util.HashMap;
  import java.util.Map;

  public class CreateUserHandler implements RequestHandler<Map<String, String>, String> {
      private final DynamoDbClient dynamoDb = DynamoDbClient.create();

      @Override
      public String handleRequest(Map<String, String> input, Context context) {
          Map<String, String> item = new HashMap<>();
          item.put("userId", input.get("userId"));
          item.put("destination", input.get("destination"));
          item.put("travelDate", input.get("travelDate"));

          PutItemRequest request = PutItemRequest.builder()
                  .tableName("Users")
                  .item(item)
                  .build();
          dynamoDb.putItem(request);
          return "User created: " + input.get("userId");
      }
  }
  ```  
  **Entregável**: Função Lambda salvando perfis no DynamoDB.  

- **Terça, 29/07**  
  **Objetivo**: Consultar DynamoDB com SDK Java.  
  **Tarefa**: Assista à seção "DynamoDB GET" do curso de Maarek (20 min). Crie uma função Lambda para consultar perfis por `destination` (GET `/users/{destination}`) usando SDK Java (40 min). Teste via API Gateway.  
  **Entregável**: Endpoint GET funcional.  

- **Quarta, 30/07**  
  **Objetivo**: Configurar permissões IAM para Lambda-DynamoDB.  
  **Tarefa**: Revise permissões IAM no curso de Maarek (20 min). Crie uma política IAM para a função Lambda acessar `Users` (40 min). Teste a função.  
  **Entregável**: Permissões IAM configuradas.  

- **Quinta, 31/07**  
  **Objetivo**: Finalizar endpoints do Projeto Amigas na Estrada.  
  **Tarefa**: Assista à seção "Integração API Gateway-Lambda-DynamoDB" do curso de Maarek (20 min). Teste o projeto via Postman (POST `/users`, GET `/users/{destination}`) e valide dados no DynamoDB (40 min).  
  **Entregável**: Endpoints do projeto funcionais.  

- **Sexta, 01/08**  
  **Objetivo**: Introdução a SQS.  
  **Tarefa**: Assista à seção "SQS" do curso de Maarek (20 min). Crie uma fila SQS `ConnectionQueue` via AWS Console e envie uma mensagem de teste via AWS CLI (40 min).  
  **Entregável**: Fila SQS criada e testada.  

### Semana 3 (4 a 8 de agosto de 2025): SQS e SNS
- **Segunda, 04/08**  
  **Objetivo**: Integrar SQS com Lambda.  
  **Tarefa**: Assista à seção "Integração SQS-Lambda" do curso de Maarek (20 min). Crie uma função Lambda em Java para consumir mensagens da `ConnectionQueue` (40 min). Código:  
  ```java
  import com.amazonaws.services.lambda.runtime.Context;
  import com.amazonaws.services.lambda.runtime.events.SQSEvent;

  public class ProcessConnectionHandler {
      public void handleRequest(SQSEvent event, Context context) {
          for (SQSEvent.SQSMessage msg : event.getRecords()) {
              context.getLogger().log("Connection request: " + msg.getBody());
          }
      }
  }
  ```  
  **Entregável**: Função Lambda consumindo mensagens SQS.  

- **Terça, 05/08**  
  **Objetivo**: Introdução a SNS.  
  **Tarefa**: Assista à seção "SNS" do curso de Maarek (20 min). Crie um tópico SNS `ConnectionTopic` e adicione um assinante de e-mail (40 min). Publique uma mensagem de teste via AWS Console.  
  **Entregável**: Tópico SNS configurado.  

- **Quarta, 06/08**  
  **Objetivo**: Integrar SNS com Lambda e SQS.  
  **Tarefa**: Revise integração SQS-SNS no curso de Maarek (20 min). Modifique a função Lambda para publicar mensagens do SQS no `ConnectionTopic` (40 min). Código:  
  ```java
  import com.amazonaws.services.lambda.runtime.Context;
  import com.amazonaws.services.lambda.runtime.events.SQSEvent;
  import software.amazon.awssdk.services.sns.SnsClient;
  import software.amazon.awssdk.services.sns.model.PublishRequest;

  public class ProcessConnectionHandler {
      private final SnsClient snsClient = SnsClient.create();

      public void handleRequest(SQSEvent event, Context context) {
          for (SQSEvent.SQSMessage msg : event.getRecords()) {
              PublishRequest request = PublishRequest.builder()
                      .topicArn("arn:aws:sns:us-east-1:123456789012:ConnectionTopic")
                      .message(msg.getBody())
                      .build();
              snsClient.publish(request);
          }
      }
  }
  ```  
  **Entregável**: Integração SQS-SNS-Lambda funcional.  

- **Quinta, 07/08**  
  **Objetivo**: Adicionar notificações ao Projeto Amigas na Estrada.  
  **Tarefa**: Assista à seção "Mensageria" do curso de Maarek (20 min). Configure o projeto: Crie uma função Lambda que consome da `ConnectionQueue` e publica no `ConnectionTopic`. Teste enviando uma mensagem via SDK Java (40 min).  
  **Entregável**: Notificações do projeto configuradas.  

- **Sexta, 08/08**  
  **Objetivo**: Introdução a AWS SAM.  
  **Tarefa**: Assista à seção "AWS SAM" do curso de Maarek (20 min). Instale AWS SAM CLI e crie um projeto SAM básico com uma função Lambda em Java (40 min). Teste localmente com `sam local invoke`.  
  **Entregável**: Projeto SAM inicializado.  

### Semana 4 (11 a 15 de agosto de 2025): AWS SAM e Implantação
- **Segunda, 11/08**  
  **Objetivo**: Implantar Projeto Amigas na Estrada com SAM.  
  **Tarefa**: Revise SAM templates no curso de Maarek (20 min). Crie um `template.yaml` para o projeto (SQS, SNS, Lambda, API Gateway, DynamoDB) (40 min). Exemplo:  
  ```yaml
  Resources:
    ConnectionQueue:
      Type: AWS::SQS::Queue
      Properties:
        QueueName: ConnectionQueue
    ConnectionTopic:
      Type: AWS::SNS::Topic
      Properties:
        TopicName: ConnectionTopic
    CreateUserFunction:
      Type: AWS::Serverless::Function
      Properties:
        CodeUri: ./target/lambda.jar
        Handler: CreateUserHandler::handleRequest
        Runtime: java11
        Policies:
          - DynamoDBWritePolicy:
              TableName: Users
    ProcessConnectionFunction:
      Type: AWS::Serverless::Function
      Properties:
        CodeUri: ./target/lambda.jar
        Handler: ProcessConnectionHandler::handleRequest
        Runtime: java11
        Events:
          SQSEvent:
            Type: SQS
            Properties:
              Queue: !GetAtt ConnectionQueue.Arn
  ```  
  **Entregável**: Template SAM criado.  

- **Terça, 12/08**  
  **Objetivo**: Implantar e testar Projeto Amigas na Estrada.  
  **Tarefa**: Assista à seção "Implantação SAM" do curso de Maarek (20 min). Implante o projeto com `sam deploy` (40 min). Teste enviando uma mensagem SQS via SDK Java e verificando a notificação SNS.  
  **Entregável**: Projeto implantado.  

- **Quarta, 13/08**  
  **Objetivo**: Introdução a CloudWatch.  
  **Tarefa**: Assista à seção "CloudWatch" do curso de Maarek (20 min). Habilite CloudWatch Logs para a função Lambda do projeto e analise logs após uma execução (40 min).  
  **Entregável**: Logs visíveis no CloudWatch.  

- **Quinta, 14/08**  
  **Objetivo**: Introdução a AWS X-Ray.  
  **Tarefa**: Assista à seção "X-Ray" do curso de Maarek (20 min). Habilite X-Ray na função Lambda do projeto e analise traces (40 min).  
  **Entregável**: X-Ray configurado.  

- **Sexta, 15/08**  
  **Objetivo**: Finalizar Projeto Amigas na Estrada.  
  **Tarefa**: Revise integração SQS-SNS-Lambda no curso de Maarek (20 min). Teste o projeto completo: envie mensagens via SDK Java, verifique processamento e notificações (40 min).  
  **Entregável**: Projeto concluído.  

### Semana 5 (18 a 22 de agosto de 2025): Segurança
- **Segunda, 18/08**  
  **Objetivo**: Aprofundar em IAM avançado.  
  **Tarefa**: Assista à seção "Políticas IAM Avançadas" do curso de Maarek (20 min). Crie uma política IAM com condições (ex.: acesso ao DynamoDB apenas de uma VPC) para o projeto (40 min).  
  **Entregável**: Política IAM com condições configurada.  

- **Terça, 19/08**  
  **Objetivo**: Introdução a Amazon Cognito.  
  **Tarefa**: Assista à seção "Cognito" do curso de Maarek (20 min). Crie um User Pool no Cognito e configure autenticação para o endpoint `/users` do projeto (40 min).  
  **Entregável**: Cognito configurado no projeto.  

- **Quarta, 20/08**  
  **Objetivo**: Introdução a AWS KMS.  
  **Tarefa**: Assista à seção "KMS" do curso de Maarek (20 min). Crie uma chave KMS e use-a para criptografar dados na tabela `Users` do projeto (40 min).  
  **Entregável**: Dados criptografados no DynamoDB.  

- **Quinta, 21/08**  
  **Objetivo**: Introdução a AWS Secrets Manager.  
  **Tarefa**: Assista à seção "Secrets Manager" do curso de Maarek (20 min). Armazene uma credencial no Secrets Manager e acesse-a via SDK Java na função Lambda do projeto (40 min).  
  **Entregável**: Secrets Manager integrado.  

- **Sexta, 22/08**  
  **Objetivo**: Revisar segurança no Projeto Amigas na Estrada.  
  **Tarefa**: Revise práticas de segurança no curso de Maarek (20 min). Teste o projeto com Cognito, KMS e Secrets Manager, validando autenticação e criptografia (40 min).  
  **Entregável**: Projeto com segurança implementada.  

### Semana 6 (25 a 29 de agosto de 2025): CI/CD
- **Segunda, 25/08**  
  **Objetivo**: Introdução a CodePipeline.  
  **Tarefa**: Assista à seção "CodePipeline" do curso de Maarek (20 min). Crie um pipeline simples no CodePipeline para implantar uma função Lambda (40 min).  
  **Entregável**: Pipeline básico configurado.  

- **Terça, 26/08**  
  **Objetivo**: Introdução a CodeBuild.  
  **Tarefa**: Assista à seção "CodeBuild" do curso de Maarek (20 min). Configure um projeto CodeBuild para compilar o código Java do projeto (40 min).  
  **Entregável**: Projeto CodeBuild configurado.  

- **Quarta, 27/08**  
  **Objetivo**: Integrar CodePipeline com SAM.  
  **Tarefa**: Revise integração CodePipeline-SAM no curso de Maarek (20 min). Modifique o pipeline para implantar o Projeto Amigas na Estrada usando SAM (40 min).  
  **Entregável**: Pipeline com SAM funcional.  

- **Quinta, 28/08**  
  **Objetivo**: Testar pipeline CI/CD.  
  **Tarefa**: Assista à seção "Boas Práticas CI/CD" do curso de Maarek (20 min). Faça uma alteração no código do projeto e teste o pipeline completo (40 min).  
  **Entregável**: Pipeline testado.  

- **Sexta, 29/08**  
  **Objetivo**: Adicionar monitoramento ao projeto.  
  **Tarefa**: Assista à seção "CloudWatch Logs" do curso de Maarek (20 min). Adicione métricas personalizadas ao projeto (ex.: contagem de conexões processadas) usando SDK Java (40 min).  
  **Entregável**: Métricas personalizadas configuradas.  

### Semana 7 (1 a 5 de setembro de 2025): Otimização
- **Segunda, 01/09**  
  **Objetivo**: Otimizar Lambda.  
  **Tarefa**: Assista à seção "Otimização de Lambda" do curso de Maarek (20 min). Ajuste a função Lambda do projeto (ex.: aumentar memória, reduzir tempo de execução) e teste (40 min).  
  **Entregável**: Função otimizada.  

- **Terça, 02/09**  
  **Objetivo**: Otimizar DynamoDB.  
  **Tarefa**: Assista à seção "Otimização de DynamoDB" do curso de Maarek (20 min). Adicione um índice secundário global à tabela `Users` e teste consultas (40 min).  
  **Entregável**: Índice configurado.  

- **Quarta, 03/09**  
  **Objetivo**: Revisar segurança.  
  **Tarefa**: Revise segurança no curso de Maarek (20 min). Adicione Cognito ao projeto para proteger o endpoint `/users` (40 min).  
  **Entregável**: Autenticação configurada.  

- **Quinta, 04/09**  
  **Objetivo**: Revisar monitoramento.  
  **Tarefa**: Assista à seção "CloudWatch Avançado" do curso de Maarek (20 min). Crie um alarme CloudWatch para monitorar falhas no projeto (40 min).  
  **Entregável**: Alarme configurado.  

- **Sexta, 05/09**  
  **Objetivo**: Revisar CI/CD.  
  **Tarefa**: Revise CI/CD no curso de Maarek (20 min). Adicione o projeto a um pipeline CodePipeline (40 min).  
  **Entregável**: Pipeline configurado.  

### Semana 8 (8 a 12 de setembro de 2025): Revisão Geral
- **Segunda, 08/09**  
  **Objetivo**: Revisar Lambda e API Gateway.  
  **Tarefa**: Assista à revisão "Lambda/API Gateway" do curso de Maarek (20 min). Teste o projeto via Postman (40 min).  
  **Entregável**: Projeto revisado.  

- **Terça, 09/09**  
  **Objetivo**: Revisar DynamoDB e SDK.  
  **Tarefa**: Assista à revisão "DynamoDB" do curso de Maarek (20 min). Adicione uma consulta complexa ao projeto usando SDK Java (40 min).  
  **Entregável**: Consulta implementada.  

- **Quarta, 10/09**  
  **Objetivo**: Revisar SQS/SNS.  
  **Tarefa**: Assista à revisão "SQS/SNS" do curso de Maarek (20 min). Teste o projeto enviando mensagens via SDK Java (40 min).  
  **Entregável**: Projeto revisado.  

- **Quinta, 11/09**  
  **Objetivo**: Revisar segurança.  
  **Tarefa**: Assista à revisão "IAM/Cognito" do curso de Maarek (20 min). Valide autenticação no projeto (40 min).  
  **Entregável**: Autenticação revisada.  

- **Sexta, 12/09**  
  **Objetivo**: Revisar implantação.  
  **Tarefa**: Assista à revisão "CI/CD" do curso de Maarek (20 min). Teste o pipeline do projeto (40 min).  
  **Entregável**: Pipeline revisado.  

### Semana 9 (15 a 19 de setembro de 2025): Simulados
- **Segunda, 15/09**  
  **Objetivo**: Simulado 1 Tutorials Dojo.  
  **Tarefa**: Faça o Simulado 1 de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Terça, 16/09**  
  **Objetivo**: Simulado 1 Maarek.  
  **Tarefa**: Faça o Simulado 1 de Maarek (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Quarta, 17/09**  
  **Objetivo**: Simulado 2 Tutorials Dojo.  
  **Tarefa**: Faça o Simulado 2 de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Quinta, 18/09**  
  **Objetivo**: Simulado 2 Maarek.  
  **Tarefa**: Faça o Simulado 2 de Maarek (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Sexta, 19/09**  
  **Objetivo**: Revisar tópicos fracos.  
  **Tarefa**: Identifique tópicos fracos nos simulados (20 min). Estude esses tópicos no curso de Maarek (40 min).  
  **Entregável**: Tópicos fracos revisados.  

### Semana 10 (22 a 26 de setembro de 2025): Simulados
- **Segunda, 22/09**  
  **Objetivo**: Simulado 3 Tutorials Dojo.  
  **Tarefa**: Faça o Simulado 3 de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Terça, 23/09**  
  **Objetivo**: Simulado 3 Maarek.  
  **Tarefa**: Faça o Simulado 3 de Maarek (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Quarta, 24/09**  
  **Objetivo**: Simulado 4 Tutorials Dojo.  
  **Tarefa**: Faça o Simulado 4 de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Quinta, 25/09**  
  **Objetivo**: Simulado 4 Maarek.  
  **Tarefa**: Faça o Simulado 4 de Maarek (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Sexta, 26/09**  
  **Objetivo**: Revisar tópicos fracos.  
  **Tarefa**: Identifique tópicos fracos nos simulados (20 min). Estude esses tópicos no curso de Maarek (40 min).  
  **Entregável**: Tópicos fracos revisados.  

### Semana 11 (29 de setembro a 3 de outubro de 2025): Simulados
- **Segunda, 29/09**  
  **Objetivo**: Simulado 5 Tutorials Dojo.  
  **Tarefa**: Faça o Simulado 5 de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Terça, 30/09**  
  **Objetivo**: Simulado 5 Maarek.  
  **Tarefa**: Faça o Simulado 5 de Maarek (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Quarta, 01/10**  
  **Objetivo**: Revisar tópicos fracos.  
  **Tarefa**: Identifique tópicos fracos nos simulados (20 min). Estude esses tópicos no curso de Maarek (40 min).  
  **Entregável**: Tópicos fracos revisados.  

- **Quinta, 02/10**  
  **Objetivo**: Repetir Simulado 1 Tutorials Dojo.  
  **Tarefa**: Refaça o Simulado 1 de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Sexta, 03/10**  
  **Objetivo**: Repetir Simulado 1 Maarek.  
  **Tarefa**: Refaça o Simulado 1 de Maarek (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

### Semana 12 (6 a 10 de outubro de 2025): Simulados
- **Segunda, 06/10**  
  **Objetivo**: Repetir Simulado 2 Tutorials Dojo.  
  **Tarefa**: Refaça o Simulado 2 de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Terça, 07/10**  
  **Objetivo**: Repetir Simulado 2 Maarek.  
  **Tarefa**: Refaça o Simulado 2 de Maarek (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Quarta, 08/10**  
  **Objetivo**: Repetir Simulado 3 Tutorials Dojo.  
  **Tarefa**: Refaça o Simulado 3 de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Quinta, 09/10**  
  **Objetivo**: Repetir Simulado 3 Maarek.  
  **Tarefa**: Refaça o Simulado 3 de Maarek (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Sexta, 10/10**  
  **Objetivo**: Revisar tópicos fracos.  
  **Tarefa**: Identifique tópicos fracos nos simulados (20 min). Estude esses tópicos no curso de Maarek (40 min).  
  **Entregável**: Tópicos fracos revisados.  

### Semana 13 (13 a 17 de outubro de 2025): Simulados
- **Segunda, 13/10**  
  **Objetivo**: Repetir Simulado 4 Tutorials Dojo.  
  **Tarefa**: Refaça o Simulado 4 de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Terça, 14/10**  
  **Objetivo**: Repetir Simulado 4 Maarek.  
  **Tarefa**: Refaça o Simulado 4 de Maarek (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Quarta, 15/10**  
  **Objetivo**: Repetir Simulado 5 Tutorials Dojo.  
  **Tarefa**: Refaça o Simulado 5 de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Quinta, 16/10**  
  **Objetivo**: Repetir Simulado 5 Maarek.  
  **Tarefa**: Refaça o Simulado 5 de Maarek (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Sexta, 17/10**  
  **Objetivo**: Revisar tópicos fracos.  
  **Tarefa**: Identifique tópicos fracos nos simulados (20 min). Estude esses tópicos no curso de Maarek (40 min).  
  **Entregável**: Tópicos fracos revisados.  

### Semana 14 (20 a 24 de outubro de 2025): Simulados
- **Segunda, 20/10**  
  **Objetivo**: Repetir Simulado 1 Tutorials Dojo.  
  **Tarefa**: Refaça o Simulado 1 de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Terça, 21/10**  
  **Objetivo**: Repetir Simulado 1 Maarek.  
  **Tarefa**: Refaça o Simulado 1 de Maarek (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Quarta, 22/10**  
  **Objetivo**: Repetir Simulado 2 Tutorials Dojo.  
  **Tarefa**: Refaça o Simulado 2 de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Quinta, 23/10**  
  **Objetivo**: Repetir Simulado 2 Maarek.  
  **Tarefa**: Refaça o Simulado 2 de Maarek (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Sexta, 24/10**  
  **Objetivo**: Revisar tópicos fracos.  
  **Tarefa**: Identifique tópicos fracos nos simulados (20 min). Estude esses tópicos no curso de Maarek (40 min).  
  **Entregável**: Tópicos fracos revisados.  

### Semana 15 (27 a 31 de outubro de 2025): Simulados
- **Segunda, 27/10**  
  **Objetivo**: Repetir Simulado 3 Tutorials Dojo.  
  **Tarefa**: Refaça o Simulado 3 de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Terça, 28/10**  
  **Objetivo**: Repetir Simulado 3 Maarek.  
  **Tarefa**: Refaça o Simulado 3 de Maarek (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Quarta, 29/10**  
  **Objetivo**: Repetir Simulado 4 Tutorials Dojo.  
  **Tarefa**: Refaça o Simulado 4 de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Quinta, 30/10**  
  **Objetivo**: Repetir Simulado 4 Maarek.  
  **Tarefa**: Refaça o Simulado 4 de Maarek (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Sexta, 31/10**  
  **Objetivo**: Revisar tópicos fracos.  
  **Tarefa**: Identifique tópicos fracos nos simulados (20 min). Estude esses tópicos no curso de Maarek (40 min).  
  **Entregável**: Tópicos fracos revisados.  

### Semana 16 (3 a 7 de novembro de 2025): Simulados
- **Segunda, 03/11**  
  **Objetivo**: Repetir Simulado 5 Tutorials Dojo.  
  **Tarefa**: Refaça o Simulado 5 de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Terça, 04/11**  
  **Objetivo**: Repetir Simulado 5 Maarek.  
  **Tarefa**: Refaça o Simulado 5 de Maarek (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Quarta, 05/11**  
  **Objetivo**: Repetir Simulado 1 Tutorials Dojo.  
  **Tarefa**: Refaça o Simulado 1 de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Quinta, 06/11**  
  **Objetivo**: Repetir Simulado 1 Maarek.  
  **Tarefa**: Refaça o Simulado 1 de Maarek (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Sexta, 07/11**  
  **Objetivo**: Revisar tópicos fracos.  
  **Tarefa**: Identifique tópicos fracos nos simulados (20 min). Estude esses tópicos no curso de Maarek (40 min).  
  **Entregável**: Tópicos fracos revisados.  

### Semana 17 (10 a 14 de novembro de 2025): Simulados
- **Segunda, 10/11**  
  **Objetivo**: Repetir Simulado 2 Tutorials Dojo.  
  **Tarefa**: Refaça o Simulado 2 de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Terça, 11/11**  
  **Objetivo**: Repetir Simulado 2 Maarek.  
  **Tarefa**: Refaça o Simulado 2 de Maarek (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Quarta, 12/11**  
  **Objetivo**: Repetir Simulado 3 Tutorials Dojo.  
  **Tarefa**: Refaça o Simulado 3 de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Quinta, 13/11**  
  **Objetivo**: Repetir Simulado 3 Maarek.  
  **Tarefa**: Refaça o Simulado 3 de Maarek (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Sexta, 14/11**  
  **Objetivo**: Revisar tópicos fracos.  
  **Tarefa**: Identifique tópicos fracos nos simulados (20 min). Estude esses tópicos no curso de Maarek (40 min).  
  **Entregável**: Tópicos fracos revisados.  

### Semana 18 (17 a 21 de novembro de 2025): Simulados
- **Segunda, 17/11**  
  **Objetivo**: Repetir Simulado 4 Tutorials Dojo.  
  **Tarefa**: Refaça o Simulado 4 de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Terça, 18/11**  
  **Objetivo**: Repetir Simulado 4 Maarek.  
  **Tarefa**: Refaça o Simulado 4 de Maarek (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Quarta, 19/11**  
  **Objetivo**: Repetir Simulado 5 Tutorials Dojo.  
  **Tarefa**: Refaça o Simulado 5 de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Quinta, 20/11**  
  **Objetivo**: Repetir Simulado 5 Maarek.  
  **Tarefa**: Refaça o Simulado 5 de Maarek (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Sexta, 21/11**  
  **Objetivo**: Revisar tópicos fracos.  
  **Tarefa**: Identifique tópicos fracos nos simulados (20 min). Estude esses tópicos no curso de Maarek (40 min).  
  **Entregável**: Tópicos fracos revisados.  

### Semana 19 (24 a 28 de novembro de 2025): Preparação Final
- **Segunda, 24/11**  
  **Objetivo**: Revisar Projeto Amigas na Estrada.  
  **Tarefa**: Revise anotações do projeto (20 min). Teste todos os endpoints via Postman (40 min).  
  **Entregável**: Projeto revisado.  

- **Terça, 25/11**  
  **Objetivo**: Repetir Simulado 1 Tutorials Dojo.  
  **Tarefa**: Refaça o Simulado 1 de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Quarta, 26/11**  
  **Objetivo**: Repetir Simulado 1 Maarek.  
  **Tarefa**: Refaça o Simulado 1 de Maarek (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído, erros revisados.  

- **Quinta, 27/11**  
  **Objetivo**: Revisão final.  
  **Tarefa**: Revise o Guia do Exame da AWS (20 min, https://d1.awsstatic.com/training-and-certification/docs-dev-associate/AWS-Certified-Developer-Associate_Exam-Guide.pdf). Faça 10 questões de cada simulado (Tutorials Dojo, Maarek) (40 min).  
  **Entregável**: Revisão finalizada.  

- **Sexta, 28/11**  
  **Objetivo**: Descanso e preparação mental.  
  **Tarefa**: Revise anotações leves (20 min). Descanse e prepare-se para o exame (40 min).  
  **Entregável**: Preparação mental concluída.  

## Dicas Finais
- **Ambiente AWS**: Monitore custos no AWS Budgets. Exclua recursos após o projeto para evitar encargos.  
- **Simulados**: Continue refazendo simulados até atingir uma pontuação consistente (recomendado: ≥80% em cada simulado). Anote tópicos fracos (ex.: KMS, X-Ray) e revise-os prioritariamente.  
- **Projeto**: Mantenha o código do Projeto Amigas na Estrada em um repositório Git (local ou GitHub). Exemplo: `git commit -m "Projeto Amigas na Estrada concluído"`.  
- **Prova**: Confirme o exame para 28 de novembro de 2025 via Pearson VUE. Escolha o idioma português, mas consulte termos técnicos em inglês se necessário.  
- **Descanso**: Reserve o fim de semana antes da prova (22-23/11) para descansar e revisar anotações leves.  
- **Exportação**: Para transferir este cronograma, salve como `aws-developer-study-plan.md`. Copie para um USB, envie por e-mail ou publique no GitHub. Use um editor Markdown ou converta para PDF com Pandoc.

