# INFORME, PROCEDIMIENTO Y EXPOSICIÓN
## Actividad práctica: “Mi página web envía mensajes a Telegram”

**Módulo:** Diseño y Desarrollo Web  
**Nivel:** Bachillerato Técnico en Informática  
**Herramientas:** HTML, CSS, JavaScript, Node.js, Telegram Bot API y GitHub  
**Autora:** Erika Flores 💙

# 1. Objetivo general
Desarrollar una página web que permita enviar mensajes mediante un bot de Telegram, integrando HTML, CSS y JavaScript con una API externa y protegiendo el token mediante un servidor backend.

# 2. Objetivos específicos
- Crear un bot de Telegram con BotFather.
- Obtener y proteger el token y conocer el Chat ID.
- Diseñar un formulario web con campos y validaciones.
- Enviar una petición POST al backend y desde este a Telegram Bot API.
- Manejar respuestas JSON y errores.
- Publicar el frontend y documentar la configuración del servidor.

# 3. Investigación previa

## 3.1 ¿Cómo se crea un bot y qué rol cumple BotFather?
En Telegram se busca `@BotFather`, se envía `/newbot`, se indica un nombre y un nombre de usuario disponible. BotFather crea el bot y entrega un token de autenticación. El token permite que el backend se identifique ante Telegram Bot API.

## 3.2 ¿Qué es un token y por qué debe protegerse?
Es una credencial secreta que autoriza las llamadas a la API del bot. Quien lo tenga puede controlar el bot dentro de los permisos de la API. Debe guardarse en una variable de entorno del servidor, no en HTML, JavaScript del navegador, repositorios públicos ni capturas.

## 3.3 ¿Cómo se obtiene el Chat ID?
Primero, el usuario abre el bot y pulsa Start/Iniciar y le envía un mensaje. Luego se consulta `getUpdates` con el token privado. En la respuesta JSON se busca `message.chat.id`. El Chat ID identifica el chat al que se enviará el mensaje. Nunca se debe compartir la URL de consulta porque contiene el token.

## 3.4 ¿Cuál es el endpoint de Telegram para enviar mensajes?
El método es `sendMessage`. Su endpoint tiene esta estructura:

`https://api.telegram.org/bot<TOKEN>/sendMessage`

Se envía una solicitud HTTP POST con JSON, por ejemplo:

```json
{
  "chat_id": "CHAT_ID",
  "text": "Hola, este es un mensaje de prueba."
}
```

El token real se mantiene exclusivamente en el backend.

## 3.5 ¿Cómo funciona una petición POST con fetch?
El frontend utiliza `fetch()` con el método POST, el encabezado `Content-Type: application/json` y un cuerpo JSON. La solicitud se dirige al backend propio, no directamente a Telegram, para que el token no quede expuesto.

```javascript
fetch("/api/send-message", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ chatId: "CHAT_ID", message: "Hola" })
});
```

## 3.6 ¿Cómo se manejan errores y respuestas JSON?
El backend revisa los datos recibidos, realiza la solicitud a Telegram y comprueba el estado HTTP y el campo `ok` de la respuesta JSON. El frontend utiliza `try/catch` y muestra el resultado o un mensaje de error.

## 3.7 ¿Cómo publicar una página en GitHub Pages?
GitHub Pages publica el frontend estático desde un repositorio. Se sube la carpeta `public` o su contenido, se configura Settings → Pages y se selecciona la rama y carpeta. Sin embargo, GitHub Pages no ejecuta Node.js. Para el envío real, el backend debe desplegarse en un servicio compatible con Node.js y configurarse la variable secreta `TELEGRAM_BOT_TOKEN`.

## 3.8 ¿Qué riesgos existen al colocar el token en el frontend?
El código del navegador puede ser inspeccionado. Si el token se publica, otra persona puede controlar el bot o abusar de sus permisos. Se debe usar un backend, variables de entorno, límites de uso y protección contra solicitudes no autorizadas.

# 4. Procedimiento de desarrollo paso a paso

## Fase 1. Crear el bot
1. Abrir Telegram y buscar `@BotFather`.
2. Enviar `/newbot`.
3. Escribir el nombre del bot.
4. Elegir un nombre de usuario terminado en `bot`.
5. Guardar el token de forma privada.
6. Abrir el bot creado y pulsar Start/Iniciar.

## Fase 2. Preparar el proyecto
7. Descomprimir la carpeta del proyecto.
8. Instalar Node.js LTS.
9. Abrir una terminal en la carpeta y ejecutar `npm install`.
10. Copiar `.env.example` como `.env`.
11. Colocar el token en `TELEGRAM_BOT_TOKEN` dentro de `.env`; no compartir ni subir este archivo.

## Fase 3. Obtener el Chat ID
12. Enviar un mensaje al bot desde Telegram.
13. Consultar el método `getUpdates` con el token en privado.
14. Localizar `message.chat.id` en el JSON.
15. Copiar el ID numérico para la prueba.

## Fase 4. Ejecutar y probar localmente
16. Ejecutar `npm start`.
17. Abrir `http://localhost:3000`.
18. Introducir el Chat ID, escribir el mensaje y marcar la autorización.
19. Pulsar “Enviar mensaje por Telegram”.
20. Verificar el aviso de éxito en la página y revisar el mensaje recibido en Telegram.

## Fase 5. Pruebas locales y evidencias

| Prueba | Acción | Resultado esperado |
|---|---|---|
| 1 | Abrir la página | Se visualizan las secciones y el formulario. |
| 2 | Enviar vacío | Se muestran validaciones. |
| 3 | Escribir Chat ID con letras | Se limpian caracteres no permitidos o aparece aviso. |
| 4 | No marcar autorización | Se solicita confirmación. |
| 5 | Enviar con servidor apagado | Se muestra un error de conexión. |
| 6 | Enviar con token incorrecto | Se muestra el error devuelto por Telegram. |
| 7 | Enviar con datos correctos | Telegram recibe el mensaje y la página muestra éxito. |
| 8 | Abrir en celular | La página se adapta al ancho de pantalla. |

**Evidencias que se deben capturar realmente:** conversación con BotFather (sin mostrar token), bot iniciado, Chat ID con datos privados ocultos, página web, validación de errores, mensaje recibido en Telegram y página publicada. No incluir el token en capturas ni en el informe.

# 5. Publicación

1. Subir el frontend a un repositorio GitHub.
2. Activar GitHub Pages desde Settings → Pages.
3. Desplegar `server.js` en un alojamiento que ejecute Node.js.
4. Configurar `TELEGRAM_BOT_TOKEN` como variable secreta en el alojamiento del backend.
5. Configurar el frontend para llamar a la URL del backend si no se sirven desde el mismo dominio.
6. Abrir la URL pública y repetir la prueba de envío.

**Nota:** GitHub Pages por sí solo no ejecuta el backend. Una página publicada en Pages no enviará mensajes reales hasta que exista un backend activo y correctamente configurado.

# 6. Documentación y exposición

## Resumen técnico
HTML construye el formulario y las secciones; CSS define el diseño adaptable; JavaScript valida el Chat ID, el texto y la autorización, y realiza una petición POST al backend. Node.js recibe la solicitud y llama al método `sendMessage` de Telegram Bot API usando un token almacenado en una variable de entorno. El servidor devuelve JSON y el frontend muestra el resultado.

## Conclusión
La actividad permite aplicar conocimientos de desarrollo web, validación de formularios, peticiones HTTP, manejo de JSON y publicación de aplicaciones. También demuestra la importancia de separar frontend y backend para proteger credenciales. El envío real requiere que el bot esté creado, que el usuario haya iniciado la conversación y que el servidor se encuentre configurado y activo.

## Guion para exposición
Buenos días. Mi proyecto se llama “Mi página web envía mensajes a Telegram”. Su objetivo es enviar un mensaje a través de un bot de Telegram desde un formulario web.

Primero creé el bot con BotFather y guardé su token de manera privada. Después inicié el bot y obtuve el Chat ID. Diseñé la página con HTML y CSS, y utilicé JavaScript para validar el formulario y enviar una petición POST al backend.

El servidor está desarrollado con Node.js y se comunica con el método sendMessage de Telegram Bot API. El token se mantiene en una variable de entorno y no se publica en el navegador. La respuesta de Telegram se procesa como JSON para mostrar si el envío fue correcto o si ocurrió un error.

Finalmente, realicé pruebas locales y preparé la publicación. GitHub Pages sirve los archivos estáticos, mientras que el backend debe ejecutarse en un alojamiento compatible con Node.js. Con este proyecto aprendí a integrar una página web con una API y a proteger datos de acceso. Muchas gracias.
