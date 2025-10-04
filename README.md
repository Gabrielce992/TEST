Aquí tienes la **versión afinada del README.md**, lista para subir a tu repositorio **junto con el diagrama** y además incluir la parte final que describes (motivación, validación y checklist).
La preparé para que sea **profesional, limpia y clara** para que quien reciba el correo pueda levantar el proyecto sin dudas. 🚀

---

```markdown
# 🚌 BusApp — Sistema de Gestión de Buses

Proyecto **Full-Stack** desarrollado como reto técnico.  
Incluye **API REST segura** (Spring Boot + PostgreSQL), **frontend web** (React + Vite) y colección **Postman** para validación.

---

## 📂 Estructura del Repositorio

```

BusApp/
├── backend/                  # API Spring Boot + PostgreSQL
│   ├── src/main/java/com/busapp/proyect
│   │   ├── BusAppApplication.java
│   │   ├── config/           # Security, CORS
│   │   ├── controller/       # BusController
│   │   ├── dto/              # BusDTO
│   │   ├── model/            # Bus, Brand
│   │   ├── repository/       # JPA Repos
│   │   ├── service/          # BusService
│   │   └── exceptions/       # GlobalExceptionHandler
│   ├── src/main/resources
│   │   ├── application.properties
│   │   └── data.sql
│   └── pom.xml
│
├── frontend/                 # React + Vite
│   ├── src/
│   │   ├── App.jsx
│   │   ├── main.jsx
│   │   ├── components/tables/BusesTable.jsx
│   │   ├── services/busService.js
│   │   └── styles/
│   ├── package.json
│   └── .env
│
└── postman/
└── BusApp.postman_collection.json

````

---

## 📊 Diagrama de Arquitectura

> **Espacio reservado para el diagrama**  
> (_Ejemplo: flujo Frontend ↔ Backend ↔ BD_)  
>  
> Colocar aquí `diagrama_busapp.png` o `diagrama.drawio` con la siguiente lógica:
> - Usuario interactúa con **Frontend (React + Vite)**
> - El frontend consume **API REST (Spring Boot)**
> - La API consulta datos en **PostgreSQL**
> - Se realizan pruebas automatizadas con **Postman**

---

## ⚙️ Requisitos Previos

- Java **21+**
- Maven **3+**
- PostgreSQL **15+**
- Node.js **18+**
- Postman (última versión)
- IDEs recomendados:
  - Backend → IntelliJ IDEA Ultimate
  - Frontend → VS Code

---

## 🔥 Configuración del Backend

1. **Base de datos PostgreSQL**
   ```sql
   CREATE DATABASE busapp;
   CREATE USER postgres WITH PASSWORD 'Papahuevo.1';
   GRANT ALL PRIVILEGES ON DATABASE busapp TO postgres;
````

2. **application.properties**

   ```properties
   spring.datasource.url=jdbc:postgresql://localhost:5432/busapp
   spring.datasource.username=postgres
   spring.datasource.password=Papahuevo.1

   spring.jpa.hibernate.ddl-auto=update
   spring.sql.init.mode=always
   spring.jackson.serialization.WRITE_DATES_AS_TIMESTAMPS=false
   ```

3. **Datos iniciales (`data.sql`)**

   ```sql
   INSERT INTO brand (nombre)
   SELECT 'Volvo' WHERE NOT EXISTS (SELECT 1 FROM brand WHERE nombre='Volvo');

   INSERT INTO brand (nombre)
   SELECT 'Scania' WHERE NOT EXISTS (SELECT 1 FROM brand WHERE nombre='Scania');

   INSERT INTO bus (numero_bus, placa, fecha_creacion, caracteristicas, marca_id, estado)
   SELECT 'B-100','ABC-101',now(),'50 asientos, A/C',b.id,'ACTIVO'
   FROM brand b WHERE b.nombre='Volvo'
   AND NOT EXISTS (SELECT 1 FROM bus WHERE placa='ABC-101');
   ```

4. **Ejecutar Backend**

   ```bash
   cd backend
   mvn spring-boot:run
   ```

   👉 API disponible en `http://localhost:8080`

---

## ⚛️ Configuración del Frontend

1. **Instalar dependencias**

   ```bash
   cd frontend
   npm install
   ```

2. **Configurar `.env`**

   ```
   VITE_BACKEND_URL=http://localhost:8080
   ```

3. **Ejecutar Frontend**

   ```bash
   npm run dev
   ```

   👉 UI disponible en `http://localhost:5173`

---

## 🔑 Seguridad

* Autenticación **Basic Auth**
* Usuario por defecto:

  ```
  user / password
  ```
* Header de autorización:

  ```
  Authorization: Basic dXNlcjpwYXNzd29yZA==
  ```

---

## 📂 Endpoints Principales

| Método | Endpoint  | Descripción             | Auth |
| ------ | --------- | ----------------------- | ---- |
| GET    | /bus      | Listar buses (paginado) | ✅    |
| GET    | /bus/{id} | Obtener bus por ID      | ✅    |
| POST   | /bus      | Crear nuevo bus         | ✅    |

Ejemplo:

```
GET http://localhost:8080/bus?page=0&size=10
Authorization: Basic dXNlcjpwYXNzd29yZA==
```

---

## 🧪 Pruebas con Postman

La colección se encuentra en:
`postman/BusApp.postman_collection.json`

### 1. Importar colección

* Abrir Postman → `Import`
* Seleccionar archivo `BusApp.postman_collection.json`

### 2. Configurar Variables

* `baseUrl` → `http://localhost:8080`
* `basicAuthHeader` → `Basic dXNlcjpwYXNzd29yZA==`

### 3. Requests incluidos

* **Get Buses (paginado)**
* **Get Bus by ID**
* **Create Bus (dinámico con script pre-request)**

Ejemplo Body para POST:

```json
{
  "numeroBus": "{{numeroBus}}",
  "placa": "{{placa}}",
  "caracteristicas": "{{caracteristicas}}",
  "marcaId": {{marcaId}},
  "estado": "{{estado}}"
}
```

---

## 🌐 Frontend en Acción

El frontend permite:

* Ver lista de buses paginada
* Consultar detalles por ID
* Navegar entre páginas

---

## 🚀 Flujo de Trabajo

1. Levantar PostgreSQL
2. Ejecutar backend → `mvn spring-boot:run`
3. Ejecutar frontend → `npm run dev`
4. Validar API con Postman
5. Visualizar buses en `http://localhost:5173`

---

## ✅ Checklist de Verificación

### Backend

* [x] Modelo Bus con campos requeridos + relación con Marca
* [x] Endpoints `/bus` y `/bus/{id}` con paginación
* [x] Seguridad con Basic Auth
* [x] Base de datos PostgreSQL configurada
* [x] Proyecto subido a repositorio remoto

### Frontend

* [x] React 18+, uso de useState y useEffect
* [x] Tabla que muestra datos de la API
* [x] Consumo de `/bus/{id}`
* [x] Paginación en la tabla
* [x] Repositorio remoto disponible

### Postman

* [x] Colección importable con variables y scripts
* [x] Validación dinámica de creación de buses

---

## 📧 Envío del Proyecto

Reto completado según los lineamientos:

* Backend: Spring Boot + PostgreSQL
* Frontend: React + Vite
* Validación: Postman
* Todo versionado en GitHub
* Entregar los enlaces del repositorio al correo:
  `rbazan@civa.com.pe`

---

## 📜 Licencia

Proyecto académico — uso educativo.

---

## 👤 Autor

**Técnico Practicante — Full-Stack Java & React**
Reto solicitado por **Civa** — 2025

```

---

✅ **Listo para subir a tu Git:**  
- `README.md` (este archivo)  
- `diagrama_busapp.png` (en la raíz o carpeta `/docs`)  
- Código del proyecto + colección Postman  

¿Quieres que te genere también el **archivo README.md descargable (.md)** y un **ejemplo de diagrama en PNG** para agregar al repo? 🚀
```
