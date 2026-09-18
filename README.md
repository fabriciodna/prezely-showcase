# Prezely

**Gestão de compras, estoque e operação comercial em um único fluxo.**

O Prezely é um projeto pessoal que estou desenvolvendo para resolver um problema que aparece muito em pequenos comércios e distribuidoras: comparar cotações de vários fornecedores sem depender de planilhas e sem precisar lançar a mesma informação várias vezes.

A parte central do sistema é a comparação de fornecedores. A partir dela, a compra pode seguir para pedido, recebimento, estoque e financeiro.

## O problema

Na prática, cada fornecedor descreve o mesmo produto de um jeito:

```text
Coca Cola Lata 350
Coca-Cola Original 350ml
COCA LT 350ML
```

Comparar isso manualmente fica lento conforme aumentam a quantidade de produtos e fornecedores.

O Prezely tenta reconhecer quando essas descrições representam o mesmo SKU, organiza as ofertas e mostra a melhor condição de compra sem misturar produtos diferentes.

## Principais áreas

### Cotações

Importação de propostas, comparação entre fornecedores e definição do melhor preço por produto.

### Produtos

Cadastro de produtos, histórico de custo, preço de venda e acompanhamento de margem.

### Estoque

Controle por movimentações, com separação entre:

- saldo físico;
- quantidade reservada;
- saldo disponível;
- estoque mínimo;
- histórico de entradas, saídas e balanços.

### Compras

A cotação pode virar pedidos separados por fornecedor. O recebimento alimenta estoque, histórico de custo e contas a pagar.

### Comercial

Clientes, contatos, oportunidades, tarefas, orçamentos e pedidos de venda.

### Financeiro

Contas a pagar e receber, recorrências, contas financeiras, importação de extratos CSV/OFX e conciliação.

## Fluxos

### Compra

```text
Cotação
   ↓
Comparação
   ↓
Melhor fornecedor por produto
   ↓
Pedido de compra
   ↓
Recebimento
   ├── Estoque
   └── Contas a pagar
```

### Venda

```text
Cliente
   ↓
Oportunidade
   ↓
Orçamento
   ↓
Pedido
   ↓
Reserva de estoque
   ↓
Faturamento
   └── Contas a receber
```

## Matching de produtos

Essa é uma das partes mais específicas do projeto.

A identificação considera evidências como marca, linha, variante, sabor, embalagem, volume, retornabilidade, GTIN e aliases já conhecidos por fornecedor.

Busca semântica ajuda a encontrar candidatos, mas não é usada como autoridade final quando existem conflitos objetivos de identidade.

## Stack

- TypeScript
- React
- PostgreSQL
- Kysely
- TanStack Query
- Zod
- SuperJSON
- Papa Parse / XLSX
- embeddings para recuperação semântica
- Floot como ambiente de desenvolvimento e infraestrutura

## Algumas decisões do projeto

- isolamento dos dados por empresa;
- estoque derivado de movimentações em vez de um saldo sobrescrito;
- reserva de estoque separada do saldo físico;
- integrações externas desacopladas das regras de negócio;
- idempotência em fluxos que podem ser repetidos;
- confirmação humana preservada quando a identidade de um produto realmente é ambígua.

Mais detalhes estão em [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).

## Status

O projeto está em desenvolvimento ativo.

Neste momento, o foco está em simplificar a experiência de uso e consolidar estoque, compras, vendas e financeiro. A estrutura para fiscal e cobranças já existe, mas permanece em modo simulado enquanto não há um provedor externo contratado.

## Código-fonte

Este repositório é apenas a apresentação pública do projeto.

O código-fonte completo permanece privado.

## Autor

**Fabrício Dantas**  
GitHub: [@fabriciodna](https://github.com/fabriciodna)
