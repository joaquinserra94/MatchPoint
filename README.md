# MatchPoint

**Gamifica la convivencia en pareja.**  
MatchPoint convierte **actividades** y **tareas del hogar** en **puntos** y **recompensas** con un toque lúdico, visual y motivador.

- Cada persona suma puntos por tareas/actividades.
- Bonus por “esfuerzo extra” (lo que normalmente no harías).
- Objetivo semanal con barra de progreso.
- Ranking semanal y canje de recompensas.
- PWA: instálala en el móvil como si fuera una app.

---

## 🚀 Estado del proyecto
MVP en desarrollo (proyecto personal / aprendizaje).  
Metodología ágil con tablero en Asana y sprints de 2 semanas.

---

## ✨ Funcionalidades (MVP)
- [x] Definición de producto y brief  
- [ ] Registro rápido de actividad/tarea + suma de puntos  
- [ ] Objetivo semanal + barra de progreso  
- [ ] Ranking semanal (puntos individuales)  
- [ ] Recompensas personalizadas y canje  
- [ ] Historial por usuario  
- [ ] PWA (manifest + service worker)  
- [ ] Despliegue en Render

**Futuro**
- [ ] Bonus configurables por persona  
- [ ] Notificaciones motivacionales  
- [ ] Logros / medallas  
- [ ] Estadísticas avanzadas

---

## 🧱 Stack
- **Frontend:** HTML/CSS/JS (PWA). *(Más adelante: Flutter o React/Next)*
- **Backend:** Python **FastAPI**  
- **DB:** SQLite (MVP) → (futuro: Postgres/Firebase)  
- **Infra:** Render (deploy)  
- **Gestión:** Asana + Everhour (time tracking)  
- **Diseño:** Figma (UI redondeada, juguetona. Tipografía: Poppins/Nunito)

---

## 📁 Estructura sugerida
```
matchpoint/
  backend/
    app/
      main.py            # FastAPI
      models.py          # SQLAlchemy/Pydantic
      routes/            # endpoints
      services/          # lógica (puntos, ranking, canjes)
    tests/
    requirements.txt
  frontend/
    index.html
    /static/
      style.css
      app.js
      manifest.json
      service-worker.js
    README.md
  docs/
    Brief_MatchPoint.txt
    UML/                 # .puml, .png (clases, secuencia)
    screenshots/
  .github/workflows/ci.yml
  .env.example
  README.md
```
> Ajusta rutas si ya tienes otra estructura.

---

## 🛠️ Puesta en marcha (local)
### Requisitos
- Python 3.11+
- (Opcional) Node 18+ si decides usar toolchain para el frontend

### Backend
```bash
cd backend
python -m venv .venv
source .venv/bin/activate      # Windows: .venv\Scripts\activate
pip install -r requirements.txt

# Ejecutar (ajusta el import a tu estructura real)
uvicorn app.main:app --reload
```
### Frontend (estático)
Abre `frontend/index.html` en el navegador durante el desarrollo.  
Si sirves estáticos desde FastAPI, monta la carpeta `/frontend/static`.

---

## 🔐 Variables de entorno
Crea un `.env` desde el ejemplo:
```
APP_ENV=dev
DATABASE_URL=sqlite:///./matchpoint.db
SECRET_KEY=change-me
```
Carga las variables en FastAPI (por ejemplo, `python-dotenv`).

---

## 📦 Despliegue en Render (sencillo)
**Web Service (Python)**
- **Build Command**
  ```bash
  pip install -r backend/requirements.txt
  ```
- **Start Command**
  ```bash
  uvicorn app.main:app --host 0.0.0.0 --port 10000
  ```
  > Ajusta `app.main:app` a tu paquete real.  
- Añade variables de entorno (las del `.env`).  
- Si sirves estáticos, asegúrate de montar `/frontend/static`.

---

## 📱 PWA (instalable)
Incluye estos archivos en `frontend/static/`:

**`manifest.json`**
```json
{
  "name": "MatchPoint",
  "short_name": "MatchPoint",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#1C2833",
  "theme_color": "#1C2833",
  "icons": [
    { "src": "/static/icon-192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "/static/icon-512.png", "sizes": "512x512", "type": "image/png" }
  ]
}
```

**`service-worker.js`**
```js
self.addEventListener('install', e => {
  e.waitUntil(
    caches.open('mp-cache-v1').then(cache =>
      cache.addAll(['/', '/static/style.css', '/static/app.js', '/static/manifest.json'])
    )
  );
});
self.addEventListener('fetch', e => {
  e.respondWith(caches.match(e.request).then(r => r || fetch(e.request)));
});
```
En tu `index.html`:
```html
<link rel="manifest" href="/static/manifest.json" />
<script>
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/static/service-worker.js');
}
</script>
```

---

## 🧪 Tests (placeholder)
- Backend: `pytest`  
- Lint/format: `flake8`, `black` (opcional: `pre-commit`)

---

## 🧩 Modelado (UML)
- **Clases:** Usuario, Pareja, Tarea, Recompensa, PuntoTransaccion, etc.  
- **Secuencias:** Registrar tarea → sumar puntos → recalcular ranking → respuesta.  
Guarda los `.puml`/`.png` en `docs/UML/`.

---

## 🗺️ Roadmap corto (Sprints)
**Sprint 1**
- Home (puntos + objetivo y progreso)
- Registro de actividad/tarea
- Estructura backend + DB
- Deploy inicial en Render

**Sprint 2**
- Ranking semanal
- Recompensas + canje
- Historial por usuario

**Sprint 3**
- PWA completa (manifest + SW)
- Pulido UI (estilo redondeado, Poppins/Nunito)
- Métricas básicas

---

## 🤝 Contribuir
Proyecto personal de aprendizaje. PRs bienvenidos cuando esté estable.  
Estandariza con `black` + `flake8` (ver `.pre-commit-config.yaml` si lo agregas).

---

## 📄 Licencia
MIT (o la que prefieras). Añade un `LICENSE` si haces público el repositorio.

---

## 📬 Contacto
Creador: **Joaquín**  
Descripción/brief y documentos en `docs/`.
