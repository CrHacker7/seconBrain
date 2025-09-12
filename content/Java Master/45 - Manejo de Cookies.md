Cambiamos al jsp y en el WebServlet añadimos /login.jsp y al tener dos se encierran con llaves

```java
Cookie[] cookies = req.getCookies() != null ? req.getCookies(): new Cookie[0];
        Optional<String> cookieOptional = Arrays.stream(cookies)
                        .filter(c -> "username".equals(c.getName()))
                .map(Cookie::getValue)
                .findAny();
        if (cookieOptional.isPresent()){
	        outhtml
        else {
	        getServletContext().getRequestDispatcher("/login.jsp")
	        .forward(req, resp);
        }
```