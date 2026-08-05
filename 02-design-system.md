# Design System — Plataforma Financeira Familiar

**Versão:** 1.0  
**Data:** Agosto/2026

---

# 1. Objetivo

Este documento define o Design System oficial da Plataforma de Finanças Pessoais & Familiares.

Seu objetivo é garantir consistência visual, excelente experiência do usuário e padronização durante todo o desenvolvimento do produto.

Toda nova funcionalidade deve seguir estas diretrizes.

---

# 2. Filosofia do Produto

O sistema deve transmitir três sensações principais:

- Clareza
- Confiança
- Tranquilidade

O usuário está administrando seu patrimônio.

Cada tela deve transmitir segurança e organização.

A interface deve parecer uma ferramenta profissional, mas extremamente simples de usar.

O objetivo é fazer o usuário sentir:

> "Tenho total controle da minha vida financeira."

---

# 3. Personalidade da Marca

O produto possui um assistente financeiro chamado **Alfred**.

Alfred representa um assessor financeiro pessoal.

Ele não é um chatbot.

Ele não é um mascote.

Ele não faz piadas.

Ele não utiliza linguagem infantil.

Sua comunicação é:

- educada
- objetiva
- acolhedora
- inteligente
- profissional

Exemplo:

✅

> Hoje encontrei duas oportunidades para melhorar sua saúde financeira.

❌

> 😄 E aí! Bora economizar um dinheirinho hoje?

---

# 4. Inspirações

A experiência visual deve se inspirar em produtos como:

- Apple
- Linear
- Stripe
- Vercel
- Arc Browser
- Notion

Características desejadas:

- minimalismo
- muito espaço em branco
- excelente tipografia
- animações discretas
- interface limpa
- foco no conteúdo

Evitar qualquer aparência semelhante a planilhas tradicionais.

---

# 5. Identidade Visual

## Cor Primária

Azul Petróleo

```css
#1F4E79
```

Representa:

- confiança
- estabilidade
- segurança

---

## Cor Secundária

Verde

```css
#22C55E
```

Usar apenas para:

- saldo positivo
- lucro
- sucesso
- metas concluídas

Nunca utilizar verde como cor principal da interface.

---

## Cor Negativa

```css
#EF4444
```

Usar para:

- despesas
- erros
- alertas críticos

---

## Cor de Atenção

```css
#F59E0B
```

Usar para:

- checklist pendente
- avisos
- metas próximas do limite

---

## Fundo (Light)

```css
Background: #F8FAFC
Cards: #FFFFFF
```

---

## Fundo (Dark)

```css
Background: #09090B
Cards: #18181B
```

---

# 6. Tipografia

Fonte principal:

- Inter

Alternativa:

- Geist

Nunca utilizar mais de duas famílias tipográficas.

## Escala

| Elemento | Tamanho |
|----------|---------|
| H1 | 40px |
| H2 | 32px |
| H3 | 24px |
| H4 | 20px |
| Texto | 16px |
| Texto pequeno | 14px |
| Legenda | 12px |

Peso recomendado:

- 400
- 500
- 600
- 700

---

# 7. Espaçamento

Todo o sistema utiliza escala baseada em múltiplos de 8.

```
4
8
16
24
32
40
48
64
80
96
```

Nunca utilizar medidas aleatórias.

---

# 8. Bordas

Botões

```
10px
```

Inputs

```
10px
```

Cards

```
16px
```

Modais

```
20px
```

---

# 9. Sombras

Utilizar sombras extremamente discretas.

Exemplo:

```css
box-shadow: 0 4px 16px rgba(0,0,0,.06);
```

Evitar sombras fortes.

---

# 10. Componentes

## Botões

Existem apenas quatro estilos.

### Primary

Fundo azul.

Texto branco.

---

### Secondary

Outline.

---

### Ghost

Sem fundo.

---

### Danger

Vermelho.

Nunca criar novos estilos de botão.

---

## Inputs

Todo input deve possuir:

- Label
- Placeholder
- Mensagem de ajuda (quando necessário)
- Mensagem de erro

Jamais utilizar placeholder como Label.

---

## Cards

Toda informação financeira deve estar dentro de cards.

Estrutura:

- título
- valor
- descrição opcional
- ação opcional

---

## Modais

Utilizar apenas quando realmente necessário.

Preferir páginas dedicadas para fluxos complexos.

---

## Tabelas

Devem possuir:

- busca
- ordenação
- filtros
- paginação

Linhas com hover.

Sem bordas pesadas.

---

## Gráficos

Biblioteca recomendada:

- Recharts

Tipos preferenciais:

- Linha
- Barra
- Área
- Donut

Evitar gráficos de pizza tradicionais.

Todos os gráficos devem possuir tooltip.

---

# 11. Dashboard

Ordem obrigatória das informações:

1. Saldo Consolidado
2. Receitas
3. Despesas
4. Investimentos
5. Projeção
6. Próximos vencimentos
7. Insights do Alfred
8. Checklist

Nunca alterar essa hierarquia.

---

# 12. Alfred

O Alfred é um assessor financeiro.

Nunca um chatbot.

Sua área sempre deve conter:

- Avatar
- Saudação
- Resumo do dia
- Recomendações
- Botão de ação

Exemplo:

> Bom dia.
>
> Analisei sua situação financeira e encontrei três pontos importantes.
>
> • Ainda existem despesas sem lançamento.
>
> • Sua fatura vence em quatro dias.
>
> • O orçamento de Delivery atingiu 82%.

Botão:

**Ver detalhes**

---

# 13. Microinterações

Toda ação importante deve possuir feedback.

Salvar

- Fade
- Toast

Excluir

- Modal de confirmação

Carregamento

- Skeleton

Nunca utilizar loaders longos quando Skeleton puder ser usado.

---

# 14. Animações

Biblioteca:

Framer Motion

Tempo:

150ms até 250ms

Curva:

Ease Out

Animações devem ser discretas.

Nunca utilizar efeitos chamativos.

---

# 15. Responsividade

Breakpoints

```
640
768
1024
1280
1536
```

Prioridade:

1 Desktop

2 Notebook

3 Tablet

4 Mobile

No mobile simplificar o layout.

Nunca esconder informações importantes.

---

# 16. Acessibilidade

Obrigatório:

- Contraste AA
- Navegação por teclado
- Labels em todos os campos
- Estados de foco visíveis
- Texto alternativo para imagens

Nunca utilizar apenas cor para transmitir significado.

---

# 17. Ícones

Biblioteca oficial:

Lucide Icons

Nunca misturar bibliotecas.

---

# 18. UX Writing

Toda comunicação deve ser:

- simples
- objetiva
- humana

Evitar termos técnicos.

Preferir:

"Saldo disponível"

ao invés de

"Disponibilidade Financeira Consolidada"

Mensagens devem orientar o usuário.

Jamais culpá-lo.

---

# 19. Estados da Interface

Toda tela deve prever:

## Loading

Utilizar Skeleton.

---

## Empty State

Explicar o motivo.

Exemplo:

> Você ainda não possui movimentações cadastradas.

Botão:

Cadastrar movimentação

---

## Error State

Explicar claramente o problema.

Oferecer ação.

Exemplo:

> Não foi possível carregar seus dados.

Botão:

Tentar novamente

---

## Success

Mostrar confirmação discreta.

Exemplo:

> Movimentação salva com sucesso.

---

# 20. Performance

Objetivos:

Dashboard:

< 2 segundos

Mudança de página:

< 500ms

Filtros:

instantâneos sempre que possível.

---

# 21. Arquitetura de Componentes

Todos os componentes devem ser:

- reutilizáveis
- pequenos
- desacoplados
- testáveis

Evitar componentes gigantes.

Preferir composição.

---

# 22. Convenções

## Nome de Componentes

```
DashboardCard
ResumoFinanceiro
GraficoPatrimonio
CardMovimentacao
```

PascalCase.

---

## Hooks

```
useDashboard()

useMovimentacoes()

useCategorias()
```

---

## Pastas

```
components/

features/

hooks/

services/

types/

utils/
```

---

# 23. Boas Práticas

Sempre:

- Destacar a informação mais importante.
- Reduzir quantidade de cliques.
- Priorizar leitura rápida.
- Utilizar linguagem simples.
- Mostrar progresso das ações.
- Evitar poluição visual.
- Utilizar bastante espaço em branco.

Nunca:

- Criar telas sobrecarregadas.
- Utilizar cores excessivas.
- Esconder ações importantes.
- Exigir confirmação para ações simples.
- Utilizar modais desnecessários.

---

# 24. Princípios do Alfred

Toda funcionalidade relacionada ao Alfred deve seguir estas regras:

- Ser proativo.
- Nunca ser invasivo.
- Explicar o motivo das recomendações.
- Priorizar recomendações de maior impacto financeiro.
- Sugerir ações práticas.
- Nunca fazer julgamentos.
- Nunca assustar o usuário.
- Utilizar linguagem respeitosa.

---

# 25. Checklist de Qualidade

Antes de finalizar qualquer tela, validar:

- [ ] O usuário entende a tela em menos de 5 segundos.
- [ ] Existe apenas uma ação principal.
- [ ] O dado mais importante está em destaque.
- [ ] Todos os estados foram implementados.
- [ ] Há feedback visual para todas as ações.
- [ ] O layout funciona em Desktop e Mobile.
- [ ] A acessibilidade foi considerada.
- [ ] O Alfred está seguindo sua personalidade.
- [ ] Os componentes seguem este Design System.

---

# 26. Princípio Final

Sempre que houver dúvida entre adicionar ou remover um elemento da interface, escolha a solução mais simples.

A simplicidade é uma funcionalidade.

O usuário deve gastar seu tempo administrando sua vida financeira, nunca tentando entender como o sistema funciona.
