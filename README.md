# 🎟️ Advanced Ticket System By TwisSpark

<div align="center">
  <img src="https://i.imgur.com/4OQ1o6T.png"width="500">
</div> 

Un sistema de tickets avanzado, fácil de usar y completamente configurable para personas que no quieren perder tiempo configurando sistemas complicados o llenos de código innecesario.

El sistema está listo para usar desde el inicio, pero también ofrece muchas opciones de personalización para usuarios con experiencia en **BDFD / BDScript** y **JSON**.

---

# ✨ Características

- ✅ Sistema listo para usar
- ✅ Fácil de configurar
- ✅ Compatible con BDFD / BDScript
- ✅ Configuración mediante JSON
- ✅ Hasta 10 opciones de tickets
- ✅ Roles personalizados para hacer ping
- ✅ Categorías personalizadas para tickets
- ✅ Sistema de logs configurable
- ✅ Soporte para múltiples canales de logs

---

# 📂 Opciones de Tickets

El sistema incluye **5 opciones configuradas por defecto**, pero fue desarrollado con soporte de hasta **10 opciones**.

Puedes:
- Agregar nuevas opciones
- Editar opciones existentes
- Personalizar nombres, emojis y configuraciones

Para hacer cambios avanzados es recomendable tener conocimientos básicos de **JSON**.

Si necesitas ayuda puedes entrar al servidor de soporte de **Sparkify**.

---

# ⚙️ Personalización

Puedes configurar:

- 📌 Roles para hacer ping al abrir un ticket
- 📁 Categorías específicas para cada tipo de ticket
- 🗂️ Una sola categoría para todos los tickets
- 📝 Un canal de logs general
- 📑 Diferentes canales de logs para cada opción

Todo el sistema es flexible y puedes adaptarlo fácilmente a tu servidor.

---

# ⚠️ Importante

No elimines ni modifiques esta parte del sistema a menos que sepas exactamente lo que haces.

Este bloque es necesario para que el sistema de tickets funcione correctamente.  
Si eliminas alguna línea o cambias la estructura, el sistema puede romperse o dejar de funcionar.

```js
$var[category;La id de la categoría]
$var[rol_ping;La id del rol]
$var[channel_id_logs;La id del canal de los logs]

$jsonParse[
{
  "abrir_ticket": [
   {
    "name": "Abrir ticket",
    "description": "Crea un ticket para recibir ayuda del staff.",
    "emoji": "🎫",
    "category": "$var[category]",
    "rol_ping": "$var[rol_ping]",
    "channel_id_logs": "$var[channel_id_logs]"
   }
\],

  "options": [
   {
    "name": "Soporte Técnico",
    "description": "Problemas con servidor, bots, lag, permisos o errores técnicos.",
    "emoji": "🔧",
    "category": "$var[category]",
    "rol_ping": "$var[rol_ping]",
    "channel_id_logs": "$var[channel_id_logs]"
   },

   {
    "name": "Reportes de miembros",
    "description": "Reporta usuarios por toxicidad, trampas, spam o acoso.",
    "emoji": "🚨",
    "category": "$var[category]",
    "rol_ping": "$var[rol_ping]",
    "channel_id_logs": "$var[channel_id_logs]"
   },

   {
    "name": "Sugerencias e Ideas",
    "description": "Comparte ideas, mejoras o cambios para el servidor.",
    "emoji": "💡",
    "category": "$var[category]",
    "rol_ping": "$var[rol_ping]",
    "channel_id_logs": "$var[channel_id_logs]"
   },

   {
    "name": "Apelaciones de Bans┆Mutes",
    "description": "Apela sanciones si crees que fueron injustas.",
    "emoji": "⚖️",
    "category": "$var[category]",
    "rol_ping": "$var[rol_ping]",
    "channel_id_logs": "$var[channel_id_logs]"
   },

   {
    "name": "Preguntas Generales",
    "description": "Dudas sobre reglas o información del servidor.",
    "emoji": "❓",
    "category": "$var[category]",
    "rol_ping": "$var[rol_ping]",
    "channel_id_logs": "$var[channel_id_logs]"
   }
\]
}
]
```

---

# 🧩 Variable JSON Necesaria

También necesitas crear una variable JSON con el siguiente nombre y valor:


|Nombre      | Valor       |
|------------|-------------|
|ticketstwis | {}          |

Si no creas esta variable, el sistema no podrá guardar correctamente la información de los tickets.

---

# 🚀 Instalación

1. Descarga el sistema
2. Configura los IDs necesarios
3. Ajusta el archivo JSON
4. Ejecuta el sistema
5. Disfruta 🎉

---

# 💡 Requisitos

Conocimientos básicos de:
- BDFD / BDScript
- JSON (opcional para configuraciones avanzadas)

---

# 🛠️ Soporte

Si tienes problemas o necesitas ayuda para configurar nuevas opciones, puedes unirte al servidor de soporte de **[Sparkify World](https://sparkify-world.vercel.app/)**.
