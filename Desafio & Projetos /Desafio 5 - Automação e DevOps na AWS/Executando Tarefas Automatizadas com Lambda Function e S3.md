Desafio 5 - 

## Conceitos Fundamentais

### **AWS Lambda**

O **AWS Lambda** é um serviço de **computação serverless** que executa código em resposta a eventos, sem necessidade de gerenciar servidores.  
É possível definir **funções** que são acionadas por gatilhos (como eventos do S3, DynamoDB, API Gateway etc.), sendo cobradas apenas pelo tempo de execução e uso de recursos.  
Ele é ideal para **tarefas automatizadas, pipelines de dados e integrações event-driven**, com alta escalabilidade e baixo custo operacional.

### **Amazon S3**

O **Amazon Simple Storage Service (S3)** é um serviço de **armazenamento de objetos** seguro, durável e escalável.  
Os arquivos (objetos) são armazenados em **buckets**, e cada ação no bucket — como upload ou exclusão — pode gerar **eventos** que disparam funções Lambda.  
Essa integração é amplamente usada para **processamento automático de arquivos**, **tratamento de imagens**, **ETL de dados** e muito mais.

###  **Integração Lambda + S3**  
A integração entre Lambda e S3 permite que funções sejam disparadas automaticamente quando eventos ocorrem em um bucket.  
Exemplos de uso:

- Processar imagens após upload (redimensionar, converter, etc.)  
- Validar ou transformar arquivos CSV antes de armazenar em banco  
- Gerar notificações ou logs quando arquivos são adicionados  
- Criar thumbnails de vídeos ou imagens  
- Indexar documentos para busca

### Upload de Arquivos com Processamento e Registro no DynamoDB

### **Fluxo de execução**

1. O usuário faz **upload de um arquivo** (por exemplo, `dados.csv`) em um bucket do S3.
2. O evento `ObjectCreated` é disparado, **acionando automaticamente a função Lambda**.
3. A função Lambda **lê o arquivo**, processa as informações necessárias e **armazena os metadados** (como nome, tamanho e data do processamento) em uma tabela do **DynamoDB**.
4. O resultado do processamento pode ser salvo em outro bucket S3 ou usado em outros serviços (como SNS ou CloudWatch).

### **Caso de uso simulado**

Um pipeline automatizado que registra uploads em tempo real:

- **Entrada:** Arquivo adicionado ao S3.
- **Processamento:** Lambda extrai informações do arquivo.
- **Saída:** DynamoDB recebe um registro com status de processamento.

Esse fluxo permite rastrear uploads, criar logs automatizados e manter consistência nos dados — tudo sem precisar de infraestrutura dedicada.

---

## Serviços AWS utilizados

- **S3** → Armazenamento e gatilho de evento.
- **Lambda** → Execução automatizada de código.
- **DynamoDB** → Registro e persistência de metadados.
- **IAM** → Controle de permissões e roles de acesso.
- **CloudWatch** → Logs e monitoramento.
- **LocalStack** → Ambiente local para testes sem custos AWS reais.

---

## AWS Local com LocalStack

O **LocalStack** simula localmente os serviços da AWS, permitindo desenvolver e testar a integração Lambda + S3 + DynamoDB sem custo.

### Exemplo de configuração (`docker-compose.yml`)

```yaml
version: "3.8"
services:
  localstack:
    image: localstack/localstack:2.1
    environment:
      - SERVICES=s3,lambda,dynamodb,iam,cloudwatch
      - DEFAULT_REGION=us-east-1
    ports:
      - "4566:4566"
    volumes:
      - "./localstack:/tmp/localstack"
      - "/var/run/docker.sock:/var/run/docker.sock"
```

## Passo a passo da implementação

1. **Criar recursos básicos**
   - Bucket S3 (`meu-bucket-uploads`)
   - Tabela DynamoDB (`Processos`) com chave primária `FileId`.
2. **Criar função Lambda**
   - Código processa o evento do S3, extrai metadados e grava registro no DynamoDB.
3. **Configurar trigger**
   - Adicionar evento `s3:ObjectCreated:*` para invocar a Lambda.
4. **Testar fluxo**
   - Fazer upload de arquivo → Lambda executa → Registro criado no DynamoDB.
5. **Verificar logs**
   - Monitorar saídas e erros no CloudWatch Logs ou console local.

## Aprendizados

- **Automação sem servidores**: Lambda + S3 elimina a necessidade de infraestrutura tradicional.
- **Escalabilidade automática**: Cada upload aciona uma instância independente da função.
- **Custo otimizado**: Pagamento apenas por execução, ideal para workloads eventuais.
- **Desenvolvimento local**: LocalStack acelera testes e integração antes do deploy.
- **Boas práticas**:
  - Manter funções idempotentes (evitar registros duplicados).
  - Restringir permissões IAM seguindo o princípio do menor privilégio.
  - Usar logs e métricas para monitorar desempenho e falhas.

## Fontes

- [AWS Lambda - Documentação Oficial](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)
- [Amazon S3 - Documentação Oficial](https://docs.aws.amazon.com/s3/index.html)
- [Amazon DynamoDB - DOcumentação Oficial](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)
- [LocalStack - Documentação Oficial](https://docs.localstack.cloud/)
- [Exemplo AWS - S3 Trigger Lambda](https://docs.aws.amazon.com/lambda/latest/dg/with-s3-example.html)
