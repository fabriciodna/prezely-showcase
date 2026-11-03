# Como o Prezely se organiza

Esta é uma visão técnica resumida. O código-fonte, os detalhes de implementação e as regras proprietárias não estão neste repositório público.

## Componentes

A interface conversa com endpoints e serviços. As regras de negócio ficam separadas da persistência em PostgreSQL. Integrações externas são tratadas por adapters/providers, para que o sistema não dependa diretamente de um único fornecedor de serviços.

## Caminho de uma compra

```text
Cotação recebida
  → ofertas organizadas e produtos identificados
  → comparação de preços
  → pedido por fornecedor
  → recebimento
  → entrada no estoque e conta a pagar
```

A geração de pedidos a partir da cotação usa controles de idempotência para evitar duplicações em reprocessamentos.

## Identificação de produtos

A comparação utiliza atributos como marca, família, variante, sabor, volume, embalagem, retornabilidade, GTIN e nomes associados ao fornecedor.

A busca por similaridade ajuda a localizar candidatos. Conflitos de identidade, como volume e embalagem diferentes, continuam sendo critérios importantes para decidir se dois registros representam o mesmo item.

## Estoque

Entradas, saídas e ajustes são registrados como movimentações. O sistema acompanha separadamente:

- **Saldo físico:** quantidade presente no estoque.
- **Saldo reservado:** quantidade comprometida em pedidos.
- **Saldo disponível:** saldo físico menos reservas ativas.

Uma reserva altera o disponível, **não** representa uma saída física. A saída é registrada no momento correspondente do fluxo operacional.

## Multiempresa

Os dados operacionais são vinculados a uma empresa. Operações de leitura e escrita validam esse contexto no backend, para não misturar informações de contas diferentes.

## Vendas e financeiro

Pedidos de venda podem reservar itens de estoque. O faturamento se conecta às saídas e às contas a receber, enquanto o recebimento de compras se relaciona às entradas e às contas a pagar.

O projeto também possui conciliação de extratos CSV/OFX. As sugestões de correspondência consideram informações como valor, data e contraparte, sem tratar conflitos como confirmações automáticas.

## Integrações externas

As partes fiscal e de cobranças usam **providers simulados** neste estágio do desenvolvimento. Isso permite trabalhar os fluxos internos sem apresentar emissão fiscal ou cobrança real como funcionalidades já operacionais.

[Voltar ao README](../README.md)
