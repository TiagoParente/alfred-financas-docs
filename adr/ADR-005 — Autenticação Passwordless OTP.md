# ADR-005 — Autenticação Passwordless OTP com Sanctum

## Status

Aceito (Substitui o [ADR-002](./ADR-002%20-%20Autentica%C3%A7%C3%A3o.md))

## Data

04/08/2026

## Contexto

A aplicação necessita de autenticação prática e segura para os usuários no aplicativo e web. Em vez de senhas tradicionais que exigem memorização e redefinições frequentes, adotou-se o modelo de autenticação sem senha (*Passwordless*) via código OTP (*One-Time Password*) enviado por e-mail.

## Decisão

1. Será utilizado o **Laravel Sanctum** para emissão e validação dos Bearer Tokens de acesso (substituindo o fluxo tradicional de senha previsto no ADR-002).
2. A autenticação será efetuada via código numérico OTP de 6 dígitos enviado por e-mail:
   - Os códigos possuem validade temporária (configurável via `AUTH_OTP_EXPIRE`).
   - Bloqueio após limite de tentativas incorretas (`AUTH_OTP_MAX_ATTEMPTS`).
   - Suporte a trava opcional de IP (`AUTH_OTP_BIND_IP`), garantindo que o código só possa ser validado pela mesma conexão que o solicitou.
3. O envio do e-mail é feito em segundo plano (assíncrono via Redis Queue Job).

## Motivos

- **Praticidade e UX:** O usuário entra informando apenas seu e-mail e digitando o código recebido, sem precisar criar ou memorizar senhas.
- **Segurança:** Código temporário de uso único, reduzindo vetores de ataque relacionados a vazamentos de senhas reutilizadas.
- **Sanctum:** Integração oficial no ecossistema Laravel, suporte robusto a Bearer Tokens.

## Alternativas avaliadas

- Autenticação por Senha Tradicional (ADR-002)
- JWT customizado
- Passport / OAuth2

## Consequências

- Menor fricção no onboarding e login de usuários.
- Necessidade de infraestrutura de envio de e-mails em segundo plano (Redis Queue + Mailer).
