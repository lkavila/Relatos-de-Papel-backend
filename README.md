# Relatos-de-Papel---backend

## Consumo de Back-End desde Postman

Para probar los servicios back-end de forma directa puedes importar la colección de Postman incluida en este repositorio:

📁 [`Relatos de papel postman_collection.json`](./Relatos%20de%20papel.postman_collection.json))

Las carpetas de la colección relevantes para este tema son:

### 📂 01 - Supplies Catalogue
Agrupa las operaciones CRUD sobre el catálogo de suministros: creación de nuevos supplies, consulta individual o listada (con soporte para filtros y paginación), modificación parcial o completa, y eliminación.

### 📂 02 - Orders
Recoge las operaciones relacionadas con los pedidos: creación de una nueva orden indicando los suministros y cantidades deseadas, y consulta del listado de órdenes existentes.

### 📂 03 - Gateway
Incluye peticiones equivalentes a las anteriores pero canalizadas a través del API Gateway. Se divide en dos secciones:
- **Straight Gateway**: las peticiones se enrutan directamente al microservicio de destino manteniendo el verbo HTTP original.
- **ACL Gateway**: todas las peticiones se realizan como `POST`, incluyendo en el body el método destino y los parámetros necesarios, de forma que el gateway actúa como capa anti-corrupción.

---

## Proyecto Relatos de papel - con Gateway ACL (Anti-Corruption Layer)

### Front-end
https://github.com/TheBryan28/relatos-de-papel-UNIR-fullstack
```bash
npm install
npm run dev
```

### Back-end (repositorios externos)

| Servicio | Repositorio |
|----------|-------------|
| Catálogo | [back-end-supplies-catalogue](https://github.com/lkavila/RelatosDePapel-Catalog-Microservice) |
| Pedidos  | [back-end-supplies-orders](https://github.com/lkavila/RelatosDePapel-Orders-Microservice) |
| Eureka Server | [back-end-eureka](https://github.com/lkavila/RelatosDePapel-Eureka-Server) |
| Cloud Gateway ACL | [back-end-cloud-gateway-filters](https://github.com/lkavila/RelatosDePapel-Gateway) |

**Orden de arranque recomendado:**
1. **Eureka Server**
2. **Cloud Gateway Filters (ACL)**
3. **Catalogue** y **Orders**
4. **Front-end**

> Este gateway incorpora **filtros** que restringen la comunicación desde el front-end: únicamente se permiten peticiones **POST**. De esta forma, toda la información viaja dentro del **body** de la petición, lo que facilita su cifrado mediante un posible certificado SSL. Cualquier otro verbo HTTP (GET, PUT, DELETE…) será rechazado por el gateway, asegurando que la capa de presentación no pueda interactuar directamente con los endpoints REST estándar de los microservicios.
