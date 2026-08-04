# ADR-001 — Utilizar Backend e Frontend desacoplados

## Status

Aceito

## Data

04/08/2026

## Contexto

O Alfred é uma plataforma que futuramente terá:

- Aplicação Web
- Aplicativo Mobile
- Integrações com IA
- APIs públicas

Precisamos de uma arquitetura que facilite a evolução do produto.

## Decisão

O backend será desenvolvido em Laravel expondo APIs REST.

O frontend será desenvolvido em React consumindo essas APIs.

## Consequências

### Positivas

- Backend reutilizável.
- Facilita criação do aplicativo mobile.
- APIs públicas.
- Melhor separação de responsabilidades.

### Negativas

- Maior complexidade inicial.
- Necessidade de gerenciar CORS.
- Dois deploys separados.
