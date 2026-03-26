# 🛒 API E-commerce — Prueba Técnica Java + Spring Boot

---

## 📌 Descripción

Desarrollar una **API REST segura** para un sistema básico de e-commerce utilizando **Java y Spring Boot**.

Esta prueba está diseñada para evaluar:

* Diseño de APIs REST
* Buenas prácticas de arquitectura
* Manejo de validaciones y errores
* Seguridad con JWT
* Persistencia con base de datos relacional
* Preparación de una aplicación para entornos reales

---

## 🎯 Objetivos

La solución debe demostrar:

* Código limpio y mantenible
* Separación de responsabilidades
* Seguridad en endpoints
* Uso correcto de HTTP (métodos y status codes)
* Escalabilidad

---

# 🧩 1. Modelo de Dominio

---

## 👤 Usuario

| Campo     | Tipo          | Restricciones      |
| --------- | ------------- | ------------------ |
| id        | Long          | Clave primaria     |
| email     | String        | Obligatorio, único |
| password  | String        | Encriptado         |
| role      | Enum          | USER / ADMIN       |
| createdAt | LocalDateTime | Autogenerado       |

---

## 📂 Categoría

| Campo       | Tipo          | Restricciones      |
| ----------- | ------------- | ------------------ |
| id          | Long          | Clave primaria     |
| name        | String        | Obligatorio, único |
| description | String        | Opcional           |
| active      | boolean       | Default: true      |
| createdAt   | LocalDateTime | Autogenerado       |

---

## 📦 Producto

| Campo     | Tipo          | Restricciones  |
| --------- | ------------- | -------------- |
| id        | Long          | Clave primaria |
| name      | String        | Obligatorio    |
| price     | BigDecimal    | Obligatorio    |
| category  | Category      | ManyToOne      |
| createdAt | LocalDateTime | Autogenerado   |

---

# 🔐 2. Autenticación y Autorización

---

## 🔑 Endpoints de autenticación

```http
POST /v1/auth/register
POST /v1/auth/login
```

---

## 📥 Request de registro

```json
{
  "email": "user@mail.com",
  "password": "123456"
}
```

---

## 📥 Request de login

```json
{
  "email": "user@mail.com",
  "password": "123456"
}
```

---

## 📤 Respuesta

```json
{
  "token": "jwt-token",
  "type": "Bearer"
}
```

---

## 🔒 Requisitos de seguridad

* Usar Spring Security
* Autenticación stateless (sin sesión)
* Encriptación de contraseñas (BCrypt)
* Uso de JWT
* En cada request protegido se debe enviar:

```http
Authorization: Bearer <token>
```

---

# 📂 3. Endpoints de la API

---

## 📂 Categorías

```http
POST   /v1/categories        (ADMIN)
GET    /v1/categories        (USER)
GET    /v1/categories/{id}   (USER)
PUT    /v1/categories/{id}   (ADMIN)
DELETE /v1/categories/{id}   (ADMIN)
```

---

## 📦 Productos

```http
POST   /v1/products          (ADMIN)
GET    /v1/products          (USER)
GET    /v1/products/{id}     (USER)
DELETE /v1/products/{id}     (ADMIN)
```

---

## 👤 Usuarios

```http
GET /v1/users                (ADMIN)
GET /v1/users/me             (USER)
```

---

# 🔐 4. Reglas de autorización

| Endpoint                  | Acceso  |
| ------------------------- | ------- |
| Auth                      | Público |
| Crear categoría           | ADMIN   |
| Editar/Eliminar categoría | ADMIN   |
| Crear producto            | ADMIN   |
| Eliminar producto         | ADMIN   |
| Ver productos/categorías  | USER    |
| Ver usuarios              | ADMIN   |

---

# ✅ 5. Validaciones

* Email con formato válido
* Password mínimo 6 caracteres
* Nombre de categoría único
* Producto debe pertenecer a una categoría existente

---

# ⚠️ 6. Manejo de errores

Se debe implementar un manejador global de excepciones.

| Escenario              | Código HTTP      |
| ---------------------- | ---------------- |
| Error de validación    | 400 Bad Request  |
| No autenticado         | 401 Unauthorized |
| Sin permisos           | 403 Forbidden    |
| Recurso no encontrado  | 404 Not Found    |
| Conflicto (duplicados) | 409 Conflict     |

---

# 🏗️ 7. Arquitectura

Se debe seguir una arquitectura en capas:

```text
Controller → Service → Repository
           ↓
      Security (JWT)
```

---

# 📦 8. Estructura del proyecto (sugerida)

```text
com.example.ecommerce
│
├── auth
├── security
├── user
├── category
├── product
├── dto
├── exception
├── config
```

---

# 🧠 9. Requisitos técnicos

* Java 17+
* Spring Boot
* Spring Data JPA
* Spring Security
* JWT
* PostgreSQL

---

# 🐳 10. Docker

Se debe incluir:

* Dockerfile de la aplicación
* docker-compose.yml con:

    * aplicación
    * base de datos (PostgreSQL)

---

# 📬 11. Postman

Incluir una colección de Postman con:

* Registro
* Login
* Requests autenticados
* Variables:

    * base_url
    * token

---

# 📊 12. Documentación API

Usar Swagger / OpenAPI:

```text
http://localhost:8080/swagger-ui.html
```

---

# 🧪 13. Testing

Se debe incluir:

* Tests unitarios (servicios)
* Tests de integración (controladores)

Herramientas:

* JUnit 5
* Mockito
* SpringBootTest

---

# 📦 14. Base de datos

* PostgreSQL
* JPA / Hibernate

---

## 🔁 Migraciones (recomendado)

* Flyway o Liquibase

---

# 📈 15. Observabilidad (bonus)

* Spring Boot Actuator

```text
/actuator/health
```

---

# 🔥 16. Funcionalidades extra (opcional pero recomendado)

* Paginación y filtros
* Soft delete
* MapStruct (DTO mapping)
* Logging
* Seguridad por roles (`@PreAuthorize`)
* Endpoint `/me`
* Expiración de token

---

# 🚀 17. Ejecución del proyecto

```bash
git clone <repo>
cd ecommerce-api
docker-compose up --build
```

---

# 🧠 18. Criterios de evaluación

| Área              | Evaluación            |
| ----------------- | --------------------- |
| Código            | Limpio y mantenible   |
| Arquitectura      | Bien estructurada     |
| Seguridad         | JWT bien implementado |
| REST              | Uso correcto de HTTP  |
| Validaciones      | Correctas             |
| Manejo de errores | Consistente           |
| Docker            | Funcional             |
| Testing           | Cobertura y calidad   |

---

# 💣 Notas finales

* NO exponer entidades directamente
* Usar DTOs para requests/responses
* Controladores livianos
* Lógica en servicios
* Manejo global de excepciones