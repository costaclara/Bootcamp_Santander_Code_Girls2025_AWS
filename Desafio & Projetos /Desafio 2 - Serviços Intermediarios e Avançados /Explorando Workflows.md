# Desafio 2 - Explorando Workflows Automatizados com AWS Step Functions

## O que é?

O **AWS Step Functions** é um serviço da **Amazon Web Services (AWS)** que permite **orquestrar múltiplos serviços e funções** em um **fluxo de trabalho automatizado e visual**, conhecido como *state machine* (máquina de estados).
Ele é amplamente utilizado para coordernar **funções AWS Lambda**, executar **processos assíncronos e paralelo**, e **controlar a lógica de decisão** em aplicações ditribuídas, tudo isso sem precisar gerenciar servidores.

## Como funciona?

O Step Funtions funciona como um **orquestrador**: ele controla a ordem de execução das tarefas (como funções Lambda, chamadas a APIs ou serviços AWS) e mantém o estado entre uma etapa e outra.

Você define o fluxo em JSON, usando a linguagem Amazon States Languages (ASL), especificando:
- A sequência das etapas (estados);
- As condições de transição;
- O tratamento de erros e exceções;
- A lógica de repetição ou paralelismo.

Durante a execução, o Step Funtions registra cada etapa, permitindo **monitoramento de depuração visual** no console da AWS.

## Componentes de um Workflow no AWS Step Funtions

| Componente | Descrição |
| ---------- | --------- |
| State Machine | Define o fluxo completo de execução (workflow). |
| State (Estado) | Cada passo individual dentro da máquina. Pode representar uma tarefa, decisão, pausa, entre outros. |
| Task State | Executa uma tarefa específica, como chamar uma função Lambda ou um serviço AWS. |
| Choice State | Define caminhos diferentes com base em condições (if/else). |
| Parallel State | Permite executar várias tarefas simultaneamente. |
| Wait State | Adiciona uma pausa antes da próxima etapa. |
| Pass State | Passa dados adiante sem executar nenhuma ação. |
| Fail/Succeed State | Determina o término do fluxo com sucesso ou falha. |

## Vantagens

- **Serverless e totalmente gerenciado**: sem necessidade de infraestrutura.
- **Monitoramento visual**: permite acompanhar cada execução em tempo real.
- **Integração nativa com diversos serviços AWS**: Lambda, DynamoDB, ECS, SNS, SQS, Glue etc.
- **Tolerância a falhas**: permite reexecuções automáticas, *catch* e *retry*.
- **Escalabilidade automática**: ajusta-se à demanda sem intervenção manual.
- **Baixo acoplamento**: facilita a manutenção e evolução das aplicações.

## Exemplos de uso

- **Processamento de pedidos** → orquestra várias etapas: validação, cobraça, estoque e notificação.
- **Data pipeline (ETL)** → coordena jobs de ingestão, transformação e carregamento de dados.
- **Automação de processos internos** → como aprovação de documentos ou execução de rotinas administrativas.
- **Treinamento de modelos de Machine Learning** → executa sequencialmente coleta de dados, treinamento e avaliação.
- **Integração entre sistemas** → conecta APIs e microserviços de forma controlada e auditável.

## Lições aprendidas

- O Step Functions é mais do que um orquestrador: ele traz clareza, rastreabilidade e resiliência para fluxos complexos.
- A linguagem declarativa (ASL) facilita a manutenção e documentação dos processos.
- Funções Lambda se encaixam perfeitamente nesse modelo, reduzindo a complexidade do código.
- A visualização gráfica ajuda a identificar gargalos e falhas rapidamente.
- É uma excelente escolha para microserviços, event-driven architectures e pipelines de dados.

## Fontes

- [AWS Step Functions](https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html)
- [AWS Step Functions Developer Guide(Amazon States Languages)](https://states-language.net/spec.html)
- [AWS Blog — Step Functions Use Cases](https://aws.amazon.com/blogs/compute/category/step-functions)
- [AWS Samples on GitHub](https://github.com/aws-samples)
