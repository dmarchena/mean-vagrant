# Habilitar el CORS en Express

Al ejecutar una aplicación Express en nuestro Vagrant es común encontrarse con este error:

```
No 'Access-Control-Allow-Origin' header is present on the requested resource.  
Origin 'http://192.168.56.12:3000' is therefore not allowed access.
```

Para evitarlo, copia el siguiente código en el `middleware` y adáptalo a las necesidades de la aplicación:

```
app.use(function (req, res, next) {
    // Website you wish to allow to connect
    res.setHeader('Access-Control-Allow-Origin', 'http://192.168.56.12:3000');
    // Request methods you wish to allow
    res.setHeader('Access-Control-Allow-Methods', 'GET, POST, OPTIONS, PUT, PATCH, DELETE');
    // Request headers you wish to allow
    res.setHeader('Access-Control-Allow-Headers', 'Origin, X-Requested-With, Content-type, Accept');
    // Set to true if you need the website 
    // to include cookies in the requests sent to the API 
    // (e.g. in case you use sessions)
    res.setHeader('Access-Control-Allow-Credentials', true);
    
    next();
});
```
_Código adaptado a la IP del `appserver` de este entorno Vagrant a partir de [esta respuesta](http://stackoverflow.com/a/18311469/3240619) de [jvandemo](http://stackoverflow.com/users/2485660/jvandemo) en [StackOverflow](http://stackoverflow.com)_