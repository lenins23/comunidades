# **Sistema de Control de Comunidades**

Este proyecto es una aplicación web React para llevar el control de asistencia y puntajes de cinco comunidades. Cuenta con un sistema de autenticación por clave de acceso para usuarios y administradores, y guarda todos los datos en una base de datos de Firebase.

### **Estructura del Proyecto**

El proyecto está organizado de la siguiente manera:

/  
├── public/  
│   └── index.html               \# Archivo HTML base.  
├── src/  
│   ├── components/  
│   │   └── App.jsx              \# Componente principal de la aplicación.  
│   ├── firebase.js              \# Archivo de configuración de Firebase.  
│   └── index.js                 \# Punto de entrada de la aplicación React.  
└── package.json                 \# Lista de dependencias y scripts del proyecto.

### **Requisitos**

Necesitas tener **Node.js** y **npm** (gestor de paquetes de Node.js) instalados en tu computadora.

### **Instalación**

Sigue estos pasos para configurar el proyecto en tu máquina local:

1. **Crea el proyecto:** Crea una carpeta principal y, dentro de ella, crea las subcarpetas src y public.  
2. **Copia los archivos:** Copia el contenido de cada bloque de código que se te proporcionó en sus respectivos archivos (index.html, index.js, App.jsx, firebase.js y package.json).  
3. **Instala las dependencias:** Abre una terminal en la carpeta principal del proyecto y ejecuta el siguiente comando:  
   npm install

4. **Inicia la aplicación:** Para ejecutar la aplicación en modo de desarrollo, usa este comando:  
   npm start

   La aplicación se abrirá en tu navegador en http://localhost:3000.

### **Configuración de Firebase**

Este paso es **crucial** para que la aplicación funcione, ya que se conecta a tu propia base de datos de Firebase.

1. **Crea un proyecto de Firebase:**  
   * Ve a la [consola de Firebase](https://console.firebase.google.com/).  
   * Crea un nuevo proyecto.  
2. **Configura Authentication y Firestore:**  
   * En tu proyecto de Firebase, ve a **Authentication** y habilita el método de inicio de sesión **Anónimo**.  
   * Ve a **Firestore Database**, crea una base de datos en modo de prueba y luego crea estas colecciones:  
     * user\_keys: Para guardar las claves de acceso de los usuarios y administradores.  
     * members: Para guardar la información de los miembros de las comunidades.  
     * challenges: Para los retos grupales.  
     * users: Para guardar los datos de los usuarios autenticados.  
3. **Actualiza el archivo de configuración:**  
   * En la consola de Firebase, ve a **Configuración del proyecto** \> **Tus apps**.  
   * Copia el objeto de configuración (API key, auth domain, etc.).  
   * Abre el archivo src/firebase.js y reemplaza el objeto firebaseConfig con tu propia configuración.  
4. **Actualiza las reglas de seguridad de Firestore:**  
   * En la consola de Firebase, ve a **Firestore Database** \> **Reglas**.  
   * Copia y pega las siguientes reglas para asegurar el acceso:

rules\_version \= '2';  
service cloud.firestore {  
  match /databases/{database}/documents {  
    match /artifacts/{appId} {  
      // Public data  
      match /public/data/{collection}/{document} {  
        allow read: if request.auth \!= null;  
        allow write: if request.auth \!= null && get(/databases/$(database)/documents/artifacts/$(appId)/users/$(request.auth.uid)).data.role \== 'admin';  
      }  
      // Private user data  
      match /users/{userId}/{collection}/{document} {  
        allow read, write: if request.auth \!= null && request.auth.uid \== userId;  
      }  
    }  
  }  
}

5. **Carga las claves iniciales:**  
   * En la consola de Firebase, ve a **Firestore Database** \> **Datos**.  
   * Ve a la colección user\_keys y añade las claves iniciales que desees, por ejemplo, la del administrador (HNBP-23-97) y las de los usuarios.

### **Comandos Disponibles**

* npm start: Inicia el servidor de desarrollo.  
* npm run build: Compila la aplicación para producción. Los archivos generados estarán en la carpeta build/.

### **Despliegue en un Hosting**

Para subir la aplicación a un hosting, puedes usar servicios como Firebase Hosting, Netlify o Vercel. Simplemente ejecuta npm run build y sube el contenido de la carpeta build/ a tu servicio de hosting.
