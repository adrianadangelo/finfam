# FinFam: cómo publicarla en GitHub Pages con Firebase

FinFam queda en dos lugares:

- **GitHub Pages** guarda la app (los archivos de esta carpeta).
- **Firebase** (de Google, gratis) guarda los datos y solo deja entrar a los teléfonos que tengan el código familiar.

Tiempo estimado: unos 30 minutos, una sola vez.

## 1. Crear el proyecto en Firebase

1. Entra a https://console.firebase.google.com con tu cuenta de Google y toca **Crear un proyecto**. Llámalo `finfam`. Google Analytics no hace falta.
2. En la página del proyecto, toca el ícono **</>** (Web) para registrar una app web. Ponle de nombre `FinFam`. No marques Firebase Hosting.
3. Firebase te muestra un bloque `firebaseConfig`. Copia esos valores en el archivo **config.js** de esta carpeta, reemplazando cada `PEGA_AQUI`.

## 2. Activar el acceso sin cuentas

Nadie de la familia necesita crear una cuenta. Cada teléfono entra **una sola vez** con un **código familiar** que tú defines.

1. En el menú, ve a **Compilación > Authentication > Comenzar**.
2. En **Método de acceso**, activa **Anónimo**. Así cada teléfono recibe una identificación interna, sin correo ni contraseña.
3. En **Configuración > Dominios autorizados**, agrega `TU-USUARIO.github.io` (lo tendrás en el paso 4).

## 3. Crear la base de datos, el código familiar y las reglas

1. Ve a **Compilación > Firestore Database > Crear base de datos**.
2. Elige **modo de producción** y una ubicación cercana, por ejemplo `southamerica-east1` (São Paulo).
3. **Crea el código familiar:** en la pestaña **Datos**, toca **Iniciar colección** y escribe:
   - ID de la colección: `config`
   - ID del documento: `acceso`
   - Campo: `codigo`, tipo **string**, y como valor tu código.
4. Abre la pestaña **Reglas**, borra lo que haya, pega el contenido del archivo **firestore.rules** y toca **Publicar**.

**Cómo elegir un buen código:** que sea largo y fácil de recordar, por ejemplo cuatro palabras y un número: `cochinito-mango-playa-2026`. Evita fechas de cumpleaños o nombres solos: un código corto se puede adivinar.

Solo los teléfonos que escriban ese código podrán ver o cambiar datos. Cualquier otra persona, aunque encuentre la página, no verá nada.

## 4. Publicar la app en GitHub Pages

1. En https://github.com crea un repositorio nuevo, por ejemplo `finfam`.
2. Sube **todos los archivos de esta carpeta**: `index.html`, `config.js`, `manifest.json`, `icon-192.png`, `icon-512.png` (este LEEME y `firestore.rules` son opcionales).
3. Ve a **Settings > Pages**. En "Branch" elige `main` y la carpeta `/ (root)`, y toca **Save**.
4. En uno o dos minutos la app queda en `https://TU-USUARIO.github.io/finfam/`.

**Nunca subas a GitHub el archivo de respaldo con los datos.** El repositorio es público: el código puede verse, pero los datos viven protegidos en Firebase.

## 5. Cargar los datos que ya tenías

1. Abre la app, escribe el código familiar y toca el **escudo** de arriba.
2. En **Respaldo**, toca **Cargar respaldo** y elige el archivo `finfam-respaldo-2026-10-17.json`, que te entregué aparte.
3. Se cargan tu plan, los planes de mesada de Vicky y Sebas, la tasa BCV y las imágenes de perfil.

## 6. Instalarla en los teléfonos

- **iPhone (Safari):** abre la página, toca **Compartir** y luego **Agregar a pantalla de inicio**.
- **Android (Chrome):** abre la página, toca **⋮** y luego **Instalar app** o **Agregar a la pantalla principal**.

Cada persona escribe el código familiar una vez, elige quién es en "Finanzas Familia" y crea su PIN en el escudo.

## Mantenimiento

- **Respaldo:** desde el escudo, descarga uno cada mes y guárdalo en un lugar privado.
- **Si el código se filtra o pierdes un teléfono:** cambia el valor de `config/acceso > codigo` y borra la colección `miembros` en Firestore. Todos los teléfonos quedarán desconectados y la familia vuelve a entrar con el código nuevo. Tus datos no se pierden.
- **Costo:** el plan gratuito de Firebase (Spark) alcanza de sobra para una familia.
