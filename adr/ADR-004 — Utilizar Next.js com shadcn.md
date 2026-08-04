# ADR-004 — Utilizar Next.js com shadcn/ui

## Status

Aceito

## Data

04/08/2026

## Contexto

O frontend deverá possuir dashboards altamente interativos, com rotas bem definidas, suporte a Server Components para melhor performance e uma biblioteca de componentes acessível e customizável que acelere o desenvolvimento.

## Decisão

Next.js 15 (App Router) + TypeScript + shadcn/ui.

## Motivos

- Next.js oferece App Router com Server Components por padrão, reduzindo o bundle enviado ao cliente.
- Roteamento baseado em sistema de arquivos simplifica a estrutura do projeto.
- shadcn/ui fornece componentes acessíveis (Radix UI) e totalmente customizáveis, sem lock-in de biblioteca — os componentes são copiados para o projeto via CLI.
- Ecossistema React consolidado (TanStack Query, React Hook Form, Framer Motion, Recharts).
- Excelente integração com bibliotecas de gráficos interativos.

## Alternativas

- React + Vite (SPA pura) — descartado por falta de SSR nativo e roteamento embutido.
- Angular — descartado pelo ecossistema menor e curva de adoção.
- Blade (Laravel) — descartado por não suportar a separação frontend/backend desejada.
