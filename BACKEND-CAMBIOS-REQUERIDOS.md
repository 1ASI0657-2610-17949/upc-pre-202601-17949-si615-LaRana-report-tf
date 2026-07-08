# Cambios de backend — Alimenta / La Rana (para el equipo de backend)

Cambios que el cliente Flutter necesita para funcionar de punta a punta. **Casi todo ya está
aplicado** en esta copia local del código (`services/`) y **compila limpio** (`mvnw compile`, Java 25).
Lo que falta es **reconstruir/redesplegar** los contenedores afectados y **exponer el WebSocket de
logistics**. Rutas exactas y detalle abajo.

## Resumen ejecutivo

| # | Cambio | Estado | Acción que te toca |
|---|--------|--------|--------------------|
| 1 | Gateway inyecta `X-User-Id` desde el JWT | ✅ aplicado local, compila | Redesplegar `api-gateway` |
| 2 | Gateway lee claim `role` (no `roles`) | ✅ aplicado local (dentro de #1) | Redesplegar `api-gateway` |
| 3 | Matching: listar ofertas por albergue (`?shelterId=`) | ✅ aplicado local, compila | Redesplegar `matching-service` |
| 4 | Matching: exponer coords en `MatchingResource` | ✅ aplicado local, compila | Redesplegar `matching-service` |
| 5 | Exponer el WebSocket de logistics (`/ws/delivery/track`) | ⬜ pendiente (decisión de infra) | Publicar puerto o rutear en gateway |

Redespliegue típico:
```
docker compose up -d --build api-gateway matching-service
```

---

## 1 y 2 — Gateway: inyectar `X-User-Id` desde el JWT (+ leer claim `role`)

**Problema.** `notifications/.../DeviceTokenController.java` exige el header `X-User-Id`, pero el
`HeaderPropagationFilter` del gateway solo reenvía una lista blanca que no lo incluye → el registro de
token FCM devolvía 401/400. Además el gateway leía el claim `roles` (array) cuando el auth-service
emite `role` (string), dejando las authorities vacías.

**Solución aplicada** (3 archivos en `gateway/api-gateway/`):
- `domain/security/JwtClaims.java` → el record ahora incluye `Long userId`.
- `infrastructure/security/JwtServiceImpl.java` → extrae `userId`; si `roles` viene vacío usa el
  claim `role`.
- `filters/HeaderPropagationFilter.java` → recibe `JwtService` e inyecta `x-user-id` derivado del JWT
  firmado (autoritativo, el cliente no lo puede falsificar).

Verificado: el gateway compila (`./mvnw -q -DskipTests clean compile` → OK).

**Comprobación tras desplegar:** con un token válido, `POST /api/v1/notifications/register`
(body `{deviceToken, platform}`) debe devolver **201** (ya no 401).

---

## 3 — Matching: listar ofertas por albergue

**Problema.** El matching envía la oferta automáticamente al mejor albergue, pero no había forma de
que la app de la ONG consultara sus ofertas (solo existía `?donationId=`).

**Solución aplicada** (`matching/matching-service/`):
- `MatchingRepository` → `List<Matching> findByShelterId(String shelterId)`.
- `MatchingQueryService` → método `findByShelterId`.
- `MatchingController` → nuevo `GET /api/v1/matching?shelterId={id}` (el existente se marcó
  `@GetMapping(params = "donationId")` para desambiguar). `shelterId` es el `userId` en formato string.

**Comprobación:** `GET /api/v1/matching?shelterId=10` con Bearer → 200 con la lista de matchings.

---

## 4 — Matching: exponer coordenadas en `MatchingResource`

**Problema.** Para iniciar una entrega (`POST /api/v1/deliveries`) se necesitan las coords del
restaurante, pero `MatchingResource` no las exponía (aunque el aggregate `Matching` sí las guarda).

**Solución aplicada** (`matching/matching-service/`):
- `interfaces/rest/resources/MatchingResource.java` → añadidos `pickupLatitude`, `pickupLongitude`,
  `shelterLatitude`, `shelterLongitude`.
- `interfaces/rest/transform/MatchingResourceFromEntityAssembler.java` → mapea esos getters.

Compila limpio. Retrocompatible (solo añade campos).

---

## 5 — ⬜ PENDIENTE: exponer el WebSocket de tracking de logistics

**Problema.** El tracking en vivo usa `ws://<host>/ws/delivery/track`
(`logistics/.../DeliveryWebSocketController`, registrado en `WebSocketConfig`). Ese path **no** está
en las rutas del gateway y el `docker-compose` solo publica el gateway (:80) y food-safety (:8000).
Por eso el cliente no puede conectarse al tracking en el servidor desplegado.

Los endpoints REST de entrega (`POST /api/v1/deliveries`, `/{id}/confirm`, `/{id}/archive`) **sí**
pasan por el gateway y funcionan.

**Opciones (elige una):**
- **A (rápida):** publicar el puerto de logistics en `docker-compose.yml` (p. ej. `- "8088:8080"`) y
  que el cliente apunte ahí con `--dart-define=LOGISTICS_WS_URL=ws://<host>:8088/ws/delivery/track`.
- **B (limpia):** rutear WebSockets en el gateway (Spring Cloud Gateway soporta upgrade WS) para que
  `ws://<host>/ws/delivery/track` funcione por el :80, y así no exponer el servicio directamente.

Mientras tanto la app degrada con gracia: la pantalla de entrega abre, permite iniciar/confirmar por
REST, y el mapa simplemente no recibe posiciones.

> Observación menor (no bloqueante): el broadcast del WS reenvía cada posición a **todas** las
> sesiones conectadas, sin filtrar por `delivery_id`. El cliente ya filtra por su `delivery_id`, pero
> a futuro convendría segmentar por entrega en el servidor.

---

## Notas de integración ya resueltas en el cliente
- La app usa **HTTP** contra el gateway (`http://168.62.52.205`); en Android se habilitó
  `usesCleartextTraffic`. Si migran el gateway a HTTPS, mejor aún (se puede quitar esa marca).
- El `applicationId` de Android es `alimenta.upc` (coincide con `google-services.json`, Firebase
  `alimenta-182f0`).
- JSON: los servicios Java usan camelCase; **logistics y food-safety usan snake_case** — el cliente
  ya modela cada uno por separado.
