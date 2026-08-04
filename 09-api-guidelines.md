# API

## Padrão

REST

---

## Versionamento

`/api/v1`

---

## Respostas

### Sucesso
```json
{
    "data": {}
}
```

### Erro
```json
{
    "message": "Descrição do erro",
    "errors": {}
}
```

---

## Autenticação

Bearer Token (Laravel Sanctum) + **Passwordless OTP via E-mail**

### Rotas de Autenticação:
- `POST /api/v1/auth/solicitar-codigo` → Solicita o código numérico OTP por e-mail.
- `POST /api/v1/auth/verificar-codigo` → Valida o código OTP e retorna o Bearer Token.
- `POST /api/v1/auth/logout` → Revoga o Bearer Token do usuário autenticado.

---

## Documentação

OpenAPI / Swagger
