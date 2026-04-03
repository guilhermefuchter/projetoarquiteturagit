# C4 Nível 1 — Diagrama de Contexto

Este diagrama apresenta o sistema VendaMais em seu contexto, mostrando os usuários que interagem com ele e os sistemas externos com os quais se integra.

---

```mermaid
C4Context
    title Diagrama de Contexto — VendaMais

    Person(vendedor, "Vendedor", "Acompanha metas, pedidos e desempenho de vendas pelo painel.")
    Person(gestor, "Gestor Comercial", "Visualiza relatórios consolidados e define metas da equipe.")
    Person(admin, "Administrador", "Gerencia usuários, integrações e configurações da plataforma.")

    System(vendamais, "VendaMais", "Plataforma de apoio a vendas que centraliza dados de clientes, pedidos e desempenho comercial.")

    System_Ext(crm, "Sistema CRM", "Fornece dados de clientes e oportunidades de negócio (ex: Salesforce).")
    System_Ext(erp, "Sistema ERP", "Exporta dados de pedidos, produtos e estoque em lotes periódicos.")
    System_Ext(ecommerce, "Plataforma E-commerce", "Envia eventos de pedidos em tempo real via webhook.")
    System_Ext(powerbi, "Power BI", "Consome dados do VendaMais para geração de dashboards executivos.")
    System_Ext(email, "Serviço de E-mail", "Envia notificações e alertas para vendedores e gestores.")

    Rel(vendedor, vendamais, "Acessa via navegador web")
    Rel(gestor, vendamais, "Acessa via navegador web")
    Rel(admin, vendamais, "Gerencia via painel administrativo")

    Rel(crm, vendamais, "Envia dados de clientes e oportunidades", "API REST / JSON")
    Rel(erp, vendamais, "Envia exportações de pedidos e produtos", "Arquivo CSV / Batch diário")
    Rel(ecommerce, vendamais, "Envia eventos de pedidos", "Webhook HTTPS")
    Rel(vendamais, powerbi, "Disponibiliza dados para relatórios", "Azure SQL / Conector nativo")
    Rel(vendamais, email, "Envia notificações e alertas", "SMTP / SendGrid")
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
