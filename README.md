
# CRUD tienda de informática

El Servlet no se ejecuta directamente: hay que desplegarlo en un servidor Servlet, normalmente **Tomcat**.

En `pom.xml` indica que se genera un archivo WAR: `<packaging>war</packaging>`

En la terminal de VS Code, compila el proyecto: `mvn clean package`

Maven genera `target/crudtiendainformatica-1.0-SNAPSHOT.war`

Hay que iniciar el servidor Tomcat, desplegar el archivo war generado, y publicar el servidor.

En el servidor de Tomcat de VSCode se accede desde `http://localhost:8080/crudtiendainformatica-1.0-SNAPSHOT/hello-servlet`

## Estado actual

El proyecto ya incluye el esquema SQL de tienda y una base para el CRUD de fabricantes: modelo, interfaz e implementación DAO, servlet y páginas JSP. 

Sin embargo, hay formularios con rutas incorrectas y la configuración de conexión contiene credenciales de ejemplo. No se encuentran clases ni vistas para productos.

## Instrucciones para completar el proyecto

**Objetivo:** completar la aplicación web Java para ofrecer operaciones CRUD sobre fabricantes y productos almacenados en MySQL, usando la estructura existente de Servlet, DAO, modelo y JSP.

### 0. Comprender y verificar las rutas iniciales

Antes de implementar nuevas funcionalidades, identifica el contexto de la aplicación asignado por Tomcat al WAR. En las siguientes URLs, <contexto> es ese nombre; por ejemplo, inicialmente es crudtiendainformatica-1.0-SNAPSHOT, aunque VS Code permita configurarlo con otro valor.

`http://localhost:8080/<contexto>/hello-servlet`

- Comprueba que Tomcat ha desplegado la aplicación y que responde el servlet de prueba con “Hello World!”. Está definido en HelloServlet.java.

`http://localhost:8080/<contexto\>/fabricantes`

- Accede al listado de fabricantes mediante FabricantesServlet.

`http://localhost:8080/<contexto>/fabs`

- Accede al mismo servlet mediante una ruta alternativa. Ambas rutas están declaradas en FabricantesServlet.java.

`http://localhost:8080/<contexto>/`

- Debe redirigir al listado de fabricantes mediante index.jsp.

### 1. Preparar la conexión a la base de datos

* Crear la base de datos e insertar los datos iniciales ejecutando `tienda.sql`.
* Configurar en `database.properties` la URL, el nombre de usuario y la contraseña reales de MySQL.
* Comprobar que el proyecto conecta a la base tienda desde Tomcat.

### 2. Completar y probar el CRUD de fabricantes

* Mantener las operaciones de listar, consultar, crear, editar y eliminar ya planteadas en el DAO y el servlet.
* Corregir las rutas de los formularios JSP: el formulario de creación debe enviar a la ruta que atiende el servlet; el de edición debe conservar el identificador del fabricante; y las acciones deben funcionar desde el contexto de la aplicación.
* Comprobar que eliminar un fabricante asociado a productos no deja datos inconsistentes. Mostrar un mensaje comprensible si la clave foránea impide el borrado.
* Verificar que cada operación actualiza la lista y que los identificadores generados se reflejan correctamente.

### 3. Implementar el modelo y acceso a datos de productos

* Crear la clase `Producto` con identificador, nombre, precio e información del fabricante asociado.
* Crear `ProductoDAO` y `ProductoDAOImpl` con métodos para listar, buscar por identificador, insertar, actualizar y borrar productos.
* Usar `PreparedStatement` para consultas con parámetros.
* Al consultar productos, obtener también el nombre del fabricante mediante la relación `producto.id_fabricante = fabricante.id`.

### 4. Implementar el CRUD web de productos

* Crear un servlet que gestione las rutas para listar productos, mostrar detalle, crear, editar y eliminar.
* Crear JSP para el listado, detalle, formulario de alta y formulario de edición, siguiendo la organización existente en `WEB-INF/jsp`.
* En los formularios de alta y edición, ofrecer un selector de fabricantes cargado desde la base de datos.
* Mostrar en el listado el nombre del fabricante asociado a cada producto.
* Al borrar un producto, volver al listado y reflejar el cambio.

### 5. Validar entradas y errores

* Rechazar nombres vacíos, precios negativos o no numéricos y fabricantes inexistentes.
* Manejar identificadores que no sean números y registros que no existan.
* Mostrar mensajes entendibles al usuario ante errores de validación o de base de datos, sin presentar trazas técnicas en la página.

### 6. Probar la entrega en Tomcat

* Desplegar el WAR en Tomcat compatible con `jakarta.servlet` (Tomcat 10 o superior).
* Verificar el flujo completo de altas, consultas, modificaciones y eliminaciones para ambas entidades.
* Confirmar que la relación se mantiene: un producto siempre tiene un fabricante válido y no se puede borrar un fabricante que todavía tenga productos asociados, salvo que se implemente y documente explícitamente otra política.

**Criterio de finalización:** desde el navegador se puede administrar fabricantes y productos, los datos persisten en MySQL y la relación entre ambas tablas se muestra y se valida correctamente.
