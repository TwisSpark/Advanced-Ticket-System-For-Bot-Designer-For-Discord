# 🎟️ Sistema de Tickets - $onInteraction

Este sistema se encarga de manejar la interacción del panel de tickets y ejecutar la acción correspondiente cuando un usuario selecciona una opción.

---

## ⚙️ Funcionamiento

- Detecta la opción seleccionada en el menú del panel de tickets
- Lee la configuración desde el JSON del sistema
- Procesa la información del ticket seleccionado
- Ejecuta la creación del ticket automáticamente
- Aplica la categoría correspondiente según la opción elegida
- Mantiene la estructura del sistema sin necesidad de modificar el código base

---

## 🧠 Importante

Este sistema está diseñado para funcionar junto al panel de tickets y el JSON de configuración.  
No es necesario modificar esta parte del sistema, ya que cualquier cambio puede afectar el funcionamiento de los tickets.

---

## 💡 Nota

Puedes agregar más opciones al sistema editando el JSON (hasta 10 opciones soportadas).

---

## 💻 Código `$onInteraction`

```
$nomention
$onlyIf[$checkContains[$customID;menú_tickets;create_ticket;reclamar_ticket;delete_ticket]==true;]

$var[category;] $c[<-La id de la categoría]
$var[rol_ping;] $c[<-La id del rol]
$var[channel_id_logs;] $c[<-La id del canal de los logs]
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

$if[$checkContains[$customID;menú_tickets]==true]
$textSplit[$message;-]
$ephemeral
$var[ticket_emoji_info;$json[$splitText[1];$splitText[2];emoji]]
$var[ticket_name_info;$json[$splitText[1];$splitText[2];name]]
$var[ticket_description_info;$json[$splitText[1];$splitText[2];description]]
$jsonParse[$getUserVar[ticketstwis;$authorID;$guildID]]

$if[$channelExists[$json[info;ticket_id]]==false]
$addContainer[ticketstwis;#673ab7]
$addTextDisplay[## ❔ Espera un momento;ticketstwis]
$addSeparator[true;large;ticketstwis]
$addSection[ticket;ticketstwis]
$addTextDisplay[Confirmas que deseas abrir este ticket?;ticket]
$addTextDisplay[Ticket de $var[ticket_emoji_info] **$var[ticket_name_info]**;ticket]
$addButtonCV2[$splitText[1]-$splitText[2]-create_ticket;Abrir;Secondary;false;;ticket]
$addSeparator[false;small;ticketstwis]
$addTextDisplay[-# Ten en cuenta que abrir tickets sin una razón válida puede causar problemas.;ticketstwis]
$else 
$ephemeral 
$addContainer[ticketstwis;#673ab7]
$addTextDisplay[## ⏳ Ticket ya abierto;ticketstwis]
$addSeparator[true;large;ticketstwis]
$addTextDisplay[Ya tienes un ticket activo: <#$json[info;ticket_id]>

Espera a que se cierre para poder crear uno nuevo.;ticketstwis]
$endif  
$stop 
$endif


$if[$checkContains[$customID;create_ticket]==true]
$textSplit[$customID;-]
$var[ticket_emoji_info;$json[$splitText[1];$splitText[2];emoji]]
$var[ticket_name_info;$json[$splitText[1];$splitText[2];name]]
$var[ticket_description_info;$json[$splitText[1];$splitText[2];description]]
$var[ticket_category_info;$json[$splitText[1];$splitText[2];category]]
$var[ticket_rol_ping_info;$json[$splitText[1];$splitText[2];rol_ping]]
$var[ticket_channel_id_logs;$json[$splitText[1];$splitText[2];channel_id_logs]]
$var[ticket_name;$replaceText[🎟️・ticket-$username;.;_]]
$var[info_channel;
🎟️ Información del Ticket

📌 **Tema del ticket:**
$var[ticket_name_info]

📝 **Descripción:**
$var[ticket_description_info] 

📆 **Fecha:**
<t:$getTimestamp:F>

▰▰▰▰▰▰▰▰▰▰▰▰▰▰▰▰▰ 

👤 Información del Usuario
 
📝 **Nombre del usuario:**
$nickname

🆔 **ID del usuario:** 
$authorID
]

$async[create_ticket]
$if[$channelExists[$var[ticket_category_info]]==false] $createChannel[$var[ticket_name];text] $else $createChannel[$var[ticket_name];text;$var[ticket_category_info]] $endif 
$endasync
$await[create_ticket]
$async[id_ticket]
 
$jsonParse[$getUserVar[ticketstwis;$authorID;$guildID]]
$jsonSetString[info;ticket_id;$findChannel[$var[ticket_name]]]
$jsonSetString[info;ticket_logs;$var[ticket_channel_id_logs]]
$jsonSetString[info;ticket_info;$var[info_channel]]
$setUserVar[ticketstwis;$jsonStringify;$authorID;$guildID]

$endasync

$await[id_ticket]

$async[edit_ticket]
$jsonParse[$getUserVar[ticketstwis;$authorID;$guildID]]
$var[ticket_id;$json[info;ticket_id]]
$modifyChannel[$var[ticket_id];$var[ticket_name];$json[info;ticket_info];;;]
$endasync
$await[edit_ticket]


$addContainer[ticketstwis;#673ab7]
$addTextDisplay[## 🎫 Ticket abierto correctamente;ticketstwis]
$addSeparator[true;large;ticketstwis]
$addTextDisplay[**Categoría:** $var[ticket_emoji_info] **$var[ticket_name_info]**
> $var[ticket_description_info]

**Tu ticket es:** <#$var[ticket_id]>

-# Sistema de tickets : $serverName[$guildID];ticketstwis]

$async[ticket]
$useChannel[$var[ticket_id]]
$var[Message;$sendEmbedMessage[$var[ticket_id];;🎟️ Ticket de $var[ticket_name_info];;
**Hola $nickname,**

*Un miembro del staff te atenderá en breve.*

- Por favor, explica con detalle lo que necesitas para poder ayudarte más fácilmente.
;#673ab7;$nickname;$authorAvatar;$serverName[$guildID];$serverIcon;;;yes;yes]]
$addButton[no;delete_ticket-$authorID;Cerrar Ticket;Danger;no;🔒;$var[Message]]
$addButton[no;reclamar_ticket-$authorID;Reclamar Ticket;secondary;no;🎟️;$var[Message]]
$endasync

$await[ticket]

$if[$roleExists[$var[ticket_rol_ping_info]]==true]
$channelSendMessage[$var[ticket_id];<@$authorID> en un momento te ayuda un staff con el rol de <@&$var[ticket_rol_ping_info]>]
$endif 

$if[$jsonExists[info;ticket_logs]==true]
$var[channel_logs;$sendEmbedMessage[$var[ticket_channel_id_logs];;Nuevo ticket abierto;;
$var[info_channel]
;#673ab7;Autor : $nickname;$authorAvatar;$serverName[$guildID];$serverIcon;;;yes;yes]]
$stop 
$endif 
$endif


$if[$checkContains[$customID;reclamar_ticket]==true]
$textSplit[$customID;-]
$var[userid;$splitText[2]]
$jsonParse[$getUserVar[ticketstwis;$var[userid];$guildID]] 

$if[$var[userid]==$authorID]
$ephemeral 
$removeButtons 
$title[🚫 Restricción de Reclamo de Ticket]
$description[- No puedes reclamar tu propio ticket]
$color[#673ab7]

$elseif[$jsonExists[staff;id]==true]
$ephemeral 
$removeButtons 
$title[⚠️ Ticket en atención]
$description[<@$json[staff;id]> ya está atendiendo este ticket.

-# No es posible reclamarlo nuevamente.]
$color[#673ab7]
$else 
$jsonParse[$getUserVar[ticketstwis;$var[userid];$guildID]]
$jsonSetString[staff;id;$authorID]
$setUserVar[ticketstwis;$jsonStringify;$var[userid];$guildID]
$editChannelPerms[$channelID;$var[userid];+sendmessages]
$editChannelPerms[$channelID;$json[staff;id];+sendmessages]
$editChannelPerms[$channelID;$guildID;-sendmessages;-createprivatethreads;-createpublicthreads]

$sendEmbedMessage[$channelID;;📌 Ticket reclamado;;
<@$json[staff;id]> ha tomado el ticket de $nickname[$var[userid]].
;#673ab7;ticket de $nickname[$var[userid]];$userAvatar[$var[userid]];$serverName[$guildID];$serverIcon;;;yes]
$endif 
$stop 
$endif 



$if[$checkContains[$customID;delete_ticket]==true]

$textSplit[$customID;-]
$var[userid;$splitText[2]]
$jsonParse[$getUserVar[ticketstwis;$var[userid];$guildID]] 

$if[$and[$var[userid]!=$authorID;$jsonExists[staff;id]==false]==true]
$ephemeral 
$removeButtons 
$title[⚠️ Debes reclamar el ticket]
$description[Para cerrar este ticket, primero debes reclamarlo.

-# De lo contrario, solo el autor puede cerrarlo.]
$color[#673ab7]
$else 

$sendEmbedMessage[$channelID;;🔐 Ticket Cerrado;;- Este ticket se cerrará y eliminará en 10 segundos;#673ab7;$nickname;$authorAvatar;$serverName[$guildID];$serverIcon;;;yes]
$editChannelPerms[$channelID;$guildID;-sendmessages;-createprivatethreads;-createpublicthreads]

$if[$jsonExists[info;ticket_logs]==true]
$async[channel_logs]
$replyIn[9s]
$var[channel_logs;$sendEmbedMessage[$json[info;ticket_logs];;Ticket cerrado;;
$json[info;ticket_info]

$if[$jsonExists[staff;id]==true]▰▰▰▰▰▰▰▰▰▰▰▰▰▰▰▰▰

👮 Información del Staff

📝 **Nombre del Staff:**
$nickname

🆔 **ID del Staff:** 
$authorID 
$endif 
;#673ab7;$if[$jsonExists[staff;id]==true]Staff$elseAutor$endif: $nickname;$authorAvatar;$serverName[$guildID];$serverIcon;;;yes;yes]]
$endasync
$endif

$async[Clean and erase] 
$replyIn[10s]
$resetUserVar[ticketstwis;$var[userid]]
$deleteChannels[$channelID]
$endasync
$endif 

$stop
$endif 
``` 

---

# 🛠️ Soporte

Si tienes problemas o necesitas ayuda para configurar nuevas opciones, puedes unirte al servidor de soporte de **Sparkify**.