# 🎟️ Panel de Tickets

Este es el código del panel utilizado para crear y gestionar el sistema de tickets.

> ⚠️ IMPORTANTE: El bot debe tener el permiso de **Administrador** para que el sistema funcione correctamente y sin errores.

---

## 🔐 Permisos requeridos del bot

El bot necesita acceso completo para poder gestionar el sistema de tickets correctamente:

- ✅ Crear canales
- ✅ Editar canales
- ✅ Eliminar canales
- ✅ Ver y enviar mensajes en logs
- ✅ Gestionar permisos de canales
- ✅ Administrar categorías
- ✅ Hacer menciones (ping)
- ✅ Crear y cerrar tickets sin restricciones

💡 **Recomendación:** Darle el permiso de **Administrador** es la forma más segura y sencilla de evitar errores. Este permiso incluye automáticamente todo lo necesario y garantiza compatibilidad con futuras actualizaciones del sistema.

---

## 👮 Permiso del staff

El miembro del staff que ejecute el comando para colocar el panel de tickets debe tener:

- 🛠️ `Manage Channels`

Sin este permiso, no podrá generar correctamente el panel.

---

## 🧩 Variable necesaria

| Nombre       | Valor |
|--------------|------|
| ticketstwis  | {}   |

---

## 💻 Código

```
$nomention
$addContainer[ticketstwis;#673ab7]

$if[$checkUserPerms[$authorID;managechannels]==false]
$reply 
$allowUserMentions[]
$addTextDisplay[## 🚫 Acceso denegado al comando;ticketstwis]
$addSeparator[true;large;ticketstwis]
$addTextDisplay[- No puedes usar este comando porque no eres staff en este servidor.;ticketstwis]
$stop 
 
$elseif[$isAdmin[$botID]==false]
$reply 
$allowUserMentions[]
$addTextDisplay[## 🔐 Solicitud de permisos de Administrador;ticketstwis]
$addSeparator[true;large;ticketstwis]
$addTextDisplay[- Necesito el permiso de Administrador para poder realizar mi trabajo correctamente y gestionar los tickets de forma adecuada;ticketstwis]
$stop 
$endif 

$var[category;La id de la categoría]
$var[rol_ping;La id del rol]
$var[channel_id_logs;La id del canal de los logs]
$jsonParse[
{
  "abrir_ticket": [
   { "name": "Abrir ticket","description": "Crea un ticket para recibir ayuda del staff.","emoji": "🎫","category": "$var[category]","rol_ping": "$var[rol_ping]","channel_id_logs": "$var[channel_id_logs]"}
 \],
  "options": [
   { "name": "Soporte Técnico","description": "Problemas con servidor, bots, lag, permisos o errores técnicos.","emoji": "🔧","category": "$var[category]","rol_ping": "$var[rol_ping]","channel_id_logs": "$var[channel_id_logs]"}, 
   { "name": "Reportes de miembros","description": "Reporta usuarios por toxicidad, trampas, spam o acoso.","emoji": "🚨","category": "$var[category]","rol_ping": "$var[rol_ping]","channel_id_logs": "$var[channel_id_logs]"},
   { "name": "Sugerencias e Ideas","description": "Comparte ideas, mejoras o cambios para el servidor.","emoji": "💡","category": "$var[category]","rol_ping": "$var[rol_ping]","channel_id_logs": "$var[channel_id_logs]"}, 
   { "name": "Apelaciones de Bans┆Mutes","description": "Apela sanciones si crees que fueron injustas.","emoji": "⚖️","category": "$var[category]","rol_ping": "$var[rol_ping]","channel_id_logs": "$var[channel_id_logs]"},
   { "name": "Preguntas Generales","description": "Dudas sobre reglas o información del servidor.","emoji": "❓","category": "$var[category]","rol_ping": "$var[rol_ping]","channel_id_logs": "$var[channel_id_logs]"}
 \]
}
] 


$addMediaGallery[image;ticketstwis]
$addMediaGalleryItem[https://i.imgur.com/4OQ1o6T.png;;false;image]
$addTextDisplay[**¡Bienvenidos al sistema de tickets de $serverName[$guildID]!**;ticketstwis]
$addSeparator[true;large;ticketstwis]
$addTextDisplay[Estamos aquí para ayudarte lo más rápido posible.;ticketstwis]
$addSeparator[false;small;ticketstwis]

$var[ts;-1] 
$if[$jsonArrayCount[options]>=1]$var[categoría;options] $else $var[categoría;abrir_ticket] $endif 
$addActionRow[menú;ticketstwis] 
$addStringSelect[menú_tickets;Elige la categoría;1;1;;menú] 
$eval[$repeatMessage[10;%{DOL}%var[ts\;%{DOL}%sum[%{DOL}%var[ts\]\;1\]\]
%{DOL}%if[%{DOL}%jsonExists[%{DOL}%var[categoría\]\;%{DOL}%var[ts\]\]==true\] 
%{DOL}%addStringSelectOption[%{DOL}%json[%{DOL}%var[categoría\]\;%{DOL}%var[ts\]\;name\]\;%{DOL}%var[categoría\]-%{DOL}%var[ts\]-abrír_ticket\;%{DOL}%cropText[%{DOL}%json[%{DOL}%var[categoría\]\;%{DOL}%var[ts\]\;description\]\;97\;..\]\;%{DOL}%json[%{DOL}%var[categoría\]\;%{DOL}%var[ts\]\;emoji\]\;\;menú_tickets\]
%{DOL}%endif]]  

$addSeparator[false;small;ticketstwis]
$addTextDisplay[-# Por favor, elige la categoría que mejor describe tu problema o duda;ticketstwis]
```

---

# 🛠️ Soporte

Si tienes problemas o necesitas ayuda para configurar nuevas opciones, puedes unirte al servidor de soporte de **[Sparkify World](https://sparkify-world.vercel.app/)**.