# Ren-employment

Frontend en React desplegado en GitHub Pages.

**URL del sitio:** https://inando4.github.io/Ren-employment/

## Cómo funciona el despliegue

Cada push a `main` dispara [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml),
que compila el proyecto y publica el resultado. No hay que ejecutar nada a mano.

El workflow se adapta solo:

| Detecta | Cómo |
|---|---|
| Dónde está el proyecto | Raíz del repo o carpeta `frontend/` |
| Vite o CRA | Por la presencia de `vite.config.*` |
| Carpeta de salida | Prueba `dist`, `build`, `out` |
| Ruta base `/Ren-employment/` | Se inyecta al construir, no hace falta tocar la config |

## Lo único que debes configurar en el código

El workflow inyecta la ruta base en el build, pero **no puede tocar tu router**:
eso es código fuente. Si usas React Router, la app debe montarse con el prefijo
del repositorio o la navegación quedará en blanco al cambiar de ruta.

Con Vite:

```jsx
import { BrowserRouter } from 'react-router-dom'

<BrowserRouter basename={import.meta.env.BASE_URL}>
  <App />
</BrowserRouter>
```

Con Create React App:

```jsx
<BrowserRouter basename={process.env.PUBLIC_URL}>
  <App />
</BrowserRouter>
```

Ambas formas funcionan igual en local (`npm run dev`) y en producción,
porque la variable vale `/` en desarrollo y `/Ren-employment/` en el build.

## Desarrollo local

```bash
npm install
npm run dev
```

## Notas

- Pages sirve **solo el frontend**. El backend necesita hosting propio y debe
  permitir CORS desde `https://inando4.github.io`.
- No pongas claves ni secretos en el código del frontend: todo lo que se
  compila queda visible para cualquiera que abra el sitio.
