# Fase 1 - Arquitectura de Eventia

Antes de ponerme a probar fallos, he revisado cómo está montada la app por dentro y qué hace cada archivo.

## Preguntas sobre cómo funciona

1. **¿Qué partes tiene la aplicación?**
   Está dividida en dos carpetas:
   - **`client/` (Frontend):** Es la web que ves en el navegador (`http://localhost:5173`), hecha con HTML, CSS, JavaScript y Vite.
   - **`server/` (Backend):** Es el servidor que hace el trabajo por detrás (`http://localhost:3000`), montado con Node.js y Express.

2. **¿De qué se encarga el frontend?**
   De todo lo que ve y toca el usuario: enseñar los eventos, el buscador, el formulario para entrar con tu cuenta, el botón de reservar y el panel de admin para crear eventos nuevos. Cuando haces clic en algo, le manda la orden al servidor.

3. **¿Cómo hablan entre ellos?**
   Por peticiones HTTP (`GET`, `POST`, `DELETE`) llamando a las rutas de la API del servidor en `http://localhost:3000/api`.

4. **¿Qué datos se mandan?**
   Se pasan todo en formato JSON. Por ejemplo, al iniciar sesión la web manda el correo y la contraseña, y el servidor le devuelve un token y los datos del usuario. Con las reservas y los eventos pasa igual.

5. **¿Dónde se guardan los datos?**
   No hay base de datos. Todo (usuarios, eventos, reservas y tokens) se guarda en memoria RAM dentro de listas (arrays) y un `Map` en el código del servidor.

6. **¿Qué pasa si reinicias el servidor?**
   Como está todo en memoria RAM, si apagas o reinicias el servidor se borra todo lo que hayas hecho (reservas nuevas, eventos creados o sesiones) y vuelven a salir los datos que venían al principio.

## Archivos principales

La app deja ver los eventos, filtrarlos en el buscador, iniciar sesión (guardando el token en el `localStorage` del navegador), reservar plazas y, si entras como admin, crear eventos nuevos y ver todas las reservas.

- **En `client/`:**
  - `index.html`: La página web principal.
  - `src/api.js`: Tiene la función `apiRequest` que hace las llamadas `fetch` al servidor y mete el token en la cabecera `Authorization`.
  - `src/main.js`: Es el JS que controla la pantalla: pinta los eventos, recoge los datos de los formularios y manda las reservas cuando pulsas el botón.
  - `src/style.css`: El diseño y los colores de la web.

- **En `server/`:**
  - `src/server.js`: Es el archivo principal del servidor. Arranca Express en el puerto 3000, comprueba si llevas token y tiene todas las rutas (`/api/auth/login`, `/api/events`, `/api/reservations`, etc.).
  - `src/data.js`: Tiene los datos de prueba en memoria (los usuarios `ana`, `luis` y `admin`, los 3 eventos iniciales, las reservas y el `Map` de sesiones).
  - `src/legacy/funciones_inseguras.js`: Un archivo aparte con tres ejemplos de código mal hecho (`eval()`, una consulta SQL juntando texto a pelo y una clave escrita en el código) para revisarlo luego con Semgrep.