# Stripboard App

Versión escritorio (Windows) del Stripboard / Calendarizador de producción audiovisual.

Toma tu HTML existente y lo empaqueta como un `.exe` nativo usando **Tauri** (la misma tecnología que usamos en MediaStack, mucho más liviana que Electron). Resultado final: un `.exe` de aproximadamente **10-15 MB** que los estudiantes descargan y abren con doble click.

---

## Estructura del proyecto

```
stripboard-app/
├── dist/
│   └── index.html              ← Tu stripboard (esto es lo que abre la app)
├── src-tauri/
│   ├── src/
│   │   ├── lib.rs              ← Backend Rust mínimo
│   │   └── main.rs
│   ├── icons/                  ← Íconos de la app
│   ├── capabilities/
│   ├── Cargo.toml              ← Dependencias Rust
│   ├── build.rs
│   └── tauri.conf.json         ← Configuración de la ventana
├── .github/
│   └── workflows/
│       └── build.yml           ← Compilación automática en GitHub
└── README.md
```

---

## Cómo generar el `.exe` con GitHub Actions

Mismo flujo que usaste para MediaStack:

### 1. Crear repo nuevo en GitHub

Nombre sugerido: `stripboard-app`. Public. Marca "Add a README".

### 2. Subir los archivos

Tienes que subir **todo el contenido de este ZIP** preservando las carpetas. Las dos opciones:

**Opción A (recomendada): GitHub Desktop**
1. Instala [GitHub Desktop](https://desktop.github.com).
2. Clona tu repo vacío localmente.
3. Copia todos los archivos del ZIP dentro de la carpeta clonada.
4. GitHub Desktop detecta los cambios → escribe un mensaje → "Commit to main" → "Push origin".
5. Listo, todo subido.

**Opción B: web de GitHub (archivo por archivo)**
1. Click en "Add file" → "Create new file" → escribir `.github/workflows/build.yml` → pegar contenido del archivo.
2. Repetir para `src-tauri/Cargo.toml`, `src-tauri/build.rs`, `src-tauri/src/lib.rs`, `src-tauri/src/main.rs`, `src-tauri/tauri.conf.json`, `src-tauri/capabilities/default.json`.
3. Para los archivos binarios (íconos) y el HTML grande, usar "Upload files" después de crear las carpetas.

Es **mucho más fácil con GitHub Desktop**. Considéralo si vas a iterar con cambios.

### 3. Esperar la compilación

Apenas subas todo, GitHub detecta el workflow y empieza a compilar. **Tarda 5-10 minutos** la primera vez (después está cacheado y es más rápido).

Ve a la pestaña **Actions** del repo para ver el progreso.

### 4. Descargar el `.exe`

Cuando el workflow termine con ✓ verde:
1. Click en el workflow → baja hasta abajo.
2. Sección **Artifacts**:
   - **Stripboard-Windows** → `.exe` portátil (recomendado para distribuir)
   - **Stripboard-Installer** → instalador `.msi` (si los estudiantes prefieren instalación)
3. Descarga el ZIP, descomprime, distribuye.

---

## Cómo modificar el stripboard

**No tienes que recrear nada.** El flujo de iteración es:

1. Edita `dist/index.html` directamente en GitHub (click en el lápiz ✏️).
2. Commit del cambio.
3. GitHub regenera el `.exe` automáticamente (5-10 minutos).
4. Descargas el nuevo `.exe` desde Actions → Artifacts.
5. Distribuyes la nueva versión a los estudiantes.

---

## Tamaño esperado

- `.exe` portátil: **~10-15 MB** (mucho menos que MediaStack porque no incluye Python/PySide6/FFmpeg).
- Instalador `.msi`: **~3-5 MB** (es solo el wrapper que descarga y configura).

---

## Notas técnicas

- La app usa el **WebView2 de Microsoft** (preinstalado en Windows 10/11), por eso es tan liviano.
- Tu HTML mantiene **100% de funcionalidad**: localStorage funciona, exportar CSV/JSON funciona, Google Fonts se cargan al abrir.
- La primera vez que un estudiante abra la app necesita internet para cargar las Google Fonts. Después funciona offline.
- Para 100% offline, podríamos empotrar las fuentes directamente en el HTML — me dices si lo necesitas.

---

## Sobre antivirus

Mismo asunto que con MediaStack: el `.exe` no está firmado con certificado corporativo (~300 USD/año), así que:

- En máquinas personales: Windows mostrará una advertencia la primera vez, se acepta con "Ejecutar de todas formas".
- En máquinas UDD con Kaspersky: probablemente lo bloquee. Los estudiantes lo usan en sus PC personales.
