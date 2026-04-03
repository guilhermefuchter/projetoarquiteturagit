# ADR-001: Estratégia de Ingestão de Dados

**Status:** Aceito  
**Data:** 2026-04-03  
**Autores:** Guilherme Fuchter, Andre Moura, Vitor Lamin, Nathan Campos

---

## Contexto

A plataforma VendaMais precisa receber dados de múltiplas fontes externas (sistemas de CRM, ERPs de clientes, planilhas de vendas e webhooks de e-commerce). Esses dados chegam em diferentes frequências — alguns em tempo quase real (webhooks), outros em lotes periódicos (exportações diárias de ERP).

A equipe precisou decidir qual estratégia de ingestão seria mais adequada considerando: custo operacional, escalabilidade, manutenção e compatibilidade com o ecossistema Azure já definido para o projeto.

---

## Alternativas Analisadas

### Alternativa 1 — Azure Functions (Serverless) ✅ *Escolhida*

Funções serverless acionadas por eventos (HTTP trigger, Timer trigger, Queue trigger) que processam e encaminham os dados para o armazenamento.

**Prós:**
- Custo baseado em execução (paga apenas pelo uso)
- Escala automaticamente conforme volume
- Integração nativa com outros serviços Azure (Service Bus, Storage, SQL)
- Baixa manutenção de infraestrutura

**Contras:**
- Cold start pode introduzir latência na primeira execução
- Limites de tempo de execução (máx. 10 min no plano Consumption)
- Depuração e monitoramento exigem configuração adicional (Application Insights)

---

### Alternativa 2 — Azure Logic Apps

Fluxos de integração low-code/no-code com conectores prontos para diversas fontes de dados.

**Prós:**
- Configuração visual, sem necessidade de código
- Conectores nativos para centenas de sistemas (Salesforce, SAP, etc.)
- Bom para integrações simples e rápidas

**Contras:**
- Custo por execução pode ser alto em volumes elevados
- Pouca flexibilidade para lógicas de transformação complexas
- Difícil de versionar e revisar em código (não segue bem práticas de Git)

---

### Alternativa 3 — Azure Data Factory (ADF)

Serviço gerenciado de ETL/ELT para pipelines de dados em larga escala.

**Prós:**
- Ideal para grandes volumes de dados em batch
- Interface visual para construção de pipelines
- Integração nativa com Data Lake e Synapse Analytics

**Contras:**
- Custo fixo mais elevado mesmo em baixo uso
- Latência alta — não adequado para ingestão em tempo real
- Complexidade desnecessária para o escopo atual do VendaMais

---

### Alternativa 4 — API Gateway + Aplicação Dedicada (AKS/App Service)

Uma API REST própria que recebe os dados e os processa continuamente em um container ou app service.

**Prós:**
- Controle total sobre a lógica de ingestão
- Sem limitações de tempo de execução

**Contras:**
- Custo fixo independente do volume de requisições
- Necessidade de gerenciar disponibilidade, escalabilidade e infraestrutura
- Maior overhead de desenvolvimento e operação

---

## Decisão

**Escolhemos Azure Functions (Serverless).**

A estratégia de ingestão via Azure Functions foi selecionada por oferecer o melhor equilíbrio entre custo, escalabilidade e simplicidade operacional para o contexto do VendaMais. O modelo serverless elimina a necessidade de gerenciar servidores, escala automaticamente conforme a demanda e possui integração nativa com o restante do ecossistema Azure. Os gatilhos disponíveis (HTTP, Timer, Queue) cobrem tanto o cenário de webhooks em tempo real quanto o de lotes periódicos provenientes de ERPs.

---

## Consequências

### Positivas
- Redução de custo operacional — pagamento apenas pelo uso real
- Escalabilidade automática sem intervenção da equipe
- Código versionável e testável, seguindo boas práticas de engenharia
- Integração direta com Azure Service Bus para desacoplamento entre ingestão e processamento

### Negativas
- Necessidade de configurar monitoramento via Application Insights
- Cold start pode impactar a latência em cenários de baixa frequência
- Limite de 10 minutos por execução requer atenção para lotes muito grandes
