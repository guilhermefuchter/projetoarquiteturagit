# VendaMais — Arquitetura de Software

Repositório de artefatos arquiteturais do projeto integrador **VendaMais**, desenvolvido na disciplina **DAS2 2026**.

--- 

## Descrição do Projeto

A VendaMais é uma plataforma de apoio a vendas que centraliza dados de clientes, pedidos e desempenho comercial. O objetivo deste repositório é documentar as decisões arquiteturais da solução, incluindo estratégias de ingestão de dados, armazenamento e decomposição em containers.

---

## Integrantes

| Nome | GitHub |
|---|---|
| Guilherme Fuchter dos Santos | [@guilhermefuchter](https://github.com/guilhermefuchter) |
| Andre Moura | [@andreemouraa](https://github.com/andreemouraa) |
| Vitor Lamin | [@vitorlaminhentschel2](https://github.com/vitorlaminhentschel2) |
| Nathan Campos | [@nathancampos23](https://github.com/nathancampos23) |

---

## Estrutura do Repositório

```
projetoarquiteturagit/
├── README.md                  # Este arquivo
├── docs/
│   ├── c4/
│   │   ├── nivel1-contexto.md # Diagrama C4 Nível 1 (Contexto)
│   │   └── nivel2-containers.md # Diagrama C4 Nível 2 (Containers)
│   └── adr/
│       ├── ADR-001-ingestao.md      # ADR sobre estratégia de ingestão
│       └── ADR-002-armazenamento.md # ADR sobre estratégia de armazenamento
```

---

## Como Navegar

- **Diagramas C4** → pasta `docs/c4/`  
  Os diagramas estão em formato Mermaid, renderizáveis diretamente no GitHub.

- **ADRs** → pasta `docs/adr/`  
  Cada ADR documenta uma decisão arquitetural com contexto, alternativas analisadas, decisão final e consequências.

---

## Processo de Desenvolvimento

Este repositório segue o fluxo de trabalho com branches por funcionalidade e Pull Requests revisados entre os integrantes:

- `main` — versão consolidada e aprovada
- `feat/diagrama-c4-contexto` — C4 Nível 1
- `feat/diagrama-c4-containers` — C4 Nível 2
- `feat/adr-001-ingestao` — ADR-001
- `feat/adr-002-armazenamento` — ADR-002
