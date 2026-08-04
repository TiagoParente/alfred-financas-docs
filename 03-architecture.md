# Arquitetura

## Visão Geral

O Alfred Finanças foi desenvolvido utilizando arquitetura desacoplada entre Backend e Frontend.

```
Next.js
↓

API REST

↓

Laravel

↓

MySQL

↓

Redis
```

---

## Backend

- Laravel 12
- PHP 8.4
- MySQL
- Redis
- Docker

---

## Frontend

- Next.js 15 (App Router)
- TypeScript
- shadcn/ui
- TanStack Query

---

## Infraestrutura

- Docker
- AWS EC2
- Nginx
- GitHub Actions

---

## Comunicação

Todo acesso será realizado através de APIs REST autenticadas.
