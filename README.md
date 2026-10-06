# ConectaWeb — Telegram Bot API

## Archivos
- `public/index.html`: página y formulario.
- `public/style.css`: diseño adaptable.
- `public/script.js`: validación y petición POST al backend.
- `server.js`: servidor Node.js/Express; conserva el token en el servidor.
- `.env.example`: plantilla de variables privadas.
- `INFORME_Y_PRESENTACION.md`: guía completa de la actividad.

## Requisitos
Instala Node.js LTS. Dentro de esta carpeta, ejecuta:

```bash
npm install
```

Copia `.env.example` a un archivo llamado `.env` y reemplaza el valor por el token que te entregó BotFather. No compartas ese archivo ni lo subas a GitHub.

Inicia el servidor:

```bash
npm start
```

Abre `http://localhost:3000`.

## Configuración de Telegram
1. En Telegram busca `@BotFather`.
2. Envía `/newbot`, sigue los pasos y guarda el token en `.env`.
3. Busca tu bot mediante el enlace que entrega BotFather y pulsa Start/Iniciar.
4. Para obtener tu Chat ID, visita en el navegador `https://api.telegram.org/bot<TU_TOKEN>/getUpdates` después de enviarle un mensaje al bot. Reemplaza `<TU_TOKEN>` localmente por tu token; no compartas esa URL ni una captura que lo muestre. Busca `message.chat.id` en la respuesta JSON.
5. Introduce ese Chat ID en el formulario y envía un mensaje.

El bot no puede iniciar una conversación privada con una persona que nunca lo haya iniciado. Para grupos, añade el bot y comprueba que tenga permisos adecuados.

## Publicación
GitHub Pages solo aloja archivos estáticos; no ejecuta `server.js`. Para que el envío real funcione públicamente, despliega el backend Node.js en un servicio compatible con aplicaciones de servidor y configura allí `TELEGRAM_BOT_TOKEN` como variable secreta. Publica el frontend y configura la ruta de API con la URL segura de tu backend (o sirve frontend y backend desde el mismo host). No expongas el token en el frontend.

## Seguridad
Rota el token desde BotFather si se filtra. No subas `.env` ni credenciales. Añade autenticación, límites de uso y protección contra abuso antes de publicar el backend para el público.
