# Estudo de caso técnico — Prezely

> Projeto pessoal de Fabrício Dantas, em desenvolvimento. Este documento apresenta decisões técnicas em alto nível; a implementação permanece em repositório privado.

## 1. Problema

Pequenos comércios e distribuidoras recebem cotações de vários fornecedores, muitas vezes com descrições diferentes para produtos equivalentes. Comparar manualmente preço, embalagem e variação de produto dificulta a decisão de compra e cria retrabalho ao atualizar estoque e financeiro.

## 2. Solução

O Prezely conecta o processo de cotação ao restante da operação:

```text
Cotações recebidas
       ↓
Normalização e busca de produtos equivalentes
       ↓
Comparação de ofertas por produto
       ↓
Pedidos de compra por fornecedor
       ↓
Recebimento, estoque e contas a pagar
```

Há também funcionalidades de vendas, clientes e contas a receber. A parte fiscal e de cobranças utiliza **providers simulados** no estágio atual, não integrações financeiras ou fiscais definitivas.

## 3. Tecnologias utilizadas

- **TypeScript e React** para a interface e os fluxos de utilização.
- **PostgreSQL e Kysely** para persistência e acesso a dados.
- **TanStack Query, Zod e SuperJSON** para comunicação, validação e tratamento de dados.
- **Papa Parse e XLSX** para trabalhar com formatos de importação.
- **Embeddings** como apoio à recuperação de candidatos na comparação de produtos.
- **Floot** como ambiente de desenvolvimento e infraestrutura.

## 4. Decisões de engenharia

### Identidade de produtos antes de similaridade

Produtos podem ter descrições distintas, mas precisam ser comparados conforme atributos relevantes, como marca, variante, sabor, embalagem, volume e GTIN. A busca semântica ajuda a encontrar candidatos; ela não deve ignorar conflitos explícitos desses atributos.

**Motivo:** um texto parecido não garante que dois itens sejam equivalentes para uma compra.

### Estoque baseado em movimentações

O saldo deriva de entradas, saídas, ajustes e reservas, em vez de depender apenas de um número editável. O sistema diferencia saldo físico, reservado e disponível.

**Motivo:** dar rastreabilidade às alterações e reduzir inconsistências entre pedidos e estoque.

### Isolamento por empresa

As operações de leitura e escrita verificam o escopo da empresa no backend.

**Motivo:** evitar cruzamento de dados entre contas de um sistema multiempresa.

### Idempotência em operações de compra

A geração de pedidos a partir de cotações aplica controles para evitar pedidos duplicados em reprocessamentos.

**Motivo:** impedir que repetição de uma ação produza registros operacionais duplicados.

### Integrações desacopladas

As funções fiscais e de cobrança são separadas por interfaces de integração. Neste estágio, os providers correspondentes são simulados.

**Motivo:** permitir evolução futura sem acoplar o domínio do sistema a um fornecedor externo específico.

## 5. O que este projeto demonstra

- Modelagem de fluxos comerciais completos e regras de negócio.
- Desenvolvimento de interfaces e comunicação com serviços.
- Tratamento de dados estruturados e importação de arquivos.
- Persistência relacional e separação de responsabilidades.
- Cuidados com segurança e isolamento multiempresa.
- Avaliação de correspondência entre produtos com critérios além da similaridade textual.

## 6. Estado atual e limites

O Prezely está em desenvolvimento ativo. A experiência de uso e a consistência dos fluxos de compras, estoque, vendas e financeiro continuam em evolução.

Este repositório é **apenas uma apresentação pública**: não oferece o código-fonte completo, nem um ambiente de demonstração com acesso autorizado. Não são apresentados indicadores de desempenho ou alegações de uso comercial que não tenham sido documentados e validados.

Para conhecer as telas e os recursos existentes, consulte o [README principal](../README.md) e o [resumo da arquitetura](ARCHITECTURE.md).

---

**Autor:** [Fabrício Dantas](https://github.com/fabriciodna)
