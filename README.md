# DSCommerce

Projeto desenvolvido como desafio final do capítulo **Login e Controle de Acesso** do curso **Java Spring Essential** da [DevSuperior](https://devsuperior.com.br).

A aplicação é uma API REST de e-commerce com autenticação OAuth2 + JWT, controle de acesso por perfis de usuário e operações completas sobre produtos, pedidos e categorias.

---

## Tecnologias

- Java 21
- Spring Boot 3.4.5
- Spring Security + OAuth2 Authorization Server
- Spring Data JPA
- Banco de dados H2 (perfil de teste)
- Bean Validation
- Maven

---

## Modelo de domínio

```
User ──< Role
  │
  └──< Order ──< OrderItem >── Product >── Category
         │
         └── Payment
```

- Um usuário pode ter múltiplos papéis (CLIENT, ADMIN)
- Um pedido pertence a um usuário e contém vários itens
- Cada item referencia um produto com quantidade e preço registrado no momento da compra
- Um produto pode pertencer a múltiplas categorias

---

## Endpoints

### Públicos (sem autenticação)

| Método | Rota | Descrição |
|--------|------|-----------|
| POST | `/oauth2/token` | Login — retorna o token de acesso JWT |
| GET | `/products` | Lista paginada de produtos (filtro por nome) |
| GET | `/products/{id}` | Busca produto por ID |
| GET | `/categories` | Lista todas as categorias |

### Autenticados (CLIENT ou ADMIN)

| Método | Rota | Descrição |
|--------|------|-----------|
| GET | `/users/me` | Retorna dados do usuário logado |
| GET | `/orders/{id}` | Busca pedido por ID (próprio ou ADMIN) |
| POST | `/orders` | Cria um novo pedido |

### Restritos a ADMIN

| Método | Rota | Descrição |
|--------|------|-----------|
| POST | `/products` | Cadastra produto |
| PUT | `/products/{id}` | Atualiza produto |
| DELETE | `/products/{id}` | Remove produto |

---

## Como executar

### Pré-requisitos

- Java 21+
- Maven 3.8+

### Rodando a aplicação

```bash
# Clone o repositório
git clone https://github.com/seu-usuario/dscommerce-desafio.git

# Entre na pasta do projeto
cd dscommerce-desafio

# Execute com Maven
./mvnw spring-boot:run
```

A API ficará disponível em `http://localhost:8080`.

O console do banco H2 pode ser acessado em `http://localhost:8080/h2-console`.

---

## Autenticação

A API usa o fluxo **Resource Owner Password Credentials** com OAuth2 + JWT.

### Obtendo o token

```http
POST /oauth2/token
Authorization: Basic bXljbGllbnRpZDpteWNsaWVudHNlY3JldA==
Content-Type: application/x-www-form-urlencoded

grant_type=password&username=maria@gmail.com&password=123456
```

Use o token retornado no header das requisições protegidas:

```http
Authorization: Bearer <token>
```

### Usuários de teste

| E-mail | Senha | Perfil |
|--------|-------|--------|
| alex@gmail.com | 123456 | CLIENT |
| maria@gmail.com | 123456 | ADMIN + CLIENT |

---

## Collection Postman

O repositório acompanha a collection e o environment do Postman para facilitar os testes:

- `DSCommerceDesafio_postman_collection.json`
- `DSCommerce_env_postman_environment.json`

Importe os dois arquivos no Postman, selecione o environment **DSCommerce env** e execute o request **Login** para gerar o token automaticamente nas variáveis de ambiente.

---

## Critérios do desafio atendidos

- [x] Mínimo de 12 commits no repositório
- [x] Endpoints públicos `GET /products` e `GET /products/{id}` sem necessidade de login
- [x] Login retornando token JWT
- [x] Endpoints `POST /PUT /DELETE /products` restritos ao perfil ADMIN
- [x] `GET /users/me` retorna o usuário logado
- [x] `GET /orders/{id}` e `POST /orders` funcionando
- [x] Usuário CLIENT não acessa pedido de outro usuário
- [x] `GET /categories` retorna todas as categorias

---

## Autor

Bruno — Estudante de Engenharia de Software na UNIFAA  
