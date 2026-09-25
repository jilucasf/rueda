# Puesta en marcha de Rueda (Firebase + GitHub Pages)

Todo es gratuito para el volumen de Rueda (plan Spark de Firebase). Tiempo aproximado: 15 minutos.

## 1. Crear el proyecto de Firebase
1. Entra en https://console.firebase.google.com con tu cuenta de Google.
2. **Crear un proyecto** → nombre: `rueda-va-sg` → puedes desactivar Google Analytics → Crear.

## 2. Registrar la app web y copiar la configuración
1. En la portada del proyecto, pulsa el icono **`</>` (Web)**.
2. Apodo: `Rueda` → **Registrar app** (no marques Firebase Hosting).
3. Copia el bloque `firebaseConfig` (apiKey, authDomain, projectId…) y pégalo en `firebase-config.js`, sustituyendo los `PEGAR_AQUI`.
   - Estos valores no son secretos: son públicos en cualquier web con Firebase. La seguridad la dan las reglas del paso 4.

## 3. Activar el inicio de sesión
Menú **Compilación → Authentication → Comenzar → Método de inicio de sesión**:
1. **Google** → Habilitar → elige tu correo de asistencia → Guardar.
2. **Correo electrónico/contraseña** → Habilitar **solo** «Vínculo de correo electrónico (acceso sin contraseña)» → Guardar.
3. Pestaña **Configuración → Dominios autorizados → Agregar dominio**: `jilucasf.github.io`

## 4. Crear la base de datos y sus reglas
1. **Compilación → Firestore Database → Crear base de datos**.
2. Ubicación: `eur3 (europe-west)` → modo **producción** → Crear.
3. Pestaña **Reglas**: borra lo que haya, pega el contenido de `firestore.rules` → **Publicar**.

## 5. Poner el código de invitación
1. **Firestore Database → Datos → Iniciar colección** → ID de la colección: `privado`.
2. ID del documento: `acceso` → campo `codigo` (tipo string) → valor en MAYÚSCULAS y sin espacios, por ejemplo `SEGOVIA2026` → Guardar.
3. Pasa ese código solo a las personas del grupo (por WhatsApp, en persona…).
- **Si el código se filtra**, cambia el valor del campo: desde ese momento el antiguo deja de valer para nuevas altas. Quien ya estaba dentro sigue dentro.
- Nadie puede leer este código desde la app: solo lo comprueban las reglas en el servidor de Google.

## 6. Hacerte administrador (opcional, para poder «Quitar» a alguien)
1. Entra una vez en Rueda para que exista tu usuario.
2. **Authentication → Usuarios**: copia tu «UID de usuario».
3. **Firestore Database → Iniciar colección** → ID `admins` → ID del documento: tu UID → añade un campo `nombre` = `Nacho` → Guardar.

## 7. Tu contacto en el aviso de privacidad
En `firebase-config.js`, rellena `window.RUEDA_RESPONSABLE` con tu nombre y un correo de contacto
(mejor uno creado para Rueda que el personal), por ejemplo `"Nacho — rueda.vasg@gmail.com"`.

## 8. Publicar en GitHub Pages
Con git desbloqueado (`sudo xcodebuild -license accept`), desde la carpeta `rueda`:
```
git init && git add . && git commit -m "Rueda Valladolid Segovia"
gh repo create rueda --public --source . --push
gh api -X POST repos/jilucasf/rueda/pages -f "source[branch]=main" -f "source[path]=/"
```
La app quedará en **https://jilucasf.github.io/rueda/** (tarda 1–2 minutos la primera vez).

## Cómo funciona el acceso
- **Google**: un toque y dentro.
- **Enlace mágico**: la persona escribe cualquier correo (Gmail, Yahoo, Educacyl…), recibe un enlace y al abrirlo entra sin contraseña. Si lo abre en otro dispositivo, la app le pide confirmar el correo.
- La sesión queda guardada en el móvil: las siguientes veces entra directamente y ve su horario y sus coincidencias.
- La primera vez, además, escribe el **código de invitación** y acepta el **aviso de privacidad**. Sin código se puede entrar, pero no se ve la lista.
- **Darme de baja** borra su horario, su alta en el grupo y su cuenta de acceso.
