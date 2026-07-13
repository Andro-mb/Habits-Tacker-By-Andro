# Mi rutina — Habit tracker

Tracker de hábitos diseñado para TDAH: rutina matutina, rutina nocturna, hábitos diarios (lectura y deporte), sistema de puntos con canje de recompensas, historial y cierre semanal.

Funciona 100% en el navegador (sin backend, sin login). Los datos se guardan en `localStorage`, así que quedan solo en el navegador/dispositivo donde lo uses.

## Cómo subirlo a GitHub Pages

1. Entra a [github.com](https://github.com) y crea un repositorio nuevo (por ejemplo `mi-rutina`). Puede ser público.
2. Dentro del repo, sube este archivo `index.html` (botón "Add file" → "Upload files", arrástralo y haz commit).
3. Ve a **Settings** → **Pages** (en el menú lateral izquierdo).
4. En "Build and deployment", en **Source** selecciona **Deploy from a branch**.
5. En **Branch** selecciona `main` y la carpeta `/ (root)`, luego **Save**.
6. Espera 1-2 minutos. Tu página va a quedar disponible en:
   `https://TU-USUARIO.github.io/mi-rutina/`

(Reemplaza `TU-USUARIO` por tu usuario de GitHub, por ejemplo `andromb23`, y `mi-rutina` por el nombre que le pongas al repo.)

## Cómo agregarlo a tu iPhone

1. Abre el link de GitHub Pages en **Safari** (tiene que ser Safari, no Chrome).
2. Toca el ícono de compartir (el cuadrado con la flecha hacia arriba).
3. Selecciona **"Agregar a pantalla de inicio"**.
4. Listo — te va a quedar un ícono como app nativa. Al abrirlo no muestra la barra de Safari.

## Sincronización en la nube (Firebase)

Este `index.html` ya viene conectado a un proyecto de Firebase (Firestore + autenticación anónima). Cada vez que marcas algo, se guarda automáticamente en la nube; si abres la página desde otro dispositivo, va a cargar tus datos solo.

**Paso obligatorio antes de usarlo:** en la consola de Firebase, ve a **Firestore Database → Reglas** (pestaña "Rules") y reemplaza el contenido por esto:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /userData/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

Luego click en **Publicar**. Sin esto, Firestore bloquea todas las lecturas/escrituras por defecto y la app no va a poder guardar nada (verás "No se pudo guardar" en el mensajito de arriba).

También confirma que activaste:
- **Authentication → Sign-in method → Correo electrónico/contraseña** (activado — este es el que se usa ahora, ya no es modo anónimo)
- **Firestore Database** creado (región `southamerica-west1`, Santiago)

### Login

Ahora la app pide crear una cuenta con correo y contraseña la primera vez, y después iniciar sesión con eso mismo. Así tus datos quedan atados a tu cuenta, no al navegador — puedes entrar desde el iPhone, el compu, o donde sea, con el mismo correo, y vas a ver siempre el mismo historial.

### Cómo saber si está funcionando

Abajo del título "Mi rutina" vas a ver un mensajito chico: "Conectando a la nube..." → "☁️ Sincronizado". Si dice "Sin conexión a la nube" o "No se pudo guardar", revisa las reglas de arriba.

### Importante sobre dispositivos

La autenticación anónima crea un ID distinto por cada navegador/dispositivo. Esto significa que si abres la página desde Safari en tu iPhone y luego desde Chrome en tu compu, **son dos "usuarios" distintos** para Firebase — no van a compartir datos automáticamente entre sí. Funciona perfecto para "mismo navegador, distintas veces" (por ejemplo si reinstalas la app en el mismo iPhone), pero si quieres que el mismo historial aparezca en varios dispositivos a la vez, hay que agregar un login real (con email, por ejemplo) en vez de anónimo — avísame si en algún momento quieres eso.

## Notas

- Si borras datos de navegación de Safari o cambias de iPhone, se pierde el historial (vive en `localStorage`, no en la nube).
- Puedes editar directamente el archivo `index.html` en GitHub (botón del lápiz) para cambiar hábitos, puntos o recompensas sin necesitar nada más instalado.
