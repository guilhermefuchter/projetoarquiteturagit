  # ADR-002: Estratégia de Armazenamento de Dados

**Status:** Aceito  
**Data:** 2026-04-03  
**Autores:** Guilherme Fuchter, Andre Moura, Vitor Lamin, Nathan Campos

---

## Contexto

A plataforma VendaMais precisa armazenar dados estruturados provenientes de múltiplas fontes: informações de clientes, pedidos, produtos, metas e desempenho de vendas. Esses dados são consumidos tanto por relatórios analíticos quanto por operações transacionais da plataforma.

A equipe precisou decidir qual serviço de armazenamento seria mais adequado, levando em conta: consistência dos dados, capacidade de consultas relacionais, custo, escalabilidade e integração com o ecossistema Azure.

---

## Alternativas Analisadas

### Alternativa 1 — Azure SQL Database ✅ *Escolhida*

Banco de dados relacional gerenciado baseado em SQL Server, hospedado na nuvem Azure.

**Prós:**
- Suporte completo a SQL padrão — consultas relacionais complexas, joins, transações ACID
- Totalmente gerenciado: backups automáticos, alta disponibilidade e patches sem intervenção
- Integração nativa com Azure Functions, Logic Apps e Power BI
- Escalabilidade vertical e horizontal via elastic pools
- Segurança robusta: criptografia em repouso e em trânsito, integração com Azure Active Directory

**Contras:**
- Custo pode crescer com aumento de DTUs/vCores
- Não é ideal para dados não estruturados (documentos, blobs, JSON livre)
- Menos flexível para esquemas que mudam com frequência

---

### Alternativa 2 — Azure Cosmos DB

Banco de dados NoSQL multimodelo, distribuído globalmente, com suporte a documentos, grafos e chave-valor.

**Prós:**
- Latência muito baixa (< 10ms garantido por SLA)
- Escala horizontal automática
- Flexibilidade de esquema — ideal para dados heterogêneos
- Distribuição global nativa

**Contras:**
- Custo baseado em RUs (Request Units) pode ser imprevisível e alto
- Não suporta joins relacionais nativos — consultas complexas exigem desnormalização
- Curva de aprendizado maior para equipes acostumadas com SQL
- Consistência eventual por padrão (requer configuração para forte consistência)

---

### Alternativa 3 — Azure Blob Storage + Azure Data Lake Storage Gen2

Armazenamento de objetos de baixo custo, frequentemente usado como data lake para análises em larga escala.

**Prós:**
- Custo muito baixo para grandes volumes de dados
- Ideal para dados brutos, arquivos, imagens e processamento analítico em batch
- Integração com Azure Synapse e Databricks para análises avançadas

**Contras:**
- Não é um banco de dados — não suporta consultas SQL relacionais diretamente
- Requer camada adicional (ex: Synapse Serverless) para consultas analíticas
- Não adequado para operações transacionais (CRUD em tempo real)

---

### Alternativa 4 — PostgreSQL no Azure (Azure Database for PostgreSQL)

Banco de dados relacional open-source gerenciado no Azure.

**Prós:**
- Open-source, sem licenciamento proprietário
- Suporte a JSON nativo (jsonb), extensões e tipos avançados
- Bom custo-benefício

**Contras:**
- Integração com ecossistema Azure menos nativa que o Azure SQL
- Menor suporte a ferramentas Microsoft (Power BI, SSMS, etc.)
- A equipe tem maior familiaridade com SQL Server

---

## Decisão

**Escolhemos Azure SQL Database.**

O Azure SQL Database foi selecionado por ser a opção que melhor atende ao perfil de dados do VendaMais: dados relacionais e estruturados (clientes, pedidos, metas, vendas) com necessidade de consultas complexas e integridade transacional. A integração nativa com o restante do ecossistema Azure — especialmente com as Azure Functions escolhidas no ADR-001 — simplifica o desenvolvimento. A gestão totalmente automática (backups, HA, patches) reduz o overhead operacional da equipe.

---

## Consequências

### Positivas
- Integridade referencial garantida via chaves estrangeiras e transações ACID
- Consultas analíticas e operacionais no mesmo banco, sem necessidade de camada extra
- Integração direta com Power BI para dashboards de desempenho de vendas
- Backup automático e recuperação pontual (PITR) sem configuração adicional
- Equipe familiarizada com SQL, reduzindo curva de aprendizado

### Negativas
- Custo cresce proporcionalmente ao aumento de capacidade (vCores/DTUs)
- Esquema relacional exige migrações cuidadosas a cada mudança de modelo de dados
- Não é a melhor opção caso o volume de dados cresça para escala de petabytes (nesse caso, migraria para Synapse)
