# Configuración de Firebase para UCAL PRO

UCAL PRO usa **Firebase Authentication (Google)** y **Cloud Firestore**. Las plantillas siguen guardándose primero en el navegador y, al conectar una cuenta, se migran/sincronizan con la nube.

## 1. Registrar la aplicación web

1. Abre [Firebase Console](https://console.firebase.google.com/) y crea un proyecto para UCAL PRO o selecciona uno que administres.
2. En la configuración del proyecto, agrega una aplicación web (icono **</>**).
3. En **Configuración del proyecto → General → Tus aplicaciones → SDK de Firebase**, copia el objeto `firebaseConfig`.

En `index.html`, busca:

```js
const firebaseConfig = {
  apiKey: "REEMPLAZAR_API_KEY",
  authDomain: "REEMPLAZAR_PROJECT_ID.firebaseapp.com",
  projectId: "REEMPLAZAR_PROJECT_ID",
  appId: "REEMPLAZAR_APP_ID"
};
```

Reemplaza esos cuatro valores por los valores reales de tu app web. Si Firebase entrega también `messagingSenderId` o `storageBucket`, puedes agregarlos al mismo objeto.

> La configuración web identifica el proyecto y se utiliza desde el navegador. No pegues aquí una clave privada, un archivo JSON de cuenta de servicio ni credenciales de administrador.

## 2. Activar el inicio de sesión con Google

1. En Firebase Console, abre **Authentication → Comenzar**.
2. En **Sign-in method / Método de acceso**, habilita **Google**.
3. En **Authentication → Settings → Authorized domains**, añade el dominio donde publicarás UCAL PRO. Para GitHub Pages del usuario SOMERITO normalmente será `somerito.github.io` (sin la ruta del repositorio). Si utilizas un dominio propio, añádelo también.

El botón **Conectar nube** inicia y cierra la sesión de Google.

## 3. Crear Cloud Firestore

1. Abre **Firestore Database → Crear base de datos**.
2. Crea la base de datos en modo de producción.
3. Selecciona la región que corresponda a tu proyecto.

## 4. Publicar las reglas de seguridad

En **Firestore Database → Rules**, reemplaza las reglas iniciales por el contenido del archivo [firestore.rules](./firestore.rules) y pulsa **Publicar**.

Las reglas permiten que una cuenta autenticada lea y actualice únicamente su propio documento de UCAL PRO. No habilitan lectura pública ni acceso anónimo.

## 5. Probar la sincronización

1. Publica los cambios de GitHub Pages y recarga la página.
2. Pulsa **Conectar nube** e inicia sesión con Google.
3. La primera conexión importa las plantillas que ya estén guardadas localmente, siempre que ese navegador todavía no esté asociado a otra cuenta.
4. En adelante, las altas, ediciones, eliminaciones y el reordenamiento se sincronizan con Firestore. Los otros dispositivos conectados a la misma cuenta reciben los cambios en tiempo real.
5. El botón de respaldo local sigue disponible como copia adicional.

## Dónde se guardan los datos

Cada cuenta utiliza esta ruta privada:

```
users/{uid}/workspaces/ucalTemplates
```

El documento contiene la lista ordenada de plantillas y su fecha de actualización.

## Costos y cuotas

Firestore tiene cuotas sin costo en el plan Spark, sujetas a los límites vigentes de Firebase. Consulta el uso del proyecto y configura alertas de presupuesto en Google Cloud. No cambies a Blaze sin revisar primero las condiciones de facturación.

- [Documentación: configurar Firebase para web](https://firebase.google.com/docs/web/setup)
- [Documentación: crear y guardar datos en Cloud Firestore](https://firebase.google.com/docs/firestore/manage-data/add-data)
- [Documentación: reglas de seguridad de Firestore](https://firebase.google.com/docs/firestore/security/get-started)
