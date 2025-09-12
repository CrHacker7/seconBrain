
ACTION → TRIGGER → ITEM → (HOST + KEY)

🧩 Paso 1: Autenticarse con Zabbix API
🧩 Paso 2: Obtener todas las Actions (con sus condiciones)
🧩 Paso 3: Filtrar las que usan conditiontype 1 (Trigger)
🧩 Paso 4: Para cada Trigger:
Obtener los items asociados
Tomar el primero (o todos, según el caso)
Extraer:
host (para -s)
key_ (para -k)
=================================
✅ 2. Obtener las Actions
json
Copiar código
{
  "jsonrpc": "2.0",
  "method": "action.get",
  "params": {
    "output": ["actionid", "name"],
    "selectConditions": "extend"
  },
  "auth": "AUTH_TOKEN_AQUI",
  "id": 2
}
📌 Aquí verás cada acción y dentro de "conditions" verás algunas de tipo "conditiontype": "1" → eso indica un trigger.

Guarda el value de esa condición. Es el triggerid.

✅ 3. Obtener Trigger a partir del triggerid

{
  "jsonrpc": "2.0",
  "method": "trigger.get",
  "params": {
    "output": ["triggerid", "description"],
    "triggerids": "TRIGGER_ID_AQUI",
    "selectItems": ["itemid", "key_", "hostid"]
  },
  "auth": "AUTH_TOKEN_AQUI",
  "id": 3
}
📌 Esto te devuelve los items asociados a ese trigger.

Guarda:

"key_" (es tu -k)

"hostid" (lo necesitas para el paso siguiente)

✅ 4. Obtener el nombre del Host (para -s)

{
  "jsonrpc": "2.0",
  "method": "host.get",
  "params": {
    "output": ["host"],
    "hostids": "HOST_ID_AQUI"
  },
  "auth": "AUTH_TOKEN_AQUI",
  "id": 4
}
📌 El "host" que te devuelve es lo que va en -s.

🧪 Resultado esperado:
Con eso vas a tener todo para construir el comando:


/usr/bin/zabbix_sender -z 192.168.10.10 -s NOMBRE_DEL_HOST -k OID_DEL_ITEM -o 1

🧠 ¿Y cómo adapto esto a SOAP?
Si tu sistema SOAP externo puede hacer llamadas tipo REST con JSON (como un wrapper), simplemente traduces estas estructuras JSON como cuerpo del mensaje.
Pero si SOAP es un sistema aparte (por ejemplo, HP OpenView, CA Spectrum, etc.) donde el OID vive en una base de conocimiento externa, ahí el flujo cambia.

                 ┌────────────┐
                 │  ACTIONS   │
                 └────┬───────┘
                      │
                      ▼
            ┌────────────────────┐
            │ action.get         │
            │ (con selectConditions)  
            └────────┬───────────┘
                     │
                     ▼
        ┌──────────────────────────────┐
        │  conditiontype == "1" (TRIGGER)
        │  => guardar triggerid         │
        └────────┬─────────────────────┘
                 │
                 ▼
           ┌──────────────────┐
           │ trigger.get      │
           │ selectItems =    │
           │ [itemid, key_, hostid] 
           └────────┬─────────┘
                    │
                    ▼
           ┌────────────────────┐
           │ host.get           │
           │ usando hostid      │
           └────────┬───────────┘
                    │
                    ▼
          ┌──────────────────────────┐
          │ CONSTRUIR COMANDO FINAL  │
          │ zabbix_sender -s <host>  │
          │                -k <key>  │
          │                -o 1      │
          └──────────────────────────┘

