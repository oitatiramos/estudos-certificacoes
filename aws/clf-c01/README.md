# Cronograma de Estudo para AWS Certified Developer – Associate (DVA-C02)

**Período**: 21 de julho a 31 de outubro de 2025 (15 semanas)  
**Objetivo**: Obter a certificação AWS Certified Developer – Associate (DVA-C02) em novembro de 2025  
**Carga Horária**: 1 hora por dia, de segunda a sexta (ex.: 19h às 20h, horário sugerido para consistência)  
**Linguagem**: Java  
**Foco**: API Gateway, Lambda, DynamoDB, SQS, SNS, AWS SAM, SDK Java  
**Recursos**:  
- **Cursos**: Neal Davis (Udemy), Stephane Maarek (Udemy), AWS Skill Builder (laboratórios gratuitos/pagos).  
- **Simulados**: Neal Davis, Stephane Maarek, Tutorials Dojo.  
- **Conta AWS**: Nível gratuito (monitore custos via AWS Budgets).  
- **Ferramentas**: AWS CLI, AWS SAM CLI, SDK Java (Maven/Gradle), IDE (ex.: IntelliJ), Postman.  
**Exportação**: Salve este arquivo como `aws-developer-study-plan.md`. Transfira para outro computador via USB, e-mail ou repositório Git (ex.: GitHub). Visualize em editores Markdown (VS Code, Obsidian) ou converta para PDF com Pandoc (`pandoc aws-developer-study-plan.md -o cronograma.pdf`).

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

## Projetos Práticos
Quatro projetos práticos serão implementados para reforçar os serviços:  
1. **Sistema de Gerenciamento de Pedidos**: API REST com API Gateway, Lambda e DynamoDB.  
2. **Sistema de Notificação de Eventos**: Mensageria com SQS, SNS, Lambda e SAM.  
3. **Sistema de Processamento de Logs**: Logs com Lambda, DynamoDB, CloudWatch e SAM.  
4. **Sistema de Gerenciamento de Tarefas**: Integração completa (API Gateway, Lambda, DynamoDB, SQS, SNS, SAM).

## Cronograma Detalhado

### Semana 1 (21 a 25 de julho de 2025): Fundamentos e Configuração
- **Segunda, 21/07**  
  **Objetivo**: Entender fundamentos da AWS e configurar ambiente.  
  **Tarefa**: Assista à seção "AWS Fundamentals" do curso de Neal Davis (Udemy, 20 min). Instale e configure AWS CLI e SDK Java (Maven/Gradle) em um projeto Java (40 min). Teste com `aws configure` e crie um projeto com dependência `software.amazon.awssdk:dynamodb`.  
  **Entregável**: AWS CLI configurado, projeto Java com SDK funcional.  

- **Terça, 22/07**  
  **Objetivo**: Aprender IAM e permissões.  
  **Tarefa**: Assista à seção "IAM" do curso de Stephane Maarek (20 min). Crie um usuário IAM com permissões para Lambda e幹DB via AWS Console (40 min). Teste com `aws sts get-caller-identity`.  
  **Entregável**: Usuário IAM configurado e testado.  

- **Quarta, 23/07**  
  **Objetivo**: Entender AWS Lambda.  
  **Tarefa**: Assista à seção "Lambda" do curso de Neal Davis (20 min). Crie uma função Lambda em Java ("Hello World") via AWS Console (40 min). Código:  
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
  **Objetivo**: Iniciar Projeto 1 (Sistema de Gerenciamento de Pedidos).  
  **Tarefa**: Revise integração API Gateway-Lambda no curso de Neal Davis (20 min). Crie uma tabela DynamoDB `Pedidos` (chave de partição: `orderId`, chave de ordenação: `timestamp`) via AWS Console (40 min). Insira um item manualmente.  
  **Entregável**: Tabela `Pedidos` criada.  

### Semana 2 (28 de julho a 1 de agosto de 2025): Lambda e DynamoDB
- **Segunda, 28/07**  
  **Objetivo**: Usar SDK Java com Lambda.  
  **Tarefa**: Assista à seção "SDK Java" do curso de Maarek (20 min). Modifique a função Lambda do Projeto 1 para salvar pedidos na tabela `Pedidos` usando SDK Java (40 min). Código:  
  ```java
  import com.amazonaws.services.lambda.runtime.Context;
  import com.amazonaws.services.lambda.runtime.RequestHandler;
  import software.amazon.awssdk.services.dynamodb.DynamoDbClient;
  import software.amazon.awssdk.services.dynamodb.model.PutItemRequest;
  import java.util.HashMap;
  import java.util.Map;

  public class CreateOrderHandler implements RequestHandler<Map<String, String>, String> {
      private final DynamoDbClient dynamoDb = DynamoDbClient.create();

      @Override
      public String handleRequest(Map<String, String> input, Context context) {
          Map<String, String> item = new HashMap<>();
          item.put("orderId", input.get("orderId"));
          item.put("timestamp", String.valueOf(System.currentTimeMillis()));
          item.put("itemName", input.get("itemName"));

          PutItemRequest request = PutItemRequest.builder()
                  .tableName("Pedidos")
                  .item(item)
                  .build();
          dynamoDb.putItem(request);
          return "Order created: " + input.get("orderId");
      }
  }
  ```  
  **Entregável**: Função Lambda salvando pedidos no DynamoDB.  

- **Terça, 29/07**  
  **Objetivo**: Consultar DynamoDB com SDK Java.  
  **Tarefa**: Assista à seção "DynamoDB GET" do curso de Neal Davis (20 min). Crie uma função Lambda para consultar pedidos por `orderId` (GET `/orders/{orderId}`) usando SDK Java (40 min). Teste via API Gateway.  
  **Entregável**: Endpoint GET funcional.  

- **Quarta, 30/07**  
  **Objetivo**: Configurar permissões IAM para Lambda-DynamoDB.  
  **Tarefa**: Revise permissões IAM no curso de Maarek (20 min). Crie uma política IAM para a função Lambda acessar `Pedidos` (40 min). Teste a função.  
  **Entregável**: Permissões IAM configuradas.  

- **Quinta, 31/07**  
  **Objetivo**: Finalizar Projeto 1.  
  **Tarefa**: Assista à seção "Integração API Gateway-Lambda-DynamoDB" do curso de Neal Davis (20 min). Teste o Projeto 1 completo via Postman (POST `/orders`, GET `/orders/{orderId}`) e valide dados no DynamoDB (40 min).  
  **Entregável**: Projeto 1 funcional.  

- **Sexta, 01/08**  
  **Objetivo**: Introdução a SQS.  
  **Tarefa**: Assista à seção "SQS" do curso de Maarek (20 min). Crie uma fila SQS `EventQueue` via AWS Console e envie uma mensagem de teste via AWS CLI (40 min).  
  **Entregável**: Fila SQS criada e testada.  

### Semana 3 (4 a 8 de agosto de 2025): SQS e SNS
- **Segunda, 04/08**  
  **Objetivo**: Integrar SQS com Lambda.  
  **Tarefa**: Assista à seção "Integração SQS-Lambda" do curso de Neal Davis (20 min). Crie uma função Lambda em Java para consumir mensagens da `EventQueue` (40 min). Código:  
  ```java
  import com.amazonaws.services.lambda.runtime.Context;
  import com.amazonaws.services.lambda.runtime.events.SQSEvent;

  public class ProcessQueueHandler {
      public void handleRequest(SQSEvent event, Context context) {
          for (SQSEvent.SQSMessage msg : event.getRecords()) {
              context.getLogger().log("Mensagem recebida: " + msg.getBody());
          }
      }
  }
  ```  
  **Entregável**: Função Lambda consumindo mensagens SQS.  

- **Terça, 05/08**  
  **Objetivo**: Introdução a SNS.  
  **Tarefa**: Assista à seção "SNS" do curso de Maarek (20 min). Crie um tópico SNS `EventTopic` e adicione um assinante de e-mail (40 min). Publique uma mensagem de teste via AWS Console.  
  **Entregável**: Tópico SNS configurado.  

- **Quarta, 06/08**  
  **Objetivo**: Integrar SNS com Lambda e SQS.  
  **Tarefa**: Revise integração SQS-SNS no curso de Neal Davis (20 min). Modifique a função Lambda para publicar mensagens do SQS no `EventTopic` (40 min). Código:  
  ```java
  import com.amazonaws.services.lambda.runtime.Context;
  import com.amazonaws.services.lambda.runtime.events.SQSEvent;
  import software.amazon.awssdk.services.sns.SnsClient;
  import software.amazon.awssdk.services.sns.model.PublishRequest;

  public class ProcessQueueHandler {
      private final SnsClient snsClient = SnsClient.create();

      public void handleRequest(SQSEvent event, Context context) {
          for (SQSEvent.SQSMessage msg : event.getRecords()) {
              PublishRequest request = PublishRequest.builder()
                      .topicArn("arn:aws:sns:us-east-1:123456789012:EventTopic")
                      .message(msg.getBody())
                      .build();
              snsClient.publish(request);
          }
      }
  }
  ```  
  **Entregável**: Integração SQS-SNS-Lambda funcional.  

- **Quinta, 07/08**  
  **Objetivo**: Iniciar Projeto 2 (Sistema de Notificação de Eventos).  
  **Tarefa**: Assista à seção "Mensageria" do curso de Maarek (20 min). Configure o Projeto 2: Crie uma função Lambda que consome da `EventQueue` e publica no `EventTopic`. Teste enviando uma mensagem via SDK Java (40 min).  
  **Entregável**: Projeto 2 parcialmente configurado.  

- **Sexta, 08/08**  
  **Objetivo**: Introdução a AWS SAM.  
  **Tarefa**: Assista à seção "AWS SAM" do curso de Neal Davis (20 min). Instale AWS SAM CLI e crie um projeto SAM básico com uma função Lambda em Java (40 min). Teste localmente com `sam local invoke`.  
  **Entregável**: Projeto SAM inicializado.  

### Semana 4 (11 a 15 de agosto de 2025): AWS SAM e Implantação
- **Segunda, 11/08**  
  **Objetivo**: Implantar Projeto 2 com SAM.  
  **Tarefa**: Revise SAM templates no curso de Maarek (20 min). Crie um `template.yaml` para o Projeto 2 (SQS, SNS, Lambda) (40 min). Exemplo:  
  ```yaml
  Resources:
    EventQueue:
      Type: AWS::SQS::Queue
      Properties:
        QueueName: EventQueue
    EventTopic:
      Type: AWS::SNS::Topic
      Properties:
        TopicName: EventTopic
    ProcessEventFunction:
      Type: AWS::Serverless::Function
      Properties:
        CodeUri: ./target/lambda.jar
        Handler: ProcessQueueHandler::handleRequest
        Runtime: java11
        Events:
          SQSEvent:
            Type: SQS
            Properties:
              Queue: !GetAtt EventQueue.Arn
  ```  
  **Entregável**: Template SAM criado.  

- **Terça, 12/08**  
  **Objetivo**: Implantar e testar Projeto 2.  
  **Tarefa**: Assista à seção "Implantação SAM" do curso de Neal Davis (20 min). Implante o Projeto 2 com `sam deploy` (40 min). Teste enviando uma mensagem SQS via SDK Java e verificando a notificação SNS.  
  **Entregável**: Projeto 2 implantado.  

- **Quarta, 13/08**  
  **Objetivo**: Introdução a CloudWatch.  
  **Tarefa**: Assista à seção "CloudWatch" do curso de Maarek (20 min). Habilite CloudWatch Logs para a função Lambda do Projeto 2 e analise logs após uma execução (40 min).  
  **Entregável**: Logs visíveis no CloudWatch.  

- **Quinta, 14/08**  
  **Objetivo**: Introdução a AWS X-Ray.  
  **Tarefa**: Assista à seção "X-Ray" do curso de Neal Davis (20 min). Habilite X-Ray na função Lambda do Projeto 2 e analise traces (40 min).  
  **Entregável**: X-Ray configurado.  

- **Sexta, 15/08**  
  **Objetivo**: Finalizar Projeto 2.  
  **Tarefa**: Revise integração SQS-SNS-Lambda no curso de Maarek (20 min). Teste o Projeto 2 completo: envie mensagens via SDK Java, verifique processamento e notificações (40 min).  
  **Entregável**: Projeto 2 concluído.  

### Semana 5 (18 a 22 de agosto de 2025): Segurança
- **Segunda, 18/08**  
  **Objetivo**: Aprofundar em IAM avançado.  
  **Tarefa**: Assista à seção "Políticas IAM Avançadas" do curso de Neal Davis (20 min). Crie uma política IAM com condições (ex.: acesso ao DynamoDB apenas de uma VPC) para o Projeto 1 (40 min).  
  **Entregável**: Política IAM com condições configurada.  

- **Terça, 19/08**  
  **Objetivo**: Introdução a Amazon Cognito.  
  **Tarefa**: Assista à seção "Cognito" do curso de Maarek (20 min). Crie um User Pool no Cognito e configure autenticação para o endpoint `/orders` do Projeto 1 (40 min).  
  **Entregável**: Cognito configurado no Projeto 1.  

- **Quarta, 20/08**  
  **Objetivo**: Introdução a AWS KMS.  
  **Tarefa**: Assista à seção "KMS" do curso de Neal Davis (20 min). Crie uma chave KMS e use-a para criptografar dados na tabela `Pedidos` do Projeto 1 (40 min).  
  **Entregável**: Dados criptografados no DynamoDB.  

- **Quinta, 21/08**  
  **Objetivo**: Introdução a AWS Secrets Manager.  
  **Tarefa**: Assista à seção "Secrets Manager" do curso de Maarek (20 min). Armazene uma credencial no Secrets Manager e acesse-a via SDK Java na função Lambda do Projeto 1 (40 min).  
  **Entregável**: Secrets Manager integrado.  

- **Sexta, 22/08**  
  **Objetivo**: Revisar segurança no Projeto 1.  
  **Tarefa**: Revise práticas de segurança no curso de Neal Davis (20 min). Teste o Projeto 1 com Cognito, KMS e Secrets Manager, validando autenticação e criptografia (40 min).  
  **Entregável**: Projeto 1 com segurança implementada.  

### Semana 6 (25 a 29 de agosto de 2025): CI/CD
- **Segunda, 25/08**  
  **Objetivo**: Introdução a CodePipeline.  
  **Tarefa**: Assista à seção "CodePipeline" do curso de Maarek (20 min). Crie um pipeline simples no CodePipeline para implantar uma função Lambda (40 min).  
  **Entregável**: Pipeline básico configurado.  

- **Terça, 26/08**  
  **Objetivo**: Introdução a CodeBuild.  
  **Tarefa**: Assista à seção "CodeBuild" do curso de Neal Davis (20 min). Configure um projeto CodeBuild para compilar o código Java do Projeto 1 (40 min).  
  **Entregável**: Projeto CodeBuild configurado.  

- **Quarta, 27/08**  
  **Objetivo**: Integrar CodePipeline com SAM.  
  **Tarefa**: Revise integração CodePipeline-SAM no curso de Maarek (20 min). Modifique o pipeline para implantar o Projeto 1 usando SAM (40 min).  
  **Entregável**: Pipeline com SAM funcional.  

- **Quinta, 28/08**  
  **Objetivo**: Testar pipeline CI/CD.  
  **Tarefa**: Assista à seção "Boas Práticas CI/CD" do curso de Neal Davis (20 min). Faça uma alteração no código do Projeto 1 e teste o pipeline completo (40 min).  
  **Entregável**: Pipeline testado.  

- **Sexta, 29/08**  
  **Objetivo**: Iniciar Projeto 3 (Sistema de Processamento de Logs).  
  **Tarefa**: Assista à seção "CloudWatch Logs" do curso de Maarek (20 min). Crie uma tabela DynamoDB `Logs` (chave: `logId`) e uma função Lambda para processar POST `/logs` (40 min). Código:  
  ```java
  import com.amazonaws.services.lambda.runtime.Context;
  import com.amazonaws.services.lambda.runtime.RequestHandler;
  import software.amazon.awssdk.services.dynamodb.DynamoDbClient;
  import software.amazon.awssdk.services.dynamodb.model.PutItemRequest;
  import java.util.HashMap;
  import java.util.Map;

  public class LogHandler implements RequestHandler<Map<String, String>, String> {
      private final DynamoDbClient dynamoDb = DynamoDbClient.create();

      @Override
      public String handleRequest(Map<String, String> input, Context context) {
          Map<String, String> item = new HashMap<>();
          item.put("logId", input.get("logId"));
          item.put("message", input.get("message"));

          PutItemRequest request = PutItemRequest.builder()
                  . TableName("Logs")
                  .item(item)
                  .build();
          dynamoDb.putItem(request);
          context.getLogger().log("Log saved: " + input.get("logId"));
          return "Log processed";
      }
  }
  ```  
  **Entregável**: Projeto 3 iniciado.  

### Semana 7 (1 a 5 de setembro de 2025): Monitoramento e Projeto 3
- **Segunda, 01/09**  
  **Objetivo**: Configurar CloudWatch no Projeto 3.  
  **Tarefa**: Revise CloudWatch Logs no curso de Neal Davis (20 min). Habilite CloudWatch Logs para a função Lambda do Projeto 3 e analise logs após uma execução (40 min).  
  **Entregável**: Logs visíveis no CloudWatch.  

- **Terça, 02/09**  
  **Objetivo**: Adicionar X-Ray ao Projeto 3.  
  **Tarefa**: Assista à seção "X-Ray" do curso de Maarek (20 min). Habilite X-Ray na função Lambda do Projeto 3 e analise traces (40 min).  
  **Entregável**: X-Ray configurado.  

- **Quarta, 03/09**  
  **Objetivo**: Implantar Projeto 3 com SAM.  
  **Tarefa**: Revise SAM no curso de Neal Davis (20 min). Crie um `template.yaml` para o Projeto 3 (Lambda, DynamoDB, API Gateway) e implante com `sam deploy` (40 min).  
  **Entregável**: Projeto 3 implantado.  

- **Quinta, 04/09**  
  **Objetivo**: Testar Projeto 3.  
  **Tarefa**: Assista à seção "Depuração" do curso de Maarek (20 min). Teste o Projeto 3 via Postman (POST `/logs`) e valide logs no CloudWatch (40 min).  
  **Entregável**: Projeto 3 funcional.  

- **Sexta, 05/09**  
  **Objetivo**: Revisar monitoramento.  
  **Tarefa**: Revise CloudWatch e X-Ray no curso de Neal Davis (20 min). Adicione métricas personalizadas ao Projeto 3 (ex.: contagem de logs processados) usando SDK Java (40 min).  
  **Entregável**: Métricas personalizadas configuradas.  

### Semana 8 (8 a 12 de setembro de 2025): Projeto 4 (Sistema de Gerenciamento de Tarefas)
- **Segunda, 08/09**  
  **Objetivo**: Configurar recursos do Projeto 4.  
  **Tarefa**: Assista à seção "Integração de Serviços" do curso de Maarek (20 min). Crie uma tabela DynamoDB `Tasks` (chave: `taskId`), uma fila SQS `TaskQueue` e um tópico SNS `TaskTopic` (40 min).  
  **Entregável**: Recursos criados.  

- **Terça, 09/09**  
  **Objetivo**: Criar função Lambda para Projeto 4.  
  **Tarefa**: Revise integração Lambda-SQS no curso de Neal Davis (20 min). Crie uma função Lambda para POST `/tasks`, salvando no DynamoDB e enfileirando no SQS (40 min). Código:  
  ```java
  import com.amazonaws.services.lambda.runtime.Context;
  import com.amazonaws.services.lambda.runtime.RequestHandler;
  import software.amazon.awssdk.services.dynamodb.DynamoDbClient;
  import software.amazon.awssdk.services.sqs.SqsClient;
  import software.amazon.awssdk.services.dynamodb.model.PutItemRequest;
  import software.amazon.awssdk.services.sqs.model.SendMessageRequest;
  import java.util.HashMap;
  import java.util.Map;

  public class CreateTaskHandler implements RequestHandler<Map<String, String>, String> {
      private final DynamoDbClient dynamoDb = DynamoDbClient.create();
      private final SqsClient sqsClient = SqsClient.create();

      @Override
      public String handleRequest(Map<String, String> input, Context context) {
          Map<String, String> item = new HashMap<>();
          item.put("taskId", input.get("taskId"));
          item.put("description", input.get("description"));
          PutItemRequest putRequest = PutItemRequest.builder()
                  .tableName("Tasks")
                  .item(item)
                  .build();
          dynamoDb.putItem(putRequest);

          SendMessageRequest sqsRequest = SendMessageRequest.builder()
                  .queueUrl("https://sqs.us-east-1.amazonaws.com/123456789012/TaskQueue")
                  .messageBody(input.get("taskId"))
                  .build();
          sqsClient.sendMessage(sqsRequest);
          return "Task created: " + input.get("taskId");
      }
  }
  ```  
  **Entregável**: Função Lambda criada.  

- **Quarta, 10/09**  
  **Objetivo**: Integrar SQS e SNS no Projeto 4.  
  **Tarefa**: Assista à seção "SNS" do curso de Maarek (20 min). Crie uma função Lambda para consumir do `TaskQueue` e publicar no `TaskTopic` (40 min).  
  **Entregável**: Integração SQS-SNS configurada.  

- **Quinta, 11/09**  
  **Objetivo**: Implantar Projeto 4 com SAM.  
  **Tarefa**: Revise SAM no curso de Neal Davis (20 min). Crie um `template.yaml` para o Projeto 4 e implante com `sam deploy` (40 min).  
  **Entregável**: Projeto 4 implantado.  

- **Sexta, 12/09**  
  **Objetivo**: Testar Projeto 4.  
  **Tarefa**: Assista à seção "Testes de Integração" do curso de Maarek (20 min). Teste o Projeto 4 via Postman (POST `/tasks`) e valide mensagens no SNS (40 min).  
  **Entregável**: Projeto 4 funcional.  

### Semana 9 (15 a 19 de setembro de 2025): Otimização e Revisão
- **Segunda, 15/09**  
  **Objetivo**: Otimizar Lambda.  
  **Tarefa**: Assista à seção "Otimização de Lambda" do curso de Neal Davis (20 min). Ajuste a função Lambda do Projeto 4 (ex.: aumentar memória, reduzir tempo de execução) e teste (40 min).  
  **Entregável**: Função otimizada.  

- **Terça, 16/09**  
  **Objetivo**: Otimizar DynamoDB.  
  **Tarefa**: Assista à seção "Otimização de DynamoDB" do curso de Maarek (20 min). Adicione um índice secundário global à tabela `Tasks` e teste consultas (40 min).  
  **Entregável**: Índice configurado.  

- **Quarta, 17/09**  
  **Objetivo**: Revisar segurança.  
  **Tarefa**: Revise segurança no curso de Neal Davis (20 min). Adicione Cognito ao Projeto 4 para proteger o endpoint `/tasks` (40 min).  
  **Entregável**: Autenticação configurada.  

- **Quinta, 18/09**  
  **Objetivo**: Revisar monitoramento.  
  **Tarefa**: Assista à seção "CloudWatch Avançado" do curso de Maarek (20 min). Crie um alarme CloudWatch para monitorar falhas no Projeto 4 (40 min).  
  **Entregável**: Alarme configurado.  

- **Sexta, 19/09**  
  **Objetivo**: Revisar CI/CD.  
  **Tarefa**: Revise CI/CD no curso de Neal Davis (20 min). Adicione o Projeto 4 a um pipeline CodePipeline (40 min).  
  **Entregável**: Pipeline configurado.  

### Semana 10 (22 a 26 de setembro de 2025): Simulados e Revisão
- **Segunda, 22/09**  
  **Objetivo**: Primeiro simulado.  
  **Tarefa**: Faça 20 questões do simulado de Tutorials Dojo (20 min). Analise erros e revise tópicos no curso de Maarek (40 min).  
  **Entregável**: Análise de erros concluída.  

- **Terça, 23/09**  
  **Objetivo**: Revisar Lambda e API Gateway.  
  **Tarefa**: Assista à revisão "Lambda/API Gateway" do curso de Neal Davis (20 min). Teste o Projeto 1 via Postman (40 min).  
  **Entregável**: Projeto 1 revisado.  

- **Quarta, 24/09**  
  **Objetivo**: Revisar DynamoDB e SDK.  
  **Tarefa**: Assista à revisão "DynamoDB" do curso de Maarek (20 min). Adicione uma consulta complexa ao Projeto 1 usando SDK Java (40 min).  
  **Entregável**: Consulta implementada.  

- **Quinta, 25/09**  
  **Objetivo**: Revisar SQS/SNS.  
  **Tarefa**: Assista à revisão "SQS/SNS" do curso de Neal Davis (20 min). Teste o Projeto 2 enviando mensagens via SDK Java (40 min).  
  **Entregável**: Projeto 2 revisado.  

- **Sexta, 26/09**  
  **Objetivo**: Segundo simulado.  
  **Tarefa**: Faça 20 questões do simulado de Neal Davis (20 min). Analise erros e revise tópicos no curso de Maarek (40 min).  
  **Entregável**: Análise de erros concluída.  

### Semana 11 (29 de setembro a 3 de outubro de 2025): Simulados e Projetos
- **Segunda, 29/09**  
  **Objetivo**: Revisar Projeto 3.  
  **Tarefa**: Revise CloudWatch/X-Ray no curso de Neal Davis (20 min). Teste o Projeto 3 e analise logs/traces (40 min).  
  **Entregável**: Projeto 3 revisado.  

- **Terça, 30/09**  
  **Objetivo**: Revisar Projeto 4.  
  **Tarefa**: Revise integração de serviços no curso de Maarek (20 min). Teste o Projeto 4 completo via Postman (40 min).  
  **Entregável**: Projeto 4 revisado.  

- **Quarta, 01/10**  
  **Objetivo**: Terceiro simulado.  
  **Tarefa**: Faça 20 questões do simulado de Maarek (20 min). Analise erros e revise tópicos no curso de Neal Davis (40 min).  
  **Entregável**: Análise de erros concluída.  

- **Quinta, 02/10**  
  **Objetivo**: Revisar segurança.  
  **Tarefa**: Assista à seção "KMS/Secrets Manager" do curso de Maarek (20 min). Adicione KMS ao Projeto 4 para criptografar dados (40 min).  
  **Entregável**: Criptografia configurada.  

- **Sexta, 03/10**  
  **Objetivo**: Revisar CI/CD.  
  **Tarefa**: Assista à revisão "CodePipeline" do curso de Neal Davis (20 min). Teste o pipeline do Projeto 4 com uma alteração de código (40 min).  
  **Entregável**: Pipeline testado.  

### Semana 12 (6 a 10 de outubro de 2025): Simulados Completos
- **Segunda, 06/10**  
  **Objetivo**: Simulado completo Tutorials Dojo.  
  **Tarefa**: Faça um simulado completo de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído.  

- **Terça, 07/10**  
  **Objetivo**: Reforçar Lambda avançado.  
  **Tarefa**: Assista à seção "Lambda Avançado" do curso de Neal Davis (20 min). Adicione camadas ao Projeto 4 (ex.: biblioteca externa) (40 min).  
  **Entregável**: Camada configurada.  

- **Quarta, 08/10**  
  **Objetivo**: Simulado completo Neal Davis.  
  **Tarefa**: Faça um simulado completo de Neal Davis (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído.  

- **Quinta, 09/10**  
  **Objetivo**: Reforçar DynamoDB avançado.  
  **Tarefa**: Assista à seção "DynamoDB Avançado" do curso de Maarek (20 min). Adicione uma transação ao Projeto 4 (ex.: criar tarefa e log) (40 min).  
  **Entregável**: Transação implementada.  

- **Sexta, 10/10**  
  **Objetivo**: Simulado completo Maarek.  
  **Tarefa**: Faça um simulado completo de Maarek (40 min). Analise erros e revise tópicos no curso de Neal Davis (20 min).  
  **Entregável**: Simulado concluído.  

### Semana 13 (13 a 17 de outubro de 2025): Revisão Geral
- **Segunda, 13/10**  
  **Objetivo**: Revisar Desenvolvimento.  
  **Tarefa**: Assista à revisão "Lambda/API Gateway" do curso de Neal Davis (20 min). Teste os Projetos 1 e 4 via Postman (40 min).  
  **Entregável**: Projetos revisados.  

- **Terça, 14/10**  
  **Objetivo**: Revisar Segurança.  
  **Tarefa**: Assista à revisão "IAM/Cognito" do curso de Maarek (20 min). Valide autenticação no Projeto 4 (40 min).  
  **Entregável**: Autenticação revisada.  

- **Quarta, 15/10**  
  **Objetivo**: Revisar Implantação.  
  **Tarefa**: Assista à revisão "CI/CD" do curso de Neal Davis (20 min). Teste o pipeline do Projeto 4 (40 min).  
  **Entregável**: Pipeline revisado.  

- **Quinta, 16/10**  
  **Objetivo**: Revisar Solução de Problemas.  
  **Tarefa**: Assista à revisão "CloudWatch/X-Ray" do curso de Maarek (20 min). Analise logs/traces do Projeto 3 (40 min).  
  **Entregável**: Monitoramento revisado.  

- **Sexta, 17/10**  
  **Objetivo**: Simulado completo Tutorials Dojo.  
  **Tarefa**: Faça um simulado completo de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Neal Davis (20 min).  
  **Entregável**: Simulado concluído.  

### Semana 14 (20 a 24 de outubro de 2025): Simulados e Reforço
- **Segunda, 20/10**  
  **Objetivo**: Simulado completo Neal Davis.  
  **Tarefa**: Faça um simulado completo de Neal Davis (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído.  

- **Terça, 21/10**  
  **Objetivo**: Reforçar Lambda/SAM.  
  **Tarefa**: Assista à revisão "SAM" do curso de Neal Davis (20 min). Reimplante o Projeto 4 com uma pequena alteração (40 min).  
  **Entregável**: Projeto revisado.  

- **Quarta, 22/10**  
  **Objetivo**: Simulado completo Maarek.  
  **Tarefa**: Faça um simulado completo de Maarek (40 min). Analise erros e revise tópicos no curso de Neal Davis (20 min).  
  **Entregável**: Simulado concluído.  

- **Quinta, 23/10**  
  **Objetivo**: Reforçar segurança.  
  **Tarefa**: Assista à revisão "KMS/Secrets Manager" do curso de Maarek (20 min). Adicione Secrets Manager ao Projeto 4 (40 min).  
  **Entregável**: Secrets Manager integrado.  

- **Sexta, 24/10**  
  **Objetivo**: Simulado completo Tutorials Dojo.  
  **Tarefa**: Faça um simulado completo de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Neal Davis (20 min).  
  **Entregável**: Simulado concluído.  

### Semana 15 (27 a 31 de outubro de 2025): Preparação Final
- **Segunda, 27/10**  
  **Objetivo**: Revisar todos os projetos.  
  **Tarefa**: Revise anotações dos Projetos 1 a 4 (20 min). Teste todos os projetos via Postman (40 min).  
  **Entregável**: Projetos revisados.  

- **Terça, 28/10**  
  **Objetivo**: Simulado completo Neal Davis.  
  **Tarefa**: Faça um simulado completo de Neal Davis (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído.  

- **Quarta, 29/10**  
  **Objetivo**: Revisar tópicos fracos.  
  **Tarefa**: Identifique tópicos fracos nos simulados anteriores (20 min). Estude esses tópicos no curso de Neal Davis ou Maarek (40 min).  
  **Entregável**: Tópicos fracos revisados.  

- **Quinta, 30/10**  
  **Objetivo**: Simulado completo Tutorials Dojo.  
  **Tarefa**: Faça um simulado completo de Tutorials Dojo (40 min). Analise erros e revise tópicos no curso de Maarek (20 min).  
  **Entregável**: Simulado concluído.  

- **Sexta, 31/10**  
  **Objetivo**: Preparação final.  
  **Tarefa**: Revise o Guia do Exame da AWS (20 min, https://d1.awsstatic.com/training-and-certification/docs-dev-associate/AWS-Certified-Developer-Associate_Exam-Guide.pdf). Faça 10 questões de cada simulado (Tutorials Dojo, Neal Davis, Maarek) (40 min).  
  **Entregável**: Preparação finalizada.  

## Dicas Finais
- **Ambiente AWS**: Monitore custos no AWS Budgets. Exclua recursos após os projetos para evitar encargos.  
- **Simulados**: Anote tópicos fracos (ex.: KMS, X-Ray) a partir da Semana 10. Revise-os prioritariamente.  
- **Projetos**: Mantenha o código em um repositório Git (local ou GitHub). Exemplo: `git commit -m "Projeto 1 concluído"`.  
- **Prova**: Agende o exame para 3-7 de novembro de 2025 via Pearson VUE. Escolha o idioma português, mas consulte termos técnicos em inglês se necessário.  
- **Descanso**: Reserve o fim de semana antes da prova para descansar e revisar anotações leves.  
- **Exportação**: Para transferir este cronograma, salve como `aws-developer-study-plan.md`. Copie para um USB, envie por e-mail ou publique no GitHub. Use um editor Markdown ou converta para PDF com Pandoc.
