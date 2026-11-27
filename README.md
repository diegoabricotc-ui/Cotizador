# Cotizador de la tienda

App web para cotizar en el mesón: productos, AppleCare+, Onsafe, trade in y cuotas sin interés por banco. Funciona en el celular y se publica gratis con GitHub Pages + Firebase.

## Archivos

- `index.html`: la app completa.
- `manifest.json`, `sw.js` y los 3 íconos `.png`: hacen que se instale en el iPhone como app (pantalla completa, ícono propio y abre sin señal).
- `firestore.rules`: reglas de seguridad de la base de datos (se pegan en Firebase, no se suben a GitHub).

## Probar sin Firebase (modo demo)

Abre `index.html` tal cual. Mientras `apiKey` diga `PEGAR_AQUI`, la app funciona en modo demo: no pide clave y guarda todo solo en ese navegador.

## Instalación (una sola vez)

### 1. Crear el proyecto en Firebase

1. Entra a https://console.firebase.google.com y crea un proyecto (por ejemplo `cotizador-tienda`). Analytics no es necesario.
2. **Authentication > Comenzar > Correo electrónico/contraseña** y actívalo.
3. En **Authentication > Usuarios**, crea 2 usuarios:
   - La cuenta de la tienda, por ejemplo `tienda@tu-dominio.cl`, con la clave que vas a compartir con los vendedores (mínimo 6 caracteres).
   - Tu cuenta de administrador, con tu correo y una contraseña que solo sepas tú.
4. **Firestore Database > Crear base de datos**, ubicación `southamerica-west1` (Santiago), en modo producción.
5. En **Firestore > Reglas**, pega el contenido de `firestore.rules`, cambia `tu-correo@gmail.com` por tu correo de admin y publica.
6. En **Configuración del proyecto (engranaje) > Tus apps > Web (</>)**, registra una app web. Firebase te muestra un bloque `firebaseConfig`.

### 2. Configurar la app

Abre `index.html` y busca el bloque `CONFIG` al principio del `<script>`:

```js
const CONFIG = {
  firebase: {
    apiKey: "...",        // copia de firebaseConfig
    authDomain: "...",
    projectId: "...",
    appId: "..."
  },
  tiendaEmail: "tienda@tu-dominio.cl",
  adminEmail: "tu-correo@gmail.com"
};
```

La `apiKey` de Firebase no es secreta: lo que protege los datos son las reglas del paso 1.5.

### 3. Publicar en GitHub Pages

1. Crea un repositorio nuevo en GitHub (puede ser privado si tienes GitHub Pro; si no, público).
2. Sube `index.html`, `manifest.json`, `sw.js` y los 3 íconos `.png`, todos en la raíz del repositorio.
3. **Settings > Pages > Branch: main / root > Save**.
4. En un minuto queda en `https://tu-usuario.github.io/nombre-repo/`.

### 4. Cargar los precios la primera vez

1. Entra a la app con **"Entrar como administrador"**.
2. Ve a la pestaña **Precios** y toca **"Cargar precios iniciales en la nube"**.
3. Listo: los vendedores entran con su nombre y la clave de la tienda.

## Uso diario

- **Vendedores:** cotizan, revisan cuotas y crean productos nuevos. Lo que uno crea queda para todos.
- **Administrador:** en **Precios** edita trade in, AppleCare, Onsafe, bancos, iPhone for Life y vigencia. Luego toca **Guardar en la nube** y les llega a todos al tiro. En **Productos** puede corregir precios o borrar.
- Para dejarla como app en el iPhone: abre el link en **Safari**, toca **Compartir > Agregar a inicio**. Queda con ícono propio y abre a pantalla completa, sin barra del navegador.
- Si cambias `index.html`, sube el número en `sw.js` (`cotizador-v1` a `cotizador-v2`) para que los celulares tomen la versión nueva.

## Cosas a tener en cuenta

- Si alguien se va de la tienda, cambia la clave de la cuenta de tienda en Firebase (Authentication > Usuarios) y avísales a todos.
- La app guarda los precios en caché, así que abre aunque falle el WiFi. Solo descarga cuando hay cambios.
- Plan gratis de Firestore: 50.000 lecturas y 20.000 escrituras al día. Con 9 vendedores se usa una fracción mínima.
