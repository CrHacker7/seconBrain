CONV XML-JSON JSON-STRING

```java
import org.json.JSONObject;
import org.json.XML;

// Convertir XML a JSON
String xmlString = "<?xml version='1.0' encoding='UTF-8'?>\r\n<mensaje>...</mensaje>";
JSONObject json = XML.toJSONObject(xmlString);

// Convertir JSONObject a String (como JSON)
String jsonString = json.toString();
```
---

quarto Start
https://quarto.org/docs/websites/

1. install quarto.deb
2. install extension quarto in vscode
3. ctrl + shift + p = create project, new website
4. naming it

repo
git init
git config user.email "crhacker7"
git config user.name "crhacker7"
git add .
git commit -m "first commit"
git remote add origin repo-name
git push -u origin master
