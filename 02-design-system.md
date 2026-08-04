# PRD — Plataforma de Finanças Pessoais & Familiares

**Versão:** 1.0
**Data:** Agosto/2026
**Stack definida:** Next.js (frontend) + Laravel (backend/API) + MySQL (banco de dados)
**Plataforma:** Web (responsivo). Mobile fora de escopo por enquanto.

---

## 1. Visão Geral

Uma plataforma web de gestão financeira pessoal e familiar, com lançamento manual de movimentações (fase inicial), dashboard visual e analítico, controle de contas fixas, cartões de crédito parcelados, investimentos/reservas, projeção financeira futura, checklist de manutenção e, em fases futuras, interação via IA (plugin no ChatGPT e integração com Claude Code) para lançar movimentações e conversar sobre as finanças em linguagem natural.

### 1.1 Objetivo do Produto
Dar a uma família visão unificada e clara da situação financeira atual e futura, reduzindo o esforço de organizar dados espalhados em vários bancos/cartões e tornando o hábito de manter tudo atualizado mais simples (via checklist) e, no futuro, mais rápido (via IA).

### 1.2 Problema
- Dinheiro espalhado em várias contas/bancos (inclusive contas usadas como "cofre"/reserva) sem visão consolidada.
- Falta de clareza sobre quanto está comprometido em contas fixas e parcelas de cartão nos próximos meses.
- Dificuldade de projetar o futuro financeiro (ex: "em 6 meses, quanto sobra?").
- Falta de constância em manter os dados atualizados.
- Casais/famílias sem uma visão financeira unificada, cada um controlando do seu jeito.

### 1.3 Público-alvo
- Uso inicial: o próprio idealizador e sua esposa (núcleo familiar).
- Perfil: pessoas organizadas que quer controle financeiro detalhado, confortáveis preenchendo dados manualmente em troca de mais controle e privacidade (sem Open Finance no início).

---

## 2. Personas

| Persona | Descrição | Necessidade principal |
|---|---|---|
| **Administrador da família** | Cria a família, convida membros, cadastra contas de todos | Visão geral consolidada + controle fino |
| **Membro da família** | Cônjuge/dependente com acesso à plataforma | Lançar/consultar suas próprias movimentações e ver o todo |

*(Papéis podem evoluir em fases futuras — ver seção 9.)*

---

## 3. Modelo Multiusuário ("Família")

- Uma **Família** agrupa vários **Usuários**.
- **Um usuário pode pertencer a mais de uma família** (ex: sua família com a esposa, e futuramente outro núcleo, como um pequeno negócio) — ao entrar na plataforma, o usuário escolhe/alterna qual família está visualizando.
- Cada usuário pode ter suas próprias contas bancárias, cartões e investimentos cadastrados, vinculados à família em que foram criados.
- **Toda informação cadastrada por um membro é visível para todos os membros daquela família por padrão** (contas unificadas).
- O Dashboard tem uma visão **"Geral" (família)** e permite alternar para visão **individual por membro** (filtro).
- Não há, na v1, conceito de dado privado/oculto entre membros — pode ser avaliado como melhoria futura (ver seção 9).

### 3.1 Convite e Confirmação de Conta (via E-mail)
- O administrador convida um novo membro informando o e-mail; o convidado recebe um link de convite por e-mail para criar sua senha e entrar na família.
- Toda criação de conta (inclusive o próprio administrador) passa por **confirmação de e-mail** via código/link enviado automaticamente.
- Reenvio de convite/código disponível caso expire ou não chegue.

---

## 4. Escopo do Produto (Módulos)

### 4.1 Dashboard Principal
Visão central da situação financeira, com "wow effect" visual (ver seção 7 — Diretrizes de Design).

**Deve conter:**
- Saldo total consolidado (todas as contas) + saldo por conta.
- Alternância entre visão **Família (geral)** e **Individual** (por membro).
- Filtro de período (mês atual, últimos 3/6/12 meses, personalizado).
- Cards de resumo: Receitas do mês, Despesas do mês, Saldo do mês, Total investido/reservado.
- Gráfico de evolução patrimonial (linha, ao longo do tempo).
- Gráfico de receitas vs. despesas por mês (barras).
- Gráfico de distribuição de gastos por categoria (pizza/donut).
- Gráfico de despesas fixas vs. variáveis vs. parceladas.
- Indicador de "saúde financeira" (ex: % da renda comprometida com despesas fixas + parcelas).
- Próximos vencimentos (contas fixas e faturas de cartão a vencer).
- Alertas/insights automáticos (ex: "seus gastos com X aumentaram 20% este mês").

### 4.2 Contas Bancárias
- Cadastro de contas (banco, tipo: corrente/poupança/carteira digital, titular, saldo inicial).
- Atualização de saldo **manual** (v1). Estrutura de dados já pensada para permitir integração automática futura (Open Finance/Belvo/Pluggy), mas **não é escopo agora**.
- Histórico de saldo por conta ao longo do tempo.
- Vínculo da conta a um membro da família (titular).

### 4.3 Movimentações (Transações)
- Lançamento manual de receitas e despesas.
- Campos: data, valor, categoria, subcategoria, conta/cartão de origem, descrição, tags, recorrente? (sim/não), membro responsável.
- Categorias e subcategorias customizáveis (com categorias padrão pré-cadastradas).
- Edição, exclusão e duplicação de lançamentos.
- Anexo de comprovante (upload de imagem/PDF) — opcional.
- Busca e filtros avançados (por período, categoria, conta, membro, valor).

### 4.4 Contas Fixas (Despesas Recorrentes)
- Cadastro de despesas fixas (aluguel, internet, streaming, escola, seguro do cartão, etc.) com valor, dia de vencimento, categoria, e **forma de pagamento**: `conta bancária` (débito automático) ou `cartão de crédito`.
  - Isso resolve o caso de despesas fixas que são cobradas dentro da fatura do cartão (ex: seguro do cartão, anuidade): a conta fixa continua existindo como um registro único (aparece nos relatórios de gastos fixos e nas projeções), mas o lançamento mensal é gerado **automaticamente dentro da fatura do cartão correspondente**, sem duplicidade.
- Geração automática do lançamento previsto todo mês (status: previsto → pago).
- Marcar como pago (baixa manual, quando pago via conta) e possibilidade de editar valor daquele mês específico (ex: conta de luz varia). Quando pago via cartão, a baixa acontece junto com o pagamento da fatura.
- Visão de "contas fixas do mês" com status (pago/pendente/atrasado).

### 4.5 Cartões de Crédito e Parcelamentos
- Cadastro de cartões (nome, bandeira, limite, dia de fechamento, dia de vencimento, titular).
- Lançamento de compras no cartão, com opção de parcelamento (nº de parcelas).
- Sistema calcula automaticamente em qual fatura cada parcela cai, considerando data de fechamento.
- **Edição de parcelas:** é possível editar o valor de uma parcela específica sem alterar as demais (útil quando o valor varia, ex: compra em moeda estrangeira).
- **Exclusão de compra parcelada:** ao excluir, o usuário escolhe entre excluir apenas uma parcela futura específica ou a compra inteira — nesse caso, apenas as parcelas futuras/não pagas são removidas; parcelas já incluídas em faturas fechadas/pagas permanecem no histórico (não podem ser apagadas retroativamente, para preservar a integridade do extrato).
- Despesas fixas pagas via cartão (ver 4.4) aparecem automaticamente na fatura do mês, lado a lado com as compras avulsas e parceladas.
- Visão de fatura atual e faturas futuras (projeção de parcelas e recorrências ainda a vencer).
- Controle de limite disponível.
- Fechamento e pagamento de fatura (baixa).

### 4.6 Investimentos / Reservas
Área dedicada para contas usadas como reserva/poupança/investimento (ex: conta em outro banco só para guardar dinheiro).
- Cadastro dessas contas como "conta de investimento/reserva" (pode ter subtipo: reserva de emergência, objetivo específico, investimento de fato).
- Lançamento de aportes (entradas) e resgates (saídas), cada um com **motivo/categoria** obrigatório (ex: "aporte mensal", "resgate para viagem", "rendimento").
- Histórico completo de movimentações por conta de investimento.
- Gráfico de evolução do valor investido/reservado ao longo do tempo.
- Possibilidade de vincular a conta a uma **meta/objetivo** (ex: "Reserva de emergência: R$ 30.000", com barra de progresso).

### 4.7 Projeção Financeira Futura
- Projeção de saldo futuro (ex: próximos 3/6/12 meses) considerando:
  - Contas fixas cadastradas.
  - Parcelas de cartão já assumidas (compromissos futuros conhecidos).
  - Receitas recorrentes cadastradas (salário, etc.).
  - Média histórica de gastos variáveis (opcional, como estimativa).
- Gráfico de linha "saldo projetado" mês a mês.
- Simulador simples: "e se eu adicionar uma despesa/receita nova, como fica a projeção?"
- Alerta de meses com projeção negativa.

### 4.8 Checklist de Manutenção
Checklist para o usuário manter a plataforma sempre atualizada.
- Itens configuráveis com frequência: **diária, semanal, mensal**.
- Exemplos padrão sugeridos: "Lançar gastos do dia", "Atualizar saldo das contas (semanal)", "Conferir fatura do cartão (mensal)", "Revisar contas fixas pagas (mensal)".
- Tela de checklist com itens pendentes/concluídos, resetando conforme a frequência.
- Indicador no Dashboard mostrando pendências do checklist.

### 4.9 Metas de Gastos por Categoria (Orçamento)
- Definição de um valor máximo mensal por categoria (ex: "Delivery: até R$ 800/mês"), por membro ou para a família toda.
- Barra de progresso no dashboard mostrando quanto já foi gasto vs. meta, por categoria.
- Alerta visual quando a meta está próxima de estourar (ex: 80%) ou já foi ultrapassada.
- Histórico de metas atingidas/estouradas por mês, para acompanhar evolução.

### 4.10 Resumo Automático por E-mail
- Envio automático de um resumo financeiro **da família** (visão geral, não individual) por e-mail.
- Frequência padrão: **semanal** (resumo da semana anterior) — usuário pode **desativar** ou trocar a frequência (ex: diário) nas configurações.
- Caso o usuário participe de mais de uma família, recebe **um e-mail separado por família** (ex: 2 famílias = 2 e-mails distintos, cada um com o resumo daquela família).
- Conteúdo do resumo: total de receitas e despesas do período, saldo do período, maiores gastos, categorias que mais consumiram, contas fixas/parcelas que vencem nos próximos dias, e status do checklist.
- Estrutura pensada para reutilizar os mesmos dados/serviços do Dashboard (evita duplicar lógica de cálculo).
- Base para futuras notificações por outros canais (ex: WhatsApp — ver seção 9).

### 4.11 Integração com IA (Fases futuras — fora do MVP)
- **Fase futura A:** Plugin/Custom GPT no ChatGPT permitindo lançar movimentações e consultar dados via chat, usando a API do backend (Laravel) como ponte.
- **Fase futura B:** Integração equivalente via Claude Code / MCP, permitindo lançar movimentações e "conversar" com a IA sobre as finanças.
- Ambas dependem de uma **API pública autenticada** bem definida (endpoints de criar transação, consultar saldo, consultar resumo, etc.) — recomenda-se já desenhar essa API desde o início pensando nesse uso futuro, mesmo que a integração em si venha depois.

---

## 5. Requisitos Funcionais — Resumo por Prioridade

### MVP (Fase 1)
1. Autenticação e criação de Família + convite de membros.
2. Cadastro de contas bancárias e cartões.
3. Lançamento manual de movimentações (receita/despesa).
4. Contas fixas com geração mensal e baixa.
5. Cartões de crédito com parcelamento e faturas.
6. Dashboard com os gráficos essenciais (evolução, receitas x despesas, categorias).
7. Checklist básico (diário/semanal/mensal).
8. Metas de gastos por categoria (orçamento).
9. Resumo automático por e-mail (diário/semanal).

### Fase 2
10. Módulo de Investimentos/Reservas completo (com metas).
11. Projeção financeira futura + simulador.
12. Insights automáticos no dashboard.
13. Anexo de comprovantes.

### Fase 3
14. API pública para integrações de IA.
15. Plugin/Custom GPT (ChatGPT).
16. Integração via Claude Code / MCP.

---

## 6. Requisitos Não Funcionais

- **Segurança:** dados financeiros são sensíveis — autenticação forte (senha + preferencialmente 2FA), criptografia de dados sensíveis em repouso, HTTPS obrigatório, tokens JWT/Sanctum para API.
- **Privacidade:** já que não há Open Finance na v1, não há dados bancários de terceiros trafegando — reduz risco, mas ainda assim exige boas práticas de proteção de dados pessoais (LGPD).
- **Performance:** dashboard deve carregar rápido mesmo com anos de histórico (uso de agregações no backend, não cálculo tudo no frontend).
- **Usabilidade:** lançamento de movimentação deve ser rápido (poucos cliques) — é o fluxo mais usado no dia a dia.
- **Responsividade:** funcionar bem em desktop e também em telas menores (notebook/tablet), já que não haverá app mobile nativo.
- **Auditoria:** manter histórico de alterações (quem editou o quê e quando), especialmente relevante no contexto multiusuário.

---

## 7. Diretrizes de Design / "Wow Effect" do Dashboard

- Paleta de cores com contraste claro entre positivo (verde) e negativo (vermelho/laranja), mas fugindo do visual "planilha".
- Uso de **animações sutis** em cards e gráficos ao carregar/atualizar dados (contadores animados, transições suaves entre gráficos).
- Gráficos interativos (hover mostrando detalhes, zoom em período, drill-down por categoria).
- Modo claro e escuro.
- Componentes com efeito de profundidade (glassmorphism/cards elevados) usados com moderação — não exagerar para não comprometer legibilidade dos números.
- Hierarquia visual clara: números grandes e destacados primeiro (o que importa), gráficos de apoio depois, detalhes por último.
- Sugestão de bibliotecas (Next.js): Recharts, Tremor, ou Chart.js para os gráficos; Framer Motion para animações.

---

## 8. Arquitetura Técnica (alto nível)

- **Frontend:** Next.js (React), consumindo API REST do Laravel.
- **Backend:** Laravel, expondo API REST autenticada (Sanctum ou JWT), responsável por toda a regra de negócio (cálculo de faturas, projeções, agregações do dashboard).
- **Banco de dados:** MySQL.
- **Convenção de nomenclatura:** tabelas, models, variáveis e código em geral devem ser escritos em **português**, preservando em **inglês apenas os termos técnicos consagrados** (ex: `id`, `status`, `token`, `slug`, `email`, `password`, nomes de padrões/frameworks). O objetivo é manter o código legível em português sem forçar tradução de termos que soam estranhos traduzidos.
- **Principais entidades (alto nível, nomes de tabela em português):**
  - `familias`, `usuarios`, `familia_usuario` (tabela pivô — permite usuário em mais de uma família)
  - `contas_bancarias` (tipo: corrente, poupança, investimento/reserva)
  - `cartoes_credito`
  - `movimentacoes` (receita/despesa, vinculada a conta ou cartão)
  - `parcelas` (parcelas de compras no cartão, com valor editável por parcela e suporte a exclusão parcial)
  - `contas_fixas` (com `forma_pagamento` = conta bancária ou cartão de crédito, e geração mensal de `movimentacoes`/entrada na fatura)
  - `categorias` / `subcategorias`
  - `orcamentos` (metas de gasto por categoria/mês/membro ou família)
  - `movimentacoes_investimento` (aportes/resgates, com motivo)
  - `metas` (metas de investimento)
  - `itens_checklist` e `checklist_conclusoes`
  - `projecoes` (parâmetros de simulação, se persistidos)
  - `configuracoes_resumo_email` (frequência escolhida por usuário/família) e `logs_resumo_email` (histórico de envios)

*(Modelagem detalhada de tabelas fica para a fase de design técnico, fora deste PRD.)*

---

## 9. Ideias para Considerar (fora do MVP, mas vale registrar)

- Nível de privacidade por conta/lançamento (marcar algo como "pessoal", visível só ao dono) — hoje descartado, mas fácil de adicionar depois se a necessidade surgir.
- Exportação de relatórios (PDF/Excel) do dashboard e extratos.
- Metas de gastos por categoria (ex: "no máximo R$ 800 em delivery por mês") com alerta.
- Open Finance (conexão automática com bancos) como evolução natural após validar o produto no manual.
- Notificações (e-mail/WhatsApp) de contas a vencer e checklist pendente.
- Multi-moeda (caso haja conta ou investimento em outra moeda).

---

## 10. Métricas de Sucesso

- Frequência de uso: nº de dias por semana em que ao menos um lançamento é feito.
- % de itens do checklist concluídos por semana/mês.
- Tempo médio para lançar uma movimentação (indicador de fricção do fluxo).
- Precisão da projeção financeira (comparar projetado vs. realizado, mês a mês).

---

## 11. Riscos e Pontos de Atenção

- **Risco de abandono por fricção manual:** sem integração bancária automática, o sucesso do produto depende do hábito de lançar dados — o checklist e a rapidez do fluxo de lançamento são críticos.
- **Complexidade do cálculo de faturas de cartão:** regras de fechamento/parcelamento exigem atenção especial nos testes.
- **Escopo de IA é dependente de API estável:** mudanças na API depois que o plugin/MCP existir podem quebrar integrações — vale versionar a API desde o início.

---

## 12. Decisões Registradas (antes em aberto)

- **Resumo por e-mail — frequência padrão:** semanal, com opção de o usuário desativar ou trocar a frequência nas configurações.
- **Escopo do resumo:** sempre da família (visão geral), não individual.
- **Usuário em múltiplas famílias:** recebe um e-mail separado por família (ex: se participa de 2 famílias, recebe 2 e-mails distintos, um resumo de cada).
