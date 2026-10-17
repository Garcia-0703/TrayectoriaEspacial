# 🛰️ Sistema de análisis y optimización de una trayectoria espacial

App web interactiva (Streamlit + Plotly) — Proyecto integrador, Matemática III.
Trayectoria `r(t) = ((a+bt)cos t, (a+bt)sen t, ct)`, campo `T(x,y,z) = 20+4x−3y+2z+x²+y²+2z²−xy`.

## Estructura
```
index.html        # versión estática para GitHub Pages (JS + Plotly.js)
app.py            # UI (presentación): navegación, sliders, tablas, alertas/toasts
ui/plots.py       # figuras Plotly (3D, dashboard 2D, comparación)
core/             # lógica matemática pura (sin Streamlit, reutilizable en una API)
  trajectory.py   #   r, r', r'', r'''  (analíticas)
  field.py        #   T, ∇T, dT/dt, derivada direccional, Hessiana
  geometry.py     #   rapidez, aceleración, curvatura, torsión, Frenet-Serret
  metrics.py      #   longitud de arco vs distancia directa
  constraints.py  #   límites de operación y detección de incumplimientos
  local.py        #   aproximación lineal/cuadrática alrededor de un punto
  analysis.py     #   orquestador   ·   recommend.py  ranking y recomendación
  errors.py       #   errores de dominio (con código HTTP equivalente)
tests/test_core.py
```


## 🌐 Versión para GitHub Pages (estática, sin servidor)
`index.html` es una versión autónoma (JavaScript + Plotly.js desde CDN) con la misma matemática y las mismas 6 secciones.
GitHub Pages **no ejecuta Python**, por eso la versión Streamlit (`app.py`) se despliega aparte (ver abajo) y esta se publica así:
1. Sube todo el proyecto al repositorio (`index.html` debe quedar en la raíz).
2. En GitHub: **Settings → Pages → Build and deployment → Source: Deploy from a branch → Branch: `main` / `(root)` → Save**.
3. En ~1 minuto estará en `https://TU_USUARIO.github.io/NOMBRE_DEL_REPO/`.
Si en Pages ves el README en vez de la app, o un 404: `index.html` no quedó en la raíz del repo (suele pasar al subir la carpeta completa o el .zip sin descomprimir).
Hay una copia en `docs/index.html`: también sirve elegir **Branch: `main` / `/docs`**. La URL debe terminar en el nombre del repo (no en `/index.html` de otra carpeta).
Si aparece un aviso amarillo abajo, indica qué falló (p. ej. la red bloquea el CDN de Plotly).
Para probarla en local basta abrir `index.html` en el navegador (necesita internet para cargar Plotly.js).

## Ejecutar la versión Streamlit en local
```bash
python -m venv .venv && source .venv/bin/activate     # Windows: .venv\Scripts\activate
pip install -r requirements.txt
streamlit run app.py                                   # abre http://localhost:8501
python -m unittest discover -s tests                   # pruebas del núcleo (opcional)
```

## Publicar en GitHub y obtener un link (Streamlit Community Cloud, gratis)
1. Crea un repositorio **público** en github.com (p. ej. `trayectoria-espacial`).
2. Sube el proyecto:
   ```bash
   git init && git add . && git commit -m "Proyecto trayectoria espacial"
   git branch -M main
   git remote add origin https://github.com/TU_USUARIO/trayectoria-espacial.git
   git push -u origin main
   ```
3. Entra a https://share.streamlit.io, inicia sesión con GitHub → **Create app** → elige el repo, rama `main`,
   archivo principal `app.py` → **Deploy**.
4. En 1–3 min tendrás un link público tipo `https://TU-APP.streamlit.app`. Cada `git push` lo actualiza solo.

## Notas
- Los errores (a=b=c=0, límites ≤ 0, valores no finitos) los captura `core/errors.py`; la UI muestra un mensaje y bloquea el renderizado.
- Propuestas iniciales (editables): P1 `(2.2, 0, 0.1)` y P2 `(2.1, 0.02, 0.1)`; la original `(2, 0.5, 0.5)` incumple rapidez, aceleración y temperatura.
- Edita los integrantes en la sección 1 (o cambia el texto por defecto en `app.py`).
