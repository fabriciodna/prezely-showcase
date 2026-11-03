# Prezely

**Um projeto pessoal para comparar cotações de fornecedores e organizar compras, estoque e vendas.**

Estou desenvolvendo o Prezely a partir de um problema que conheço do trabalho: receber preços de vários fornecedores e decidir de quem comprar cada produto. Quando a lista cresce, comparar tudo manualmente fica demorado — principalmente porque cada fornecedor pode usar um nome diferente para o mesmo item.

O que começou como um comparador de preços foi ganhando outras partes de uma operação comercial.

## Como funciona

O ponto de partida é uma cotação. O sistema organiza os itens enviados pelos fornecedores, tenta encontrar produtos equivalentes e apresenta a comparação de preços. A partir daí, é possível gerar pedidos de compra separados por fornecedor e seguir com o recebimento da mercadoria.

Hoje o projeto também inclui:

- **Produtos e preços:** cadastro, custos, preço de venda e margem.
- **Estoque:** entradas, saídas, reservas, balanços e histórico de movimentações.
- **Compras:** cotações, comparação e pedidos por fornecedor.
- **Vendas e financeiro:** clientes, orçamentos, pedidos, contas a pagar e receber, além de conciliação a partir de extratos.

## O problema mais interessante até agora

Uma cotação pode trazer descrições como:

```text
Coca Cola Lata 350
Coca-Cola Original 350ml
COCA LT 350ML
```

Os nomes são diferentes, mas podem representar o mesmo produto. Ao mesmo tempo, uma embalagem ou um volume diferente muda completamente a comparação.

Por isso, a identificação não depende só da semelhança entre os textos. O Prezely considera informações como marca, variante, embalagem, volume, GTIN e nomes já confirmados para cada fornecedor. A busca semântica ajuda a encontrar candidatos, mas casos duvidosos precisam continuar disponíveis para conferência.

Essa parte me fez perceber como uma regra aparentemente simples — *comparar o menor preço* — exige cuidado com os dados antes de qualquer cálculo.

## Algumas escolhas técnicas

**Estoque por movimentações.** Em vez de apenas sobrescrever um saldo, o sistema registra entradas, saídas e ajustes. Também separa o que existe fisicamente do que está reservado para pedidos.

**Dados separados por empresa.** Como a aplicação é multiempresa, as operações precisam respeitar o contexto de cada conta no backend.

**Cuidados com reprocessamentos.** Alguns fluxos de compra usam controles de idempotência para não gerar pedidos duplicados quando uma operação é repetida.

Escrevi mais sobre esses pontos no [estudo de caso](docs/CASE_STUDY.md). A [visão da arquitetura](docs/ARCHITECTURE.md) fica em um documento separado.

## Tecnologias

- **Interface:** React e TypeScript.
- **Banco de dados:** PostgreSQL com Kysely.
- **Dados e comunicação:** TanStack Query, Zod e SuperJSON.
- **Importação de arquivos:** Papa Parse e XLSX.
- **Busca de produtos:** regras de identidade e embeddings para recuperar candidatos.
- **Ambiente de desenvolvimento:** Floot.

## Em que pé está

O Prezely está em desenvolvimento. Já existem fluxos de cotação, compras, estoque, vendas e financeiro, mas ainda estou trabalhando para deixá-los mais simples e consistentes.

As partes fiscal e de cobranças usam **integrações simuladas** por enquanto. Não estou apresentando este projeto como um produto finalizado.

**Este é um repositório de apresentação.** O código-fonte continua privado, e não há uma demonstração pública disponível neste repositório.

---

Sou **Fabrício Dantas**, estudante de Sistemas de Informação na UVV. Estou construindo projetos como este enquanto busco minha primeira oportunidade de estágio em desenvolvimento de software.

[Meu perfil no GitHub](https://github.com/fabriciodna)
