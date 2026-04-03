# C4 Nível 2 — Diagrama de Containers

Este diagrama decompõe o sistema VendaMais em seus containers principais, mostrando as tecnologias utilizadas e as responsabilidades de cada componente.

---  

```mermaid
flowchart TD
    vendedor["👤 Vendedor"]
    gestor["👤 Gestor Comercial"]
    admin["👤 Administrador"]

    crm["⬜ CRM Externo\nSalesforce ou similar"]
    erp["⬜ ERP Externo\nExportações em lote CSV"]
    ecommerce["⬜ E-commerce\nEventos via webhook"]
    powerbi["⬜ Power BI\nDashboards executivos"]
    email["⬜ Serviço de E-mail\nSendGrid / SMTP"]

    subgraph VM ["🔷 VendaMais"]
        webapp["🌐 Aplicação Web\nReact / TypeScript"]
        api["⚙️ API Backend\nNode.js / Express"]
        ingestao["⚡ Serviço de Ingestão\nAzure Functions · Python"]
        fila["📨 Fila de Mensagens\nAzure Service Bus"]
        db["🗄️ Banco de Dados\nAzure SQL Database"]
        storage["📦 Armazenamento de Arquivos\nAzure Blob Storage"]
        auth["🔐 Autenticação\nAzure AD B2C"]
    end

    vendedor -->|"HTTPS"| webapp
    gestor -->|"HTTPS"| webapp
    admin -->|"HTTPS"| webapp

    webapp -->|"REST / JSON"| api
    webapp -->|"OAuth 2.0 / OIDC"| auth
    api -->|"JWT"| auth
    api -->|"SQL / TDS"| db
    api -->|"API SendGrid"| email

    crm -->|"REST / JSON"| ingestao
    ecommerce -->|"Webhook HTTPS"| ingestao
    erp -->|"SFTP / Upload"| storage
    ingestao -->|"Azure SDK"| storage
    ingestao -->|"AMQP"| fila
    fila -->|"AMQP"| api

    db -->|"Conector nativo Azure SQL"| powerbi

    style webapp fill:#1168BD,color:#fff,stroke:#0e57a0
    style api fill:#1168BD,color:#fff,stroke:#0e57a0
    style ingestao fill:#1168BD,color:#fff,stroke:#0e57a0
    style fila fill:#1168BD,color:#fff,stroke:#0e57a0
    style db fill:#1168BD,color:#fff,stroke:#0e57a0
    style storage fill:#1168BD,color:#fff,stroke:#0e57a0
    style auth fill:#1168BD,color:#fff,stroke:#0e57a0
    style vendedor fill:#08427B,color:#fff,stroke:#063561
    style gestor fill:#08427B,color:#fff,stroke:#063561
    style admin fill:#08427B,color:#fff,stroke:#063561
    style crm fill:#6c757d,color:#fff,stroke:#565e64
    style erp fill:#6c757d,color:#fff,stroke:#565e64
    style ecommerce fill:#6c757d,color:#fff,stroke:#565e64
    style powerbi fill:#6c757d,color:#fff,stroke:#565e64
    style email fill:#6c757d,color:#fff,stroke:#565e64
```

--- 

## Descrição dos Containers

| Container | Tecnologia | Responsabilidade |
|---|---|---|
| **Aplicação Web** | React / TypeScript | Interface do usuário — dashboards, metas, pedidos e configurações. |
| **API Backend** | Node.js / Express | Regras de negócio, endpoints REST, orquestração entre serviços. |
| **Serviço de Ingestão** | Azure Functions (Python) | Recebe dados externos, normaliza e publica na fila. Acionado por eventos (HTTP, Timer, Queue). |
| **Fila de Mensagens** | Azure Service Bus | Desacopla ingestão e processamento, garantindo entrega confiável e resiliência. |
| **Banco de Dados** | Azure SQL Database | Persistência central de todos os dados relacionais da plataforma. |
| **Armazenamento de Arquivos** | Azure Blob Storage | Recebe arquivos CSV do ERP e armazena logs de processamento. |
| **Autenticação** | Azure AD B2C | Gerencia identidades, login, tokens JWT e controle de acesso. |
