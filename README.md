# Prezely

Sistema de gestão comercial com foco em **cotação de fornecedores, compras e estoque**.

![Tela inicial do Prezely](assets/inicio.webp)

## Por que comecei esse projeto

Comecei o Prezely por um problema bem prático: receber preços de vários fornecedores e descobrir o que realmente vale a pena comprar.

Quando são poucos itens, uma planilha resolve. Quando entram dezenas de produtos, fornecedores diferentes e descrições como estas, a comparação começa a dar trabalho:

```text
Coca Cola Lata 350
Coca-Cola Original 350ml
COCA LT 350ML
```

A ideia do projeto é organizar essa etapa e fazer a informação continuar pelo sistema, em vez de terminar em uma planilha.

## Cotações

O sistema importa as propostas, tenta identificar quais linhas representam o mesmo produto e compara os preços entre os fornecedores.

Na revisão, cada produto aparece no fornecedor que venceu naquele item. Casos realmente duvidosos continuam separados para conferência, em vez de serem agrupados à força.

![Revisão de uma cotação no Prezely](assets/cotacoes.webp)

A cotação pode seguir para pedidos de compra separados por fornecedor. Quando a mercadoria é recebida, o fluxo atualiza estoque, histórico de custo e contas a pagar.

## Estoque

O estoque é controlado por movimentações. Eu preferi não trabalhar com um campo de saldo que pode ser simplesmente sobrescrito, porque depois fica difícil saber de onde aquele número veio.

O sistema mantém **físico, reservado e disponível** separados e registra entradas, saídas, balanços e reservas feitas por pedidos.

![Controle de estoque do Prezely](assets/estoque.webp)

Também há estoque mínimo, busca, filtros e histórico de movimentações.

## O que já existe

- produtos, custos, preço de venda e margem;
- fornecedores e rodadas de cotação;
- importação e comparação de propostas;
- matching de produtos entre fornecedores;
- pedidos de compra e recebimento;
- estoque físico, reservado e disponível;
- clientes, oportunidades, tarefas e orçamentos;
- pedidos de venda;
- contas a pagar e receber;
- recorrências e centros de custo;
- importação de extrato CSV/OFX e conciliação;
- permissões e trilha de auditoria por empresa.

## Como os módulos se conectam

### Compra

```text
Cotação → comparação → pedido de compra → recebimento → estoque → contas a pagar
```

### Venda

```text
Cliente → oportunidade → orçamento → pedido → reserva de estoque → faturamento → contas a receber
```

## Matching de produtos

Essa é a parte mais específica do Prezely.

A comparação não depende só do nome escrito pelo fornecedor. O processo usa informações como marca, linha, variante, sabor, embalagem, volume, retornabilidade, GTIN e aliases já confirmados.

A busca semântica ajuda a encontrar candidatos próximos. A decisão final continua sujeita às regras de identidade do produto; um candidato semanticamente parecido não deve vencer um conflito claro de volume, variante ou embalagem.

## Stack

**TypeScript · React · PostgreSQL · Kysely · TanStack Query · Zod · SuperJSON · Papa Parse · XLSX**

Também uso embeddings na recuperação de candidatos para o matching de produtos. O projeto é desenvolvido no Floot.

## Algumas decisões técnicas

O sistema é multiempresa e o escopo da empresa é validado no backend. O estoque parte de um histórico de movimentações, operações que podem ser repetidas usam controles de idempotência e as integrações externas ficam atrás de providers próprios.

A visão resumida da arquitetura está em [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).

## Status

O Prezely continua em desenvolvimento.

O foco atual está na experiência de uso e na consolidação dos fluxos de compras, estoque, vendas e financeiro. Fiscal e cobranças estão preparados estruturalmente, mas permanecem em modo simulado enquanto não há integração externa contratada.

## Código-fonte

Este repositório é somente a apresentação pública do projeto. O código-fonte completo permanece privado.

## Autor

**Fabrício Dantas**  
[@fabriciodna](https://github.com/fabriciodna)
