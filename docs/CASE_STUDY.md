# Prezely: da cotação ao pedido de compra

Este é um relato das decisões que estou tomando no desenvolvimento do Prezely. O projeto ainda está em andamento; aqui mostro a lógica por trás de algumas partes, sem abrir o código-fonte ou expor dados de fornecedores.

## De onde veio a ideia

Trabalhar com compras me colocou diante de uma tarefa recorrente: receber listas de preços de vários fornecedores, conferir os produtos e decidir onde comprar cada item.

O menor preço escrito em uma linha nem sempre é o menor preço do **produto certo**. Se uma lista fala em unidade e outra em fardo, ou se duas descrições escondem volumes diferentes, comparar os números diretamente pode levar a uma decisão errada.

Comecei o Prezely para organizar essa comparação. Depois passei a conectar a cotação com pedidos, recebimento, estoque e financeiro — justamente para não precisar registrar a mesma informação em vários lugares.

## O desafio dos nomes de produtos

Uma das dificuldades foi reconhecer o mesmo item em listas que usam abreviações e descrições diferentes.

Um exemplo simplificado:

```text
Fornecedor A: Coca Cola Lata 350
Fornecedor B: COCA LT 350ML
Fornecedor C: Coca-Cola Original 350ml
```

Não basta escolher o texto mais parecido. É preciso considerar marca, variante, volume, embalagem, retornabilidade e outros atributos quando estiverem disponíveis.

A abordagem do projeto combina essas informações com nomes já confirmados para cada fornecedor. A similaridade semântica serve para encontrar possíveis correspondências; ela não deve passar por cima de um conflito claro de volume ou embalagem.

Quando não existe segurança suficiente para agrupar, é melhor deixar o caso para conferência do que misturar produtos diferentes e apresentar uma economia que não existe.

## O estoque precisava contar uma história

Outro cuidado foi não tratar o estoque como um campo numérico que qualquer operação simplesmente substitui.

O sistema registra movimentações como entradas, saídas e ajustes. Isso permite reconstruir de onde veio uma alteração de saldo. Reservas de pedidos são acompanhadas à parte, para distinguir:

- **Físico:** quantidade registrada como presente no estoque.
- **Reservado:** quantidade comprometida com pedidos.
- **Disponível:** quantidade física menos as reservas ativas.

Reservar um produto reduz o disponível, mas não significa que ele já saiu fisicamente.

## Uma cotação não termina na comparação

Depois de encontrar as ofertas escolhidas, o próximo passo é transformar a decisão em pedidos por fornecedor. No recebimento, os fluxos se relacionam com custo, estoque e contas a pagar.

Isso trouxe uma preocupação adicional: repetir uma ação por engano não deveria criar vários pedidos iguais. Por isso, a geração de pedidos a partir de cotações utiliza controles de idempotência.

O projeto também precisa separar dados entre empresas. O escopo da conta é validado nas operações de backend, em vez de depender apenas da interface para mostrar os registros corretos.

## Ferramentas e organização

A interface utiliza **React e TypeScript**. Os dados ficam no **PostgreSQL**, com acesso por **Kysely**. Também utilizo **TanStack Query**, **Zod**, **SuperJSON**, **Papa Parse** e **XLSX** em partes de comunicação, validação e importação. O ambiente do projeto é o **Floot**.

A estrutura busca manter interface, regras de negócio, persistência e integrações externas separadas. Há uma [visão resumida da arquitetura](ARCHITECTURE.md), sem os detalhes proprietários da implementação.

## O que ainda estou ajustando

Meu foco agora é simplificar o uso e consolidar o caminho entre compras, estoque, vendas e financeiro. A parte fiscal e de cobranças ainda trabalha com providers simulados; integrações reais não devem ser confundidas com algo já entregue.

Não há métricas públicas de adoção ou um ambiente de demonstração aberto neste repositório. Prefiro mostrar o problema, a abordagem e o estágio atual do projeto sem apresentar resultados que ainda não foram validados.

[Voltar à apresentação do Prezely](../README.md) · [Meu GitHub](https://github.com/fabriciodna)
