# Arquitetura

## Visão Geral

O Alfred Finanças foi desenvolvido utilizando arquitetura desacoplada entre Backend e Frontend.

```
React
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

- React
- TypeScript
- Vite
- React Router
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
