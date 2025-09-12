📄 Chuleta rápida de Zabbix API — objetos y formatos
🔸 hostgroup.get
{
  "method": "hostgroup.get",
  "params": {
    "output": ["groupid", "name"],           // Lista []
    "filter": {
      "name": ["Producción", "Red"]          // Objeto { clave: lista }
    }
  }
}

🔸 host.get
{
  "method": "host.get",
  "params": {
    "output": ["hostid", "host", "name"],     // Lista []
    "groupids": ["15"],                       // Lista []
    "filter": {
      "status": 0                             // Objeto {}
    },
    "searchInventory": {
      "hw_arch": "inelcom"                    // Objeto {}
    },
    "selectInventory": ["type", "vendor"],    // Lista []
    "sortfield": "name",                      // Cadena
    "sortorder": "DESC"                       // Cadena
  }
}

🔸 item.get
{
  "method": "item.get",
  "params": {
    "output": ["itemid", "name", "key_"],     // Lista []
    "hostids": ["10105"],                     // Lista []
    "filter": {
      "type": 0,                              // Objeto {}
      "status": 0
    },
    "search": {
      "key_": "cpu"                           // Objeto {}
    },
    "sortfield": "name",                      // Cadena
    "sortorder": "ASC"                        // Cadena
  }
}

🔸 trigger.get
{
  "method": "trigger.get",
  "params": {
    "output": ["triggerid", "description", "priority", "lastchange"],
    "groupids": ["15"],
    "filter": {
      "value": 1,                             // Problema activo
      "status": 0                             // Habilitado
    },
    "expandDescription": true,               // Booleano
    "selectHosts": ["hostid", "name"],        // Lista []
    "sortfield": "priority",                  // Cadena
    "sortorder": "DESC"                       // Cadena
  }
}

🔸 history.get (valores recientes de ítems)
{
  "method": "history.get",
  "params": {
    "output": "extend",                       // Cadena especial
    "history": 0,                             // Tipo de dato (0 = numérico float)
    "itemids": ["23345", "23346"],            // Lista []
    "sortfield": "clock",                     // Cadena
    "sortorder": "DESC",                      // Cadena
    "limit": 10                               // Número
  }
}

🔸 event.get (eventos recientes)
{
  "method": "event.get",
  "params": {
    "output": "extend",
    "selectHosts": ["hostid", "name"],
    "source": 0,                              // 0 = trigger
    "object": 0,                              // 0 = trigger object
    "sortfield": "clock",
    "sortorder": "DESC",
    "limit": 5
  }
}

| Tipo de campo       | Formato      | Ejemplo                                     |
| ------------------- | ------------ | ------------------------------------------- |
| Listas múltiples    | `[]`         | `"output": ["hostid", "name"]`              |
| Filtros simples     | `{}`         | `"filter": { "status": 0 }`                 |
| Búsquedas parciales | `{}`         | `"search": { "key_": "cpu" }`               |
| Campos especiales   | `"string"`   | `"output": "extend"`, `"sortfield": "name"` |
| Booleanos           | `true/false` | `"expandDescription": true`                 |
| Números             | `int`        | `"limit": 10`                               |
