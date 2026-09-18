# Arquitetura

Esta é uma visão resumida da arquitetura do Prezely. Os detalhes de implementação e regras proprietárias não fazem parte do repositório público.

## Organização geral

O sistema separa interface, regras de negócio, persistência e integrações externas.

```text
Interface
   ↓
Endpoints / serviços
   ↓
Regras de negócio
   ↓
PostgreSQL

Integrações externas
   ↕
Adapters / providers
```

## Multiempresa

Os registros operacionais são associados a uma empresa. Leituras e alterações validam esse escopo no backend para evitar cruzamento de dados entre contas.

## Estoque

O estoque é calculado a partir de movimentações.

```text
Entrada       +
Saída         -
Ajuste        +/-
Reserva       -
Liberação     +
```

Com isso, o sistema consegue manter separadamente saldo físico, reservado e disponível.

## Compras

```text
Cotação
→ ofertas normalizadas
→ comparação
→ vencedor por produto
→ pedido por fornecedor
→ recebimento
→ estoque + financeiro
```

A criação de pedidos a partir de uma rodada é tratada de forma idempotente para evitar pedidos duplicados em reprocessamentos.

## Identidade de produtos

O processo de matching trabalha com uma identidade estruturada do produto.

Exemplos de atributos:

- marca;
- família;
- variante;
- sabor;
- recipiente;
- volume;
- retornabilidade;
- GTIN;
- aliases do fornecedor.

A busca vetorial é usada para recuperar candidatos. A decisão final considera regras determinísticas e conflitos explícitos.

## Financeiro

Vendas podem gerar contas a receber e compras podem gerar contas a pagar. Parcelas são registradas individualmente.

A conciliação bancária pode partir de arquivos CSV/OFX. Sugestões automáticas levam em conta tipo da movimentação, valor, proximidade de data e contraparte, mas conflitos não são conciliados automaticamente.

## Integrações

Fiscal e pagamentos foram desenhados atrás de interfaces próprias.

Hoje o desenvolvimento usa providers simulados. A intenção é permitir a troca por um serviço real futuramente sem acoplar vendas, estoque e financeiro a um fornecedor específico.
