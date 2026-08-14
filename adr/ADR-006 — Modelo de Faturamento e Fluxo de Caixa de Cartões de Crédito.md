# ADR-006 — Modelo de Faturamento e Fluxo de Caixa de Cartões de Crédito

## Status

Aceito

## Data

05/08/2026

## Contexto

A plataforma financeira necessita definir o comportamento de compras e parcelamentos realizados em cartões de crédito, especificamente no que tange à conciliação entre a data de realização da compra, o ciclo de faturamento (data de fechamento e vencimento) e a saída efetiva de dinheiro do saldo bancário do usuário.

Existem duas visões contábeis principais:
1. **Regime de Competência (Fatura / Categoria):** Quando o gasto compromete determinado período financeiro.
2. **Regime de Caixa (Conta Bancária):** Quando o dinheiro realmente sai da conta corrente do usuário.

## Decisão

Adota-se o modelo dual reconciliado para compras em cartões de crédito:

1. **Alocação na Fatura (Regime de Competência):**
   - Toda compra no cartão (ou cada parcela de uma compra parcelada) é vinculada a uma **Fatura** calculada com base no `dia_fechamento` do cartão.
   - Compras realizadas após a data de fechamento são alocadas automaticamente na fatura do mês subsequente.
   - Nos **relatórios de despesas por categoria** e no **acompanhamento de orçamentos**, o valor é contabilizado no ciclo da fatura correspondente (mês de vencimento).

2. **Impacto no Saldo Bancário (Regime de Caixa):**
   - Realizar uma compra no cartão de crédito **não reduz** o saldo de nenhuma conta bancária no momento do lançamento.
   - O saldo bancário é debitado estritamente na **data de pagamento da fatura** (quando a fatura é liquidada via movimentação de pagamento associada à conta bancária).
   - Enquanto a fatura não é paga, o valor acumula-se como um passivo/compromisso a pagar do cartão.

3. **Comprometimento do Limite do Cartão:**
   - O limite disponível do cartão de crédito é reduzido **imediatamente** no valor total da compra (ou soma de todas as parcelas restantes) no momento do lançamento.
   - O limite é recomposto progressivamente à medida que cada fatura correspondente é liquidada.

4. **Integridade do Histórico de Parcelas:**
   - Em caso de exclusão ou alteração de compras parceladas, apenas parcelas associadas a faturas abertas ou futuras podem ser removidas ou alteradas.
   - Parcelas pertencentes a faturas já fechadas e pagas são preservadas para garantir a integridade do histórico financeiro familiar.

## Motivos

- **Precisão Financeira:** Reflete a realidade prática de que o dinheiro permanece na conta bancária rendendo ou disponível até a data de pagamento da fatura.
- **Clareza de Orçamento:** Permite ao usuário planejar os gastos no mês em que a fatura será efetivamente cobrada.
- **Consistência:** Previne discrepâncias entre o saldo da conta corrente e o extrato bancário real.

## Consequências

- O `CartaoCreditoService` e os endpoints de movimentações devem calcular dinamicamente o status da fatura com base nas datas de fechamento e vencimento.
- Movimentações de pagamento de fatura requerem vinculação adequada entre a conta bancária de origem e a fatura do cartão quitada.
