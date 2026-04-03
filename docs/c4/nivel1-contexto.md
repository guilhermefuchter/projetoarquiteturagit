# C4 Nível 1 — Diagrama de Contexto

Este diagrama apresenta o sistema VendaMais em seu contexto, mostrando os usuários que interagem com ele e os sistemas externos com os quais se integra.

---  

```mermaid
flowchart TD
    vendedor["👤 Vendedor\nAcompanha metas, pedidos e desempenho de vendas pelo painel."]
    gestor["👤 Gestor Comercial\nVisualiza relatórios consolidados e define metas da equipe."]
    admin["👤 Administrador\nGerencia usuários, integrações e configurações da plataforma."]

    vendamais["🖥️ VendaMais\nPlataforma de apoio a vendas que centraliza dados de clientes, pedidos e desempenho comercial."]

    crm["⬜ Sistema CRM\nFornece dados de clientes e oportunidades (ex: Salesforce)."]
    erp["⬜ Sistema ERP\nExporta dados de pedidos, produtos e estoque em lotes periódicos."]
    ecommerce["⬜ Plataforma E-commerce\nEnvia eventos de pedidos em tempo real via webhook."]
    powerbi["⬜ Power BI\nConsome dados do VendaMais para dashboards executivos."]
    email["⬜ Serviço de E-mail\nEnvia notificações e alertas para vendedores e gestores."]

    vendedor -->|"Acessa via navegador web"| vendamais
    gestor -->|"Acessa via navegador web"| vendamais
    admin -->|"Gerencia via painel administrativo"| vendamais

    crm -->|"API REST / JSON"| vendamais
    erp -->|"Arquivo CSV / Batch diário"| vendamais
    ecommerce -->|"Webhook HTTPS"| vendamais
    vendamais -->|"Azure SQL / Conector nativo"| powerbi
    vendamais -->|"SMTP / SendGrid"| email

    style vendamais fill:#1168BD,color:#fff,stroke:#0e57a0
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

## Descrição dos Elementos

### Usuários

| Ator | Descrição |
|---|---|
| **Vendedor** | Usuário principal da plataforma. Acompanha suas metas, carteira de clientes e pedidos em aberto. |
| **Gestor Comercial** | Visualiza o desempenho consolidado da equipe e define metas. |
| **Administrador** | Responsável por configurar integrações, usuários e permissões. |

### Sistemas Externos

| Sistema | Papel |
|---|---|
| **CRM (ex: Salesforce)** | Fonte de dados de clientes e oportunidades comerciais. |
| **ERP** | Fonte de dados de pedidos, produtos e estoque, enviados em lote. |
| **E-commerce** | Fonte de eventos de pedidos em tempo real via webhook. |
| **Power BI** | Consome os dados do VendaMais para relatórios executivos. |
| **Serviço de E-mail** | Canal de notificações e alertas da plataforma. |
