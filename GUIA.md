# Hit Parade en Vivo

Votación en vivo de canciones:
- Cada quien escribe su nombre y puede agregar canciones.
- Cada canción se califica del 1 al 5 y el ranking se ordena por el promedio de todos.
- Nadie tiene que registrarse.

La página está en `public/index.html` y las reglas de la base de datos en `database.rules.json`. Cada cambio que se sube a la rama `main` se publica solo en Firebase Hosting mediante GitHub Actions (`.github/workflows/deploy.yml`).

Todo lo de abajo se hace desde el navegador, sin instalar nada. Usa tu cuenta personal de Google y la de GitHub.

---

## Parte 1: crear el proyecto de Firebase (unos 5 minutos)

1. Entra a https://console.firebase.google.com y pulsa **Crear un proyecto**.
   - Ponle un nombre, por ejemplo `hit-parade`.
   - Google Analytics no hace falta.
   - Anota el **ID del proyecto**: aparece debajo del nombre, algo como `hit-parade-1a2b3`.
2. **Activa el acceso anónimo.**
   - En el menú: **Compilación → Authentication → Comenzar**.
   - Pestaña **Método de acceso** → **Anónimo** → **Habilitar** → **Guardar**.
3. **Crea la base de datos.**
   - En el menú: **Compilación → Realtime Database → Crear base de datos**.
   - Elige la ubicación **Estados Unidos (us-central1)**.
   - Elige **modo bloqueado**. Las reglas buenas se suben solas desde el repo.
4. **Registra la app web.**
   - ⚙️ **Configuración del proyecto** → **Tus apps** → ícono **`</>`**.
   - Ponle un nombre y regístrala. No hace falta copiar nada: la página lee su configuración sola cuando está publicada en Firebase.

## Parte 2: el repositorio de GitHub

1. En https://github.com/new crea un repositorio, por ejemplo `hit-parade`.
   - Puede ser privado o público.
   - Déjalo **vacío**: sin README, sin .gitignore y sin licencia.
2. Conecta tu GitHub a Claude en https://claude.ai/connect-github y dale acceso a ese repositorio.
3. Pásale a Claude el nombre en la forma `usuario/hit-parade` para que suba el código.

## Parte 3: darle permiso a GitHub para publicar en Firebase

Esta llave **no se pega en el chat**. Va directo en GitHub como secreto.

1. Entra a https://console.cloud.google.com/iam-admin/serviceaccounts y elige tu proyecto de Firebase arriba.
2. **Crear cuenta de servicio**.
   - Nombre: `github-deploy` → **Crear y continuar**.
   - Rol: busca **Administrador de Firebase** (Firebase Admin) → **Continuar** → **Listo**.
3. Entra a la cuenta `github-deploy`.
   - Pestaña **Claves** → **Agregar clave → Crear clave nueva → JSON**.
   - Se descarga un archivo `.json`.
4. En tu repositorio de GitHub: **Settings → Secrets and variables → Actions**.
   - Pestaña **Secrets** → **New repository secret**: nombre `FIREBASE_SERVICE_ACCOUNT`, y como valor el contenido completo del `.json`.
   - Pestaña **Variables** → **New repository variable**: nombre `FIREBASE_PROJECT_ID`, y como valor el ID del proyecto de la parte 1.
5. Borra el `.json` descargado de tu computadora: ya no lo necesitas.

## Parte 4: publicar

1. En el repositorio entra a **Actions → Publicar en Firebase → Run workflow**. Después se publica solo con cada cambio que se sube a `main`.
2. Cuando termine en verde, la página queda en **`https://ID-DEL-PROYECTO.web.app`**. Ese es el link que compartes.

Si el paso falla, abre el registro en Actions. Los errores más comunes:
- Falta el secreto o la variable de la parte 3.
- La cuenta de servicio no tiene el rol de Administrador de Firebase.

---

## Cómo se usa

- Al entrar, cada persona escribe su nombre. Se recuerda mientras tenga la pestaña abierta y se puede cambiar con "cambiar".
- Cualquiera agrega canciones. Solo quien agregó una canción la puede quitar.
- Cada persona califica del 1 al 5 y puede cambiar su calificación. Si toca otra vez la misma estrella, quita su voto.
- El ranking se ordena por el promedio. Si hay empate, gana la que tiene más votos.
- "Ver quién votó" muestra el nombre y las estrellas de cada persona.

## Límites a saber

- No hay cuentas, así que alguien podría votar dos veces si usa otro navegador o una ventana de incógnito.
- El plan gratis de Firebase (Spark) alcanza: aguanta 100 personas conectadas a la vez.
- **Para empezar de cero:** en la consola de Firebase, Realtime Database → **Datos** → pasa el mouse sobre la raíz y pulsa la ✕.
