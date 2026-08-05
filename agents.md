# AGENTS.md — Guia para IAs no Projeto Alfred Finanças

Este arquivo define as diretrizes obrigatórias para qualquer IA (Copilot, Claude, Gemini, ChatGPT ou outro agente) que atue neste projeto.

Antes de qualquer geração de código, leia e respeite integralmente este documento e todos os documentos de referência listados abaixo.

---

## 📚 Documentos de Referência Obrigatória

Sempre consulte estes documentos antes de escrever qualquer código ou tomar qualquer decisão de design:

| Documento | Conteúdo |
|---|---|
| [`01-product-vision.md`](./01-product-vision.md) | Visão do produto, problema, solução e público-alvo |
| [`02-design-system.md`](./02-design-system.md) | PRD completo: módulos, regras, personas, diretrizes de UI e arquitetura |
| [`03-architecture.md`](./03-architecture.md) | Stack e arquitetura técnica da solução |
| [`04-backend.md`](./04-backend.md) | Padrões, objetivos e testes do backend |
| [`05-frontend.md`](./05-frontend.md) | Stack, organização de pastas e bibliotecas do frontend |
| [`06-business-rules.md`](./06-business-rules.md) | Regras de negócio essenciais |
| [`07-roadmap.md`](./07-roadmap.md) | Fases do produto e funcionalidades por versão |
| [`08-coding-standards.md`](./08-coding-standards.md) | Padrões de commits, branches, PHP e Next.js |
| [`09-api-guidelines.md`](./09-api-guidelines.md) | Padrão REST, versionamento, formato de respostas |
| [`10-alfred-personality.md`](./10-alfred-personality.md) | Tom de voz, personalidade e comunicação do Alfred |
| [`adr/ADR-001`](./adr/ADR-001%20-%20Arquitetura%20desacoplada.md) | Decisão: arquitetura desacoplada (Laravel + Next.js) |
| [`adr/ADR-002`](./adr/ADR-002%20-%20Autentica%C3%A7%C3%A3o.md) | Decisão: autenticação com Laravel Sanctum (Substituído pelo ADR-005) |
| [`adr/ADR-003`](./adr/ADR-003%20%E2%80%94%20Utilizar%20MySQL.md) | Decisão: banco de dados MySQL |
| [`adr/ADR-004`](./adr/ADR-004%20%E2%80%94%20Utilizar%20Next.js%20com%20shadcn.md) | Decisão: Next.js 15 + shadcn/ui |
| [`adr/ADR-005`](./adr/ADR-005%20%E2%80%94%20Autentica%C3%A7%C3%A3o%20Passwordless%20OTP.md) | Decisão: autenticação Passwordless OTP com Sanctum |

---

## 🏗️ Arquitetura & Stack

O projeto é dividido em repositórios separados e desacoplados (ADR-001):

- **`alfred-financas-api`** — Backend em **Laravel 12 / PHP 8.4**, expõe API REST com autenticação via **Laravel Sanctum** (ADR-002)
- **`alfred-financas-web`** — Frontend em **Next.js 15 (App Router) + TypeScript + shadcn/ui** (ADR-004)
- **`alfred-financas-docs`** — Documentação oficial do projeto
- **Banco de dados:** MySQL (ADR-003) + Redis (cache e filas)
- **Infraestrutura:** Docker, AWS EC2, Nginx, GitHub Actions

> Nunca misture responsabilidades entre backend e frontend. Toda regra de negócio, cálculo de fatura, projeção e agregação do dashboard reside **exclusivamente no backend (Laravel)**.

---

## 🗣️ Nomenclatura de Código

Conforme definido no PRD (seção 8):

- **Português** para: nomes de tabelas, models, variáveis, colunas, classes de domínio e comentários de negócio
- **Inglês** apenas para: termos técnicos consagrados (`id`, `status`, `token`, `slug`, `email`, `password`, nomes de padrões e frameworks)
- Exemplos corretos: `familias`, `movimentacoes`, `contas_bancarias`, `cartoes_credito`, `parcelas`, `orcamentos`
- Evite: `families`, `transactions`, `bank_accounts` (forçar tradução técnica de termos de domínio)

---

## 🖥️ Backend — Padrões Obrigatórios

### Estrutura de código

Use **sempre** a seguinte separação de responsabilidades:

```
Controllers   → apenas recebem requisição e retornam resposta
Form Requests → validação de entrada (nunca valide no controller)
Services      → regras de negócio, cálculos, orquestração
DTOs          → transporte de dados entre camadas
Enums         → padronização de status e tipos fixos de domínio
Policies      → autorização (Sanctum + Policies)
Resources     → transformação de dados de saída (API Resources)
Jobs          → processamento assíncrono em segundo plano (filas Redis)
Events/Listeners → comunicação desacoplada entre módulos
```

### Regras de qualidade

- Siga **PSR-12** e use **Laravel Pint** para formatação automática
- Aplique **SOLID**, **Clean Code**, **DRY** e **KISS** em toda implementação
- Escreva testes para **toda feature nova**: Feature Tests e Unit Tests com PHPUnit
- Nunca coloque lógica de negócio em controllers ou models
- Models são apenas mapeamento do banco; use Services para toda regra
- **Padronização de Status com Enums**: Todos os status e tipos fixos devem ser padronizados em Enums, evitando o uso de strings soltas (*magic strings*) pelo código
- **Processamento em Segundo Plano**: Processos muito demorados ou pesados que possam sobrecarregar o servidor **devem obrigatoriamente rodar em segundo plano** via Jobs/Queues (Redis)

### API REST

```
Prefixo base:      /api/v1
Autenticação:      Bearer Token (Sanctum)
Formato sucesso:   { "data": { ... } }
Formato erro:      { "message": "...", "errors": { ... } }
Documentação:      OpenAPI / Swagger
```

- Todos os retornos de API **devem ser estritamente padronizados** (`{ "data": { ... } }` para sucesso ou `{ "message": "...", "errors": { ... } }` para erros) através de API Resources
- Sempre versione a API desde o início (`/api/v1/`) — as integrações com IA do futuro dependem de uma API estável
- Retorne HTTP status codes corretos: 200, 201, 422, 401, 403, 404, 500
- Nunca exponha detalhes internos do servidor em respostas de erro de produção

### Entidades principais (tabelas em português)

```
bancos                      → cadastro global de instituições financeiras (código COMPE, nome, logo e cor padrão)
familias                    → grupos familiares
usuarios                    → usuários da plataforma
familia_usuario             → pivô (usuário pode estar em várias famílias)
contas_bancarias            → contas corrente, poupança, reserva/investimento (vinculadas a um banco opcional)
cartoes_credito             → cartões com limite, fechamento, vencimento
movimentacoes               → receitas e despesas (vinculadas a conta ou cartão)
parcelas                    → parcelas de compras parceladas no cartão
contas_fixas                → despesas recorrentes (forma_pagamento: conta | cartão)
categorias / subcategorias  → categorizações customizáveis
orcamentos                  → metas de gasto por categoria/mês/membro ou família
movimentacoes_investimento  → aportes e resgates com motivo obrigatório
metas                       → objetivos de investimento/reserva
itens_checklist             → itens do checklist de manutenção
checklist_conclusoes        → registro de conclusões por frequência
projecoes                   → parâmetros de simulação financeira futura
configuracoes_resumo_email  → frequência do resumo por usuário/família
logs_resumo_email           → histórico de envios de resumo
```

---

## 🎨 Frontend — Padrões Obrigatórios

### Estrutura de pastas (Next.js App Router)

```
app/                        → rotas e layouts (App Router)
components/                 → componentes reutilizáveis de negócio
components/ui/              → componentes shadcn (NUNCA editar diretamente)
features/                   → módulos por domínio (dashboard, movimentacoes, etc.)
hooks/                      → custom hooks React
lib/                        → utilitários e configurações globais
services/                   → camada de comunicação com a API
types/                      → tipos e interfaces TypeScript
utils/                      → funções utilitárias puras
```

### Regras de componentes

- **Server Components por padrão** — use `"use client"` apenas quando necessário (eventos, estado, hooks do browser)
- **Componentes shadcn:** gerados via CLI (`npx shadcn@latest add`), **nunca editados diretamente em `components/ui/`** — crie wrappers em `components/` para customizações
- **TypeScript Strict** habilitado — sem `any` implícito, sem tipos `unknown` sem tratamento
- Use **ESLint + Prettier** — nunca faça commit com erros de lint

### Bibliotecas aprovadas

```
shadcn/ui          → componentes base (Radix UI + Tailwind)
TanStack Query     → fetching, cache e estado de servidor
React Hook Form    → formulários
Zod                → validação de schemas (use com React Hook Form)
Recharts           → gráficos (dashboard financeiro)
Framer Motion      → animações e transições
```

> Não adicione novas dependências de UI sem verificar se shadcn/ui ou as bibliotecas acima já atendem o caso.

### Design e UX (seção 7 do PRD)

- Paleta com contraste claro entre positivo (verde) e negativo (vermelho/laranja), mas **sem visual "planilha"**
- **Animações sutis** em cards e gráficos ao carregar (contadores animados, transições suaves)
- Gráficos interativos com hover e drill-down por categoria
- Suporte a **modo claro e escuro**
- Glassmorphism/cards elevados usados com **moderação** — não comprometer legibilidade de números
- Hierarquia visual: **números grandes primeiro**, gráficos depois, detalhes por último
- O lançamento de movimentação deve ser o fluxo mais **rápido e simples** da plataforma

---

## 🔐 Segurança

- **HTTPS obrigatório** em produção
- **Autenticação forte**: Sanctum + preparação para 2FA futuro
- Criptografia de dados sensíveis em repouso
- Respeite **LGPD**: dados financeiros são sensíveis, nunca logue dados pessoais
- Use **Policies** do Laravel para toda verificação de autorização (não apenas autenticação)
- Nunca confie em input do usuário sem validação no backend (Form Requests)

---

## 📦 Git — Commits e Branches

### Conventional Commits (obrigatório)

```
feat(auth): login com JWT
fix(cartoes): cálculo incorreto de parcelas
docs(api): adiciona documentação do endpoint de movimentações
refactor(dashboard): extrai lógica de projeção para service
test(movimentacoes): adiciona feature test de criação
chore(deps): atualiza laravel para 12.x
```

### Estratégia de branches

```
main       → produção estável
develop    → integração de features
feature/*  → novas funcionalidades
hotfix/*   → correções urgentes em produção
release/*  → preparação de release
```

---

## ✅ Checklist antes de gerar código

Antes de implementar qualquer funcionalidade, responda:

- [ ] Consultei o PRD (`02-design-system.md`) para verificar se este módulo está definido?
- [ ] Estou respeitando a arquitetura desacoplada (ADR-001)?
- [ ] A regra de negócio está isolada no Service (backend) ou no hook/feature (frontend)?
- [ ] Os nomes de tabelas e variáveis estão em português (exceto termos técnicos)?
- [ ] Estou usando os padrões corretos de resposta da API (`{ data: {} }` / `{ message, errors }`)?
- [ ] Os status utilizam Enums padronizados (sem strings soltas no código)?
- [ ] Processos demorados estão configurados para execução em segundo plano (Jobs/Queues)?
- [ ] Estou adicionando testes para a funcionalidade?
- [ ] Estou seguindo Conventional Commits?
- [ ] No frontend, estou usando Server Components por padrão e `"use client"` somente quando necessário?

---

## 🚫 O que NÃO fazer

- ❌ Não coloque lógica de negócio em Controllers ou Models
- ❌ Não edite arquivos dentro de `components/ui/` — são gerenciados pelo shadcn CLI
- ❌ Não use `any` no TypeScript sem justificativa explícita
- ❌ Não crie endpoints fora do padrão `/api/v1/` ou com respostas fora do padrão estabelecido
- ❌ Não defina status como strings soltas (*magic strings*) espalhadas pelo código (use Enums)
- ❌ Não rode processamentos pesados/demorados de forma síncrona na requisição HTTP (use Jobs em segundo plano)
- ❌ Não faça cálculo de faturas, projeções ou agregações no frontend
- ❌ Não implemente funcionalidades de fases futuras sem alinhamento explícito (ex: Open Finance, IA conversacional)
- ❌ Não adicione bibliotecas de UI sem verificar o conjunto aprovado primeiro
- ❌ Não quebre a estratégia de branches (nunca commite direto em `main`)

---

## 💡 Contexto de Negócio Importante

- O produto é para **uso familiar/pessoal** — privacidade e confiança são cruciais
- **Toda informação de uma família é visível por todos os membros** por padrão (não há dado privado entre membros na v1)
- Um usuário pode pertencer a **mais de uma família** — sempre considere o contexto de família ativo
- Cartões de crédito têm regras específicas de **fechamento e parcelamento** — são críticos e exigem testes rigorosos
- **Reservas** não compõem o saldo disponível — são tratadas separadamente
- O dashboard deve ter **"wow effect"** visual — não entregue telas genéricas
- O fluxo de **lançamento de movimentação** deve ser o mais rápido e simples possível (poucos cliques)
- A **API deve ser estável e versionada desde o início** — integrações com IA futuras (ChatGPT Plugin, Claude MCP) dependerão dela

---

## 🤖 Sobre o Alfred (Assistente de IA do produto)

O Alfred é o assistente de IA embutido no produto. Ao gerar textos, mensagens ou lógica relacionada ao assistente, siga rigorosamente o tom definido em [`10-alfred-personality.md`](./10-alfred-personality.md):

- **Tom:** educado, profissional, calmo, objetivo, prestativo
- **Nunca:** usa piadas, infantiliza o usuário, usa emojis excessivamente, julga escolhas financeiras
- **Sempre:** explica motivos, prioriza clareza, sugere ações práticas, baseia recomendações em dados

---

*Última atualização: Agosto/2026 — Alfred Finanças Docs*
