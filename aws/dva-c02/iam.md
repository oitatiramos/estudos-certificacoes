# 🌩️ AWS IAM - Usuários, Grupos e Políticas

Este é um resumo leve e divertido para entender os conceitos básicos de **IAM (Identity and Access Management)** na AWS.

---

## 📘 Explicação Técnica

### 👤 Usuários (Users)
- Representam pessoas reais ou sistemas.
- Possuem credenciais (usuário/senha ou chaves de acesso).
- Criados no IAM para acessar recursos da AWS.

### 👥 Grupos (Groups)
- Coleção de usuários com as mesmas permissões.
- Permite gerenciamento em lote de acessos.
- Exemplo: grupo "Desenvolvedores" com acesso a EC2 e S3.

### 📜 Políticas (Policies)
- Documentos JSON que definem permissões.
- Dizem o que é permitido ou negado.
- Podem ser aplicadas a usuários, grupos ou roles.

---

## 🧒 Explicação para Crianças

### 🎢 AWS é como um parque de diversões digital!

Imagine que você entrou num parque mágico cheio de brinquedos:
- 🖥️ Computadores (EC2)
- 🏰 Castelo de arquivos (S3)
- 📬 Sala de mensagens mágicas (SNS)

Agora, veja como funciona:

### 🧍‍♀️ **Usuário**
> É como uma criança com um **crachá** dizendo quem ela é.

- Ex: "Essa é a Tati!"
- O crachá dá acesso aos brinquedos que ela tem permissão.

### 👯‍♀️ **Grupo**
> É como uma **excursão da escola**.

- Todos da turma recebem o mesmo conjunto de regras.
- Ex: Turma "Desenvolvedores" pode brincar na montanha-russa e no castelo.

### 📖 **Política**
> É o **livro de regras** do parque.

- Diz o que pode ou não pode fazer:
  - ✅ Pode brincar no castelo
  - ❌ Não pode subir na torre sozinha
- As regras são coladas no crachá do usuário ou do grupo.

---

## 🧠 Resumo Visual

| Conceito | Explicação técnica | Explicação infantil |
|---------|--------------------|---------------------|
| **Usuário** | Pessoa ou sistema com credenciais | Criança com crachá |
| **Grupo** | Coleção de usuários com as mesmas regras | Turma da escola |
| **Política** | Regras em JSON que controlam acesso | Livro de regras do parque |

---

### ✨ Dica para a prova AWS Developer:
- **IAM é um serviço global**: não depende de regiões.
- Você usa IAM para controlar **quem pode fazer o quê, e onde**.
