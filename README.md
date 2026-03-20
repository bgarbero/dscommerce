# DSCommerce

[![Java](https://img.shields.io/badge/Java-21-orange?style=flat-square&logo=openjdk)](https://www.oracle.com/java/)
[![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.4.5-brightgreen?style=flat-square&logo=springboot)](https://spring.io/projects/spring-boot)
[![Spring Security](https://img.shields.io/badge/Spring_Security-OAuth2_+_JWT-brightgreen?style=flat-square&logo=springsecurity)](https://spring.io/projects/spring-security)
[![H2](https://img.shields.io/badge/H2-In--Memory-blue?style=flat-square)](https://www.h2database.com/)
[![Maven](https://img.shields.io/badge/Maven-3.9-red?style=flat-square&logo=apachemaven)](https://maven.apache.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Concluído-success?style=flat-square)]()

> API RESTful de um sistema de e-commerce desenvolvida como projeto de estudo do curso **Java Spring Professional** da [DevSuperior](https://devsuperior.com.br/), ministrado pelo Prof. Nélio Alves. Cobre desde modelagem de domínio com JPA até autenticação e autorização com OAuth2 + JWT.

---

## 📋 Sumário

- [Sobre o Projeto](#sobre-o-projeto)
- [Screenshots](#screenshots)
- [Modelo Conceitual](#modelo-conceitual)
- [Stack Tecnológica](#stack-tecnológica)
- [Módulos do Curso](#módulos-do-curso)
- [Guia de Execução](#guia-de-execução)
- [Documentação da API](#documentação-da-api)
- [Controle de Acesso](#controle-de-acesso)
- [Licença](#licença)
- [Autor](#autor)

---

## Sobre o Projeto

O **DSCommerce** é uma API REST de e-commerce que permite o gerenciamento de produtos, categorias e pedidos, com autenticação baseada em tokens JWT via OAuth2. O projeto foi construído incrementalmente ao longo do curso, aplicando boas práticas de arquitetura em camadas (Controller → Service → Repository), tratamento de exceções customizado, Bean Validation e controle de acesso por papéis (`ROLE_ADMIN` e `ROLE_CLIENT`).

---

## Screenshots

### Swagger UI — Endpoints

| Product Controller | Order / User / Category Controllers |
|:-:|:-:|
| ![Product Controller](https://github.com/user-attachments/assets/97790ce1-2515-4ffe-86b5-fea07bd8e4aa) | ![Demais Controllers](https://github.com/user-attachments/assets/40f1f471-4326-45c1-8395-77aeb95c8741) |

### Swagger UI — Schemas (DTOs)

| Schemas — Parte 1 | Schemas — Parte 2 |
|:-:|:-:|
| ![Schemas Parte 1](https://github.com/user-attachments/assets/68c02830-f437-4752-820e-31830cb66a2b) | ![Schemas Parte 2](https://github.com/user-attachments/assets/4e4fcf3b-0001-4849-9740-934e9739cfe2) |

---

## Modelo Conceitual

```
┌──────────────────┐        ┌──────────────────┐
│      User        │        │    Category      │
│──────────────────│        │──────────────────│
│ id               │        │ id               │
│ name             │        │ name             │
│ email            │        └────────┬─────────┘
│ password         │                 │ N
│ phone            │                 │
│ birthDate        │        ┌────────┴─────────┐
│ roles (N:N)      │        │     Product      │
└────────┬─────────┘        │──────────────────│
         │ 1                │ id               │
         │                  │ name             │
         │                  │ description      │
┌────────┴─────────┐        │ price            │
│      Order       │        │ imgUrl           │
│──────────────────│  N:N   │ categories       │
│ id               ├────────┤                  │
│ moment           │        └──────────────────┘
│ status (enum)    │
│ payment (1:1)    │
│ items (1:N)      │
└──────────────────┘

OrderStatus: WAITING_PAYMENT | PAID | SHIPPED | DELIVERED | CANCELED
```

---

## Stack Tecnológica

**Backend**
- Java 21
- Spring Boot 3.4.5
- Spring Web (REST)
- Spring Data JPA / Hibernate
- Spring Security
- Spring Security OAuth2 Authorization Server
- Spring Security OAuth2 Resource Server (JWT)
- Bean Validation (jakarta.validation)
- SpringDoc OpenAPI (Swagger UI)

**Banco de Dados**
- H2 (perfil `test` — banco em memória)

**Ferramentas**
- Maven 3.9
- Git
- Postman (coleção inclusa em `/postman`)

---

## Módulos do Curso

| # | Módulo | Conceitos Aplicados |
|---|--------|---------------------|
| 1 | Componentes e Injeção de Dependência | `@Component`, `@Service`, `@Repository`, `@Autowired` |
| 2 | Modelo de Domínio e ORM | Entidades JPA, relacionamentos `@ManyToMany`, `@OneToMany`, `@Embeddable` |
| 3 | API REST, Camadas, CRUD, Exceções e Validações | Controllers, DTOs, `@ControllerAdvice`, Bean Validation, `ResponseEntity` |
| 4 | JPA, Consultas SQL e JPQL | `@Query`, JPQL, projeções com `interface` (Spring Data Projections) |
| 5 | Login e Controle de Acesso | OAuth2, JWT, `@PreAuthorize`, `ROLE_ADMIN`, `ROLE_CLIENT` |
| 6 | Homologação e Implantação com CI/CD | Perfis Spring (`test`/`prod`), variáveis de ambiente, deploy |

---

## Guia de Execução

### Pré-requisitos

- JDK 21+
- Maven 3.9+
- Git

### Instalação

```bash
# Clonar o repositório
git clone https://github.com/seu-usuario/dscommerce.git

# Entrar na pasta do projeto
cd dscommerce

# Compilar e executar (perfil test com H2)
./mvnw spring-boot:run
```

> **Windows:** use `mvnw.cmd spring-boot:run`

### Acessar o banco em memória (H2 Console)

```
URL:      http://localhost:8080/h2-console
JDBC URL: jdbc:h2:mem:testdb
User:     sa
Password: 
```

### Acessar a documentação interativa (Swagger UI)

```
http://localhost:8080/swagger-ui/index.html
```

---

## Documentação da API

### Autenticação

Para acessar os endpoints protegidos, obtenha um token JWT via OAuth2 Password Grant:

```http
POST /oauth2/token
Content-Type: application/x-www-form-urlencoded

grant_type=password&username=maria@gmail.com&password=123456
```

**Response:**
```json
{
  "access_token": "eyJhbGciOiJSUzI1NiJ9...",
  "token_type": "Bearer",
  "expires_in": 86400
}
```

Use o token no header de todas as requisições protegidas:
```
Authorization: Bearer <access_token>
```

---

### Produtos `/products`

| Método | Rota | Acesso | Descrição |
|--------|------|--------|-----------|
| `GET` | `/products/{id}` | Público | Busca produto por ID |
| `GET` | `/products?name=&page=&size=` | Público | Lista produtos paginados com filtro por nome |
| `POST` | `/products` | `ROLE_ADMIN` | Cadastra novo produto |
| `PUT` | `/products/{id}` | `ROLE_ADMIN` | Atualiza produto |
| `DELETE` | `/products/{id}` | `ROLE_ADMIN` | Remove produto |

**Request Body (POST/PUT):**
```json
{
  "name": "Notebook Gamer",
  "description": "Processador Intel i7, 16GB RAM, SSD 512GB",
  "price": 4599.90,
  "imgUrl": "https://img.example.com/notebook.jpg",
  "categories": [
    { "id": 3 }
  ]
}
```

**Response (200 OK):**
```json
{
  "id": 26,
  "name": "Notebook Gamer",
  "description": "Processador Intel i7, 16GB RAM, SSD 512GB",
  "price": 4599.90,
  "imgUrl": "https://img.example.com/notebook.jpg",
  "categories": [
    { "id": 3, "name": "Computadores" }
  ]
}
```

---

### Categorias `/categories`

| Método | Rota | Acesso | Descrição |
|--------|------|--------|-----------|
| `GET` | `/categories` | Público | Lista todas as categorias |

**Response (200 OK):**
```json
[
  { "id": 1, "name": "Livros" },
  { "id": 2, "name": "Eletrônicos" },
  { "id": 3, "name": "Computadores" }
]
```

---

### Pedidos `/orders`

| Método | Rota | Acesso | Descrição |
|--------|------|--------|-----------|
| `GET` | `/orders/{id}` | `ROLE_ADMIN` ou dono do pedido | Busca pedido por ID |
| `POST` | `/orders` | `ROLE_CLIENT` | Cria novo pedido |

**Request Body (POST):**
```json
{
  "items": [
    { "productId": 1, "quantity": 2 },
    { "productId": 5, "quantity": 1 }
  ]
}
```

**Response (201 Created):**
```json
{
  "id": 4,
  "moment": "2024-06-20T14:35:00Z",
  "status": "WAITING_PAYMENT",
  "client": { "id": 1, "name": "Maria Brown" },
  "payment": null,
  "items": [
    { "productId": 1, "name": "The Lord of the Rings", "price": 90.50, "quantity": 2, "subTotal": 181.00 },
    { "productId": 5, "name": "Rails for Dummies", "price": 100.99, "quantity": 1, "subTotal": 100.99 }
  ],
  "total": 281.99
}
```

---

### Usuário logado `/users`

| Método | Rota | Acesso | Descrição |
|--------|------|--------|-----------|
| `GET` | `/users/me` | `ROLE_ADMIN` ou `ROLE_CLIENT` | Retorna dados do usuário autenticado |

**Response (200 OK):**
```json
{
  "id": 1,
  "name": "Maria Brown",
  "email": "maria@gmail.com",
  "phone": "988888888",
  "birthDate": "2001-07-25"
}
```

---

## Controle de Acesso

| Papel | Permissões |
|-------|-----------|
| `ROLE_ADMIN` | CRUD completo de produtos; leitura de qualquer pedido |
| `ROLE_CLIENT` | Criar pedidos; visualizar somente os próprios pedidos |
| Público (sem token) | Listar e buscar produtos; listar categorias |

Credenciais de teste disponíveis na coleção Postman em `/postman`.

---

## Licença

Este repositório é para estudos e está disponível sob uma licença permissiva. Abaixo o texto da MIT License (cópia livre). Sinta-se à vontade para usar e modificar o código.

Licença MIT

Copyright (c) 2026 Bruno Garbero

É concedida permissão, gratuitamente, a qualquer pessoa que obtenha uma cópia deste software e arquivos de documentação associados (o "Software"), para lidar com o Software sem restrições, incluindo, sem limitação, os direitos de usar, copiar, modificar, fundir, publicar, distribuir, sublicenciar e/ou vender cópias do Software, e para permitir que as pessoas a quem o Software é fornecido o façam, sujeitas às seguintes condições:

O aviso de direitos autorais acima e este aviso de permissão devem ser incluídos em todas as cópias ou partes substanciais do Software.

O SOFTWARE É FORNECIDO "NO ESTADO EM QUE SE ENCONTRA", SEM GARANTIA DE QUALQUER TIPO, EXPRESSA OU IMPLÍCITA, INCLUINDO, MAS NÃO SE LIMITANDO ÀS GARANTIAS DE COMERCIALIZAÇÃO, ADEQUAÇÃO A UM FIM ESPECÍFICO E NÃO VIOLAÇÃO. EM NENHUMA HIPÓTESE OS AUTORES OU DETENTORES DOS DIREITOS AUTORAIS SERÃO RESPONSÁVEIS POR QUAISQUER REIVINDICAÇÕES, DANOS OU OUTRAS RESPONSABILIDADES, SEJA EM AÇÃO CONTRATUAL, EXTRACONTRATUAL OU DE OUTRA NATUREZA, DECORRENTES DE, OU RELACIONADAS COM O SOFTWARE OU O USO OU OUTRAS NEGOCIAÇÕES COM O SOFTWARE.

---

## Autor

**Bruno**  
Estudante de Engenharia de Software — UNIFAA  
Técnico de Suporte em TI | Desenvolvedor Backend Java (em formação)

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Conectar-blue?style=flat-square&logo=linkedin)](https://linkedin.com/in/seu-perfil)
[![GitHub](https://img.shields.io/badge/GitHub-Portfólio-black?style=flat-square&logo=github)](https://github.com/seu-usuario)
[![Gmail](https://img.shields.io/badge/Email-Contato-red?style=flat-square&logo=gmail)](mailto:seuemail@gmail.com)

---

> Projeto desenvolvido durante o curso **Java Spring Professional** — [DevSuperior](https://devsuperior.com.br/) · Prof. Nélio Alves
