# ADR-002 — Utilizar Laravel Sanctum

## Status

Substituído pelo [ADR-005](./ADR-005%20%E2%80%94%20Autentica%C3%A7%C3%A3o%20Passwordless%20OTP.md)

## Data

04/08/2026

## Contexto

A aplicação necessita autenticação segura para usuários autenticados via SPA.

## Decisão

Será utilizado Laravel Sanctum.

## Motivos

- Integração oficial.
- Fácil manutenção.
- Excelente documentação.

## Alternativas avaliadas

- JWT
- Passport

## Consequências

Menor complexidade de implementação.
