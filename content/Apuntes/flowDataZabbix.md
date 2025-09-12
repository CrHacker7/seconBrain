DIBUJO CON EMOJIS

[1] 📦 Zabbix Database
    🡇
[2] 🌐 API Request: "host.get"
    🡇
[3] 📨 JSON Response con datos del host
    Ejemplo:
    {
      "host": "servidor-original",
      "interfaces": [
          {
            "type": "2",
            "ip": "192.168.1.10",
            "port": "161",
            ...
          }
      ],
      ...
    }
    🡇
[4] 🧠 Java: List<ZHost> completeHostList
    🔁 Se transforma ese JSON en objetos ZHost

    Por ejemplo:
    ZHost completeHost = completeHostList.get(0);

    🡇
[5] 🔍 Se accede a las interfaces del host:
    completeHost.getInterfaces() → List<ZHostInterface>

    Se filtra:
    if (hostInterface.getType().equals("2")) { ... }

    🡇
[6] 🛠 Se crea una lista de mapas:
    List<Map<String, String>> interfaces

    Cada mapa contiene:
    {
      "type": "2",
      "ip": "192.168.20.20",
      "port": "161",
      ...
    }

    🡇
[7] 🧩 También se cargan:
    ✅ List<Map<String, Object>> groups
    ✅ List<Map<String, Object>> templates
    ✅ Map<String, Object> inventory

    🡇
[8] 🧠 Se llama a:
    zOperatorCFG.hostCreateWithInventoryAndTemplate(
        host, name, interfaces, groups, templates, inventory
    )

    🡇
[9] 🌐 API Request: "host.create"
    Enviado a Zabbix con todos los datos

    Ejemplo de JSON que se envía:
    {
      "host": "nuevo-host-clonado",
      "interfaces": [...],
      "groups": [...],
      "templates": [...],
      "inventory": {...}
    }

    🡇
[10] ✅ Zabbix responde con éxito:
    {
      "result": {
        "hostids": ["10234"]
      }
    }

    🡇
[11] 🎉 Host clonado en Zabbix con ID nuevo




---
 **cómo construir correctamente el JSON que se le envía a Zabbix desde tu código Java**, y **cómo tratar la respuesta para validar que todo salió bien**.

---

## 🧭 CONTEXTO: 

El código Java arma estructuras (`interfaces`, `groups`, `templates`, `inventory`) y las pasa a un método como este:

```java
zOperatorCFG.hostCreateWithInventoryAndTemplate(host, name, interfaces, groups, templates, inventory);
```

Este método probablemente termina haciendo un request tipo:

```http
POST /api_jsonrpc.php
Content-Type: application/json
```

Con un cuerpo JSON como este:

```json
{
  "jsonrpc": "2.0",
  "method": "host.create",
  "params": {
    "host": "nombre-del-host",
    "interfaces": [...],
    "groups": [...],
    "templates": [...],
    "inventory": {...}
  },
  "auth": "token_de_autenticacion",
  "id": 1
}
```

---

## ✅ ¿Cómo construir ese JSON en Java?

Cómo Java convierte tus estructuras (`List`, `Map`, etc.) en un JSON válido que **Zabbix sí acepta**.

### 1. `interfaces`: ✅ JSON válido

```java
List<Map<String, String>> interfaces = new ArrayList<>();

Map<String, String> iface = new HashMap<>();
iface.put("type", "2");
iface.put("main", "1");
iface.put("useip", "1");
iface.put("ip", "192.168.1.10");
iface.put("dns", "");
iface.put("port", "161");

interfaces.add(iface);
```

Se convierte en:

```json
"interfaces": [
  {
    "type": "2",
    "main": "1",
    "useip": "1",
    "ip": "192.168.1.10",
    "dns": "",
    "port": "161"
  }
]
```

---

### 2. `groups`: ✅ JSON válido

```java
List<Map<String, Object>> groups = new ArrayList<>();

Map<String, Object> group = new HashMap<>();
group.put("groupid", "10");
groups.add(group);
```

Resultado:

```json
"groups": [
  { "groupid": "10" }
]
```

---

### 3. `templates`: ✅ JSON válido

```java
List<Map<String, Object>> templates = new ArrayList<>();

Map<String, Object> template = new HashMap<>();
template.put("templateid", "10001");
templates.add(template);
```

Resultado:

```json
"templates": [
  { "templateid": "10001" }
]
```

---

### 4. `inventory`: ✅ JSON válido

```java
Map<String, Object> inventory = new HashMap<>();
inventory.put("os", "Linux");
inventory.put("location", "Madrid");
```

Resultado:

```json
"inventory": {
  "os": "Linux",
  "location": "Madrid"
}
```

---

## 🔄 ¿Cómo se convierte todo eso en un JSON completo?

Supongamos que estás usando `org.json.JSONObject`, podrías construir el body así:

```java
JSONObject params = new JSONObject();
params.put("host", "nombre-del-clonado");
params.put("interfaces", interfaces);
params.put("groups", groups);
params.put("templates", templates);
params.put("inventory", inventory);

JSONObject request = new JSONObject();
request.put("jsonrpc", "2.0");
request.put("method", "host.create");
request.put("params", params);
request.put("auth", "token_zabbix");
request.put("id", 1);
```

Y lo envías con un cliente HTTP (como `HttpClient`, `HttpURLConnection`, etc.)

---

## 🧪 ¿Cómo tratar la respuesta?

Cuando Zabbix responde, recibimoss un `JSONObject response`, que puede tener:

### 🔹 Éxito:

```json
{
  "jsonrpc": "2.0",
  "result": {
    "hostids": ["10234"]
  },
  "id": 1
}
```

### 🔹 Error:

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32602,
    "message": "Invalid params.",
    "data": "Host already exists"
  },
  "id": 1
}
```

---

### ✔️ Tratamiento del resultado en Java:

```java
if (response.has("error")) {
    JSONObject error = response.getJSONObject("error");
    logger.error("Error creando host: " + error.getString("message") + " - " + error.getString("data"));
} else {
    JSONObject result = response.getJSONObject("result");
    JSONArray hostIds = result.getJSONArray("hostids");
    logger.info("Host creado correctamente. ID: " + hostIds.getString(0));
}
```

---

## 🚨 ERRORES COMUNES Y CÓMO EVITARLOS

| Problema                     | Causa común                               | Solución rápida                                       |
|-----------------------------|-------------------------------------------|--------------------------------------------------------|
| `"Invalid params"`          | Campos mal formateados                    | Asegurarse de usar `List<Map<String, ?>>` correctos   |
| `"Host already exists"`     | El host ya existe con ese nombre          | Cambiar `host` en `"params"`                          |
| `"Group ID not found"`      | `groupid` no válido o inexistente         | Verificar con `hostgroup.get`                         |
| `"Missing templateid"`      | Template no asignado correctamente        | Usar `template.get` antes y validar el ID             |

---

## ✅ BONUS: Validación antes de enviar

Siempre que puedas, loguea los datos antes de hacer el `host.create`:

```java
logger.debug("Creando host con datos: " + params.toString(2));
```


---

## 📦 



