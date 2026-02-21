# BE Lab 01 - Tipo de Cambio Perú (Spring Boot)

## 0) Objetivo
Implementar un endpoint **GET** que devuelva el tipo de cambio USD→PEN por fecha, usando **data en memoria**.

---

## 1) Levanta el proyecto
En la raíz del repo:

```powershell
.\mvnw.cmd spring-boot:run
```

Verifica que funciona:
- Ping: `GET http://localhost:8080/api/v1/ping`
- Swagger: `http://localhost:8080/swagger-ui/index.html`

---

## 2) Crea estos 3 archivos
> Respeta los paquetes/rutas tal cual.

### A) DTO de respuesta
**Archivo:** `src/main/java/pe/incubadora/backend/api/TipoCambioResponse.java`

Debe tener estos campos:
- `fecha` (String)
- `compra` (double)
- `venta` (double)
- `fuente` (String)

---

### B) Servicio con data en memoria
**Archivo:** `src/main/java/pe/incubadora/backend/application/TipoCambioService.java`

Debe tener un `Map<LocalDate, TipoCambioResponse>` con al menos **3 fechas precargadas**, por ejemplo:
- `2026-02-18`
- `2026-02-19`
- `2026-02-20`

Y un método:
- `TipoCambioResponse getByFecha(LocalDate fecha)`

---

### C) Controller del endpoint GET
**Archivo:** `src/main/java/pe/incubadora/backend/api/TipoCambioController.java`

Debe exponer:

- `GET /api/v1/tipo-cambio/{fecha}`
- `fecha` con formato `yyyy-MM-dd`

Comportamiento:
- Si `fecha` es inválida → **400** con un JSON de error
- Si no hay data para esa fecha → **404** con un JSON de error
- Si existe → **200** con la respuesta

---

## 3) Contrato del endpoint

### 200 OK
`GET http://localhost:8080/api/v1/tipo-cambio/2026-02-20`

```json
{
  "fecha": "2026-02-20",
  "compra": 3.72,
  "venta": 3.76,
  "fuente": "INTERNO"
}
```

### 400 Bad Request
Ejemplo:
- `GET /api/v1/tipo-cambio/hoy`

Debe devolver JSON con:
- `code`: `VALIDATION_ERROR`
- `message`: `Fecha inválida. Use formato yyyy-MM-dd`

### 404 Not Found
Ejemplo:
- `GET /api/v1/tipo-cambio/2026-01-01`

Debe devolver JSON con:
- `code`: `TC_NOT_FOUND`
- `message`: `No existe tipo de cambio para la fecha 2026-01-01`

---

## Checklist
- [ ] El proyecto arranca con `.\mvnw.cmd spring-boot:run`
- [ ] Swagger abre: `http://localhost:8080/swagger-ui/index.html`
- [ ] `GET /api/v1/tipo-cambio/2026-02-20` responde 200
- [ ] Fecha inválida responde 400
- [ ] Fecha sin data responde 404
