# PPS-Unidad3Actividad12-Broken Authentication
Explotación y Mitigación de Broken Authenticatión().
Tenemos como objetivo:

> - Ver cómo se pueden hacer ataques autenticación.
>
> - Analizar el código de la aplicación que permite ataques de autenticación débil.
>
> - Implementar diferentes modificaciones del codigo para aplicar mitigaciones o soluciones.

## ¿Qué es la Autenticación débil?
---

Algunos sitios web ofrecen un proceso de registro de usuarios que automatiza (o semiautoma) el aprovisionamiento del acceso del sistema a los usuarios. Los requisitos de identidad para el acceso varían de una identificación positiva a ninguna, dependiendo de los requisitos de seguridad del sistema. Muchas aplicaciones públicas automatizan completamente el proceso de registro y aprovisionamiento porque el tamaño de la base de usuarios hace que sea imposible administrar manualmente. Sin embargo, muchas aplicaciones corporativas aprovisionarán a los usuarios manualmente, por lo que este caso de prueba puede no aplicarse.

Esto puede incluir credenciales débiles, almacenamiento inseguro de contraseñas, gestión inadecuada de sesiones y falta de protección contra ataques de fuerza bruta.

**Consecuencias de Autenticación débil:**
- Descubrimiento de credenciales de usuario.
- Ejecución de ataques de suplantación de usuarios. 

- 
## ACTIVIDADES A REALIZAR
---
> Lee detenidamente la sección de vulnerabilidades de subida de archivos.  de la página de PortWigger <https://portswigger.net/web-security/authentication>
>
> Lee el siguiente [documento sobre Explotación y Mitigación de ataques de Remote Code Execution](./files/ExplotacionYMitigacionBrokenAuthentication.pdf>
> 
> También y como marco de referencia, tienes [ la sección de correspondiente de los Procesos de Registros de Usuarios del  **Proyecto Web Security Testing Guide** (WSTG) del proyecto **OWASP**.](https://owasp.org/www-project-web-security-testing-guide/stable/4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process)
>


Vamos realizando operaciones:

## Creación de la Base de Datos

Para realizar esta actividad necesitamos acceder a una Base de datos con usuarios y contraseñas. Si ya la has creado en la actividad de Explotación y mitigación de ataques de inyección SQL, no es necesario que la crees de nuevo. Si no la has creado, puedes verlo en <https://github.com/jmmedinac03vjp/PPS-Unidad3Actividad4-InyeccionSQL> en la sección de Creación de Base de datos.

Crea la tabla de usuarios. Debería de mostrarte algó así al acceder a:

~~~
http://localhost:8080
~~~

![](images/ba1.png)

## Código vulnerable

El código contiene varias vulnerabilidades que pueden ser explotadas para realizar ataques de autenticación rota.

Crear al archivo **login_weak.php** con el siguiente contenido (tencuidado de sustituír **mi_password** por la contraseña de root de tu BBDD:

~~~
<?php
// creamos la conexión 
$conn = new mysqli("database", "root", "josemi", "SQLi");

if ($conn->connect_error) {
        // Excepción si nos da error de conexión
        die("Error de conexión: " . $conn->connect_error);
}
if ($_SERVER["REQUEST_METHOD"] == "POST" || $_SERVER["REQUEST_METHOD"] == "GET") {
        // Recogemos los datos pasados
        $username = $_REQUEST["username"];
        $password = $_REQUEST["password"];

        print("Usuario: " . $username . "<br>");
        print("Contraseña: " . $password . "<br>");

        // preparamos la consulta
        $query = "SELECT * FROM usuarios WHERE usuario = '$username' AND contrasenya = '$password'";
        print("Consulta SQL: " . $query . "<br>");

        //realizamos la consulta y recogemos los resultados
        $result = $conn->query($query);
        if ($result->num_rows > 0) {
        echo "Inicio de sesión exitoso";
        } else {
                echo "Usuario o contraseña incorrectos";
        }
}
$conn->close();

?>
<form method="post">
        <input type="text" name="username" placeholder="Usuario">
        <input type="password" name="password" placeholder="Contrasenya">
        <button type="submit">Iniciar Sesión</button>
</form>
~~~
Antes de acceder la página web, asegurarse de que el servicio está en ejecución, y si es necesario, arrancar o reiniciar el servicio.

Acceder a la pagina web aunque también podemos poner directamente el usuario y contraseña. Un ejemplo es  el siguiente enlace:

~~~
http://localhost/login_weak.php?username=admin&password=123456
~~~


Vemos que si los datos son incorrectos nos muestra que no lo es:

![](images/ba2.png)

Y si es correcta nos lo indica:

![](images/ba3.png)



**Vulnerabilidades del código:**
1. Inyección SQL: La consulta SQL usa variables sin validación, lo que permite ataques de inyección.

2. Uso de contraseñas en texto plano: No se usa hashing para almacenar las contraseñas, lo que facilita su robo en caso de acceso a la base de datos.

3. Falta de control de intentos de inicio de sesión: No hay mecanismos de protección contra ataques de fuerza bruta.

4. Falta de gestión segura de sesiones: No se generan tokens de sesión seguros tras un inicio de sesión exitoso.

---


![](images/ba1.png)
![](images/ba1.png)
![](images/ba1.png)
![](images/ba1.png)
![](images/ba1.png)
![](images/ba1.png)
![](images/ba1.png)

![](images/.png)


### **Código seguro**
---

Aquí está el código securizado:

🔒 Medidas de seguridad implementadas

- :

        - 

        - 



🚀 Resultado

✔ 

✔ 

✔ 

## ENTREGA

> __Realiza las operaciones indicadas__

> __Crea un repositorio  con nombre PPS-Unidad3Actividad6-Tu-Nombre donde documentes la realización de ellos.__

> No te olvides de documentarlo convenientemente con explicaciones, capturas de pantalla, etc.

> __Sube a la plataforma, tanto el repositorio comprimido como la dirección https a tu repositorio de Github.__

