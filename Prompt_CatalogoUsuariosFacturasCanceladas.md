## Prompt mejorado para GitHub Copilot: Catálogo de Usuarios para Facturas Canceladas

**Contexto:**  
Desarrollar un catálogo para administrar los usuarios del módulo "Facturas Canceladas", conectado a través de una API. Usa el archivo `README.md` del repositorio como referencia obligatoria para aplicar estándares de estilo y buenas prácticas de codificación. A continuación se detallan los requisitos por capa y archivo.

---

**Objetivo:**  
Permitir consultar, insertar y eliminar usuarios relacionados con el módulo de Facturas Canceladas.

---

**Tecnologías a utilizar:**  
- **Frontend:** DataTable, jQuery, Alertify, Bootstrap  
- **Backend:** .NET (o conforme a la arquitectura del repositorio)  
- **Comunicación:** API (peticiones utilizando fetch)  
- **Configuración:** Usar la cadena “strConnectionSC”

---

## Detalles Técnicos

### Frontend

**Archivo principal:**  
- `CapUsuariosFacturaCancelada.aspx`  
  - Utiliza la MasterPage `MasterPage01_bootstrap`.
  - Aplica estilos Bootstrap a todos los elementos HTML.

**Componentes:**
1. **Contenedor para agregar usuario:**
   - Campo "Sucursal": `<select>` (opciones obtenidas vía API - método CatalogoSucursales)
   - Campo "Usuario": `<select>` (opciones vía API - método CatalogoUsuarios dependiente de la sucursal)
   - Botón "Agregar"

2. **Contenedor de tabla "Usuarios Activos":**
   - Tabla con columnas: "Sucursal", "Usuario", "Correo", "Desactivar"
   - La columna "Desactivar" contiene un botón para cada registro.

3. **Funcionalidad de Loading:**
   - Incluye el código CSS y los elementos HTML necesarios para mostrar un indicador de carga en las operaciones asincrónicas.

---

**JavaScript (`CapUsuariosFacturaCancelada.js`):**
- Estructura el código como un singleton.
- Utiliza Alertify para mostrar alertas, validaciones y errores.
- Usa la librería DataTable para la tabla de usuarios activos.
- Declara una variable global para la URL base del API: `ApplicationUrl`.
- Realiza llamadas al API mediante fetch, formando las rutas como `ApplicationUrl + controlador + método`.
- Al cargar la página, inicializa los elementos y llama al método CatalogoSucursales para llenar el select "Sucursal".
- Al cambiar el select "Sucursal", consulta el método CatalogoUsuarios, actualizando el select "Usuario" y la tabla de usuarios activos.
- El botón "Agregar" llama al método AgregarUsuario (parámetros: intIdCd del select "Sucursal" e intIdU del select "Usuario"). La respuesta actualiza el select "Usuario" y la tabla de usuarios activos.
- El botón "Desactivar" en la tabla llama al método DesactivarUsuario (parámetros: intIdCd y intIdU), actualizando la tabla tras la operación.
- Implementa validaciones, manejo robusto de errores y mensajes amigables para el usuario.

---

### Backend

**Entidad:**  
- `entUsuarioFacturaCancelada.cs`
  - Propiedades: `intIdCd`, `strCdNombre`, `intIdUsuario`, `strUsuario`, `strCorreo`

**Controlador:**  
- `CapUsuariosFacturaCanceladaController.cs`
  - Métodos:
    - `CatalogoSucursales`: Devuelve lista de sucursales (`intIdCd`, `strCdNombre`)
    - `CatalogoUsuarios`: Devuelve dos listas (usuarios activos para la tabla, usuarios no activos para el select)
    - `DesactivarUsuario`: Elimina usuario activo
    - `AgregarUsuario`: Agrega usuario activo y devuelve los listados actualizados
  - Todos los métodos usan la cadena de conexión “strConnectionSC”.

**Capa de negocios:**  
- `CN_UsuariosFacturaCancelada.cs`
  - Métodos para enlazar el controlador con la capa de datos.

**Capa de datos:**  
- `CD_UsuariosFacturaCancelada.cs`
  - Realiza las operaciones en la base de datos.
  - Instancia la clase `CD_Datos` y utiliza el método `GenerarSqlCommand('', ref dr, Parametros, Valores)`.
  - Los nombres de los datos resultado deben coincidir con las propiedades de la entidad.

---

### Notas generales

- Utiliza los ejemplos y convenciones del archivo `README.md` para nombrar variables, métodos y elementos HTML.
- Implementa manejo de errores y validaciones tanto en frontend como en backend.
- Comenta el código donde sea relevante para facilitar el mantenimiento.
- Sigue la estructura por capas: entidad, datos, negocio y controlador.
- Prioriza la claridad, mantenibilidad y experiencia de usuario.

---

### Resultado esperado

Una solución funcional, clara y mantenible, que cumpla con los requisitos y mejores prácticas de desarrollo, haciendo énfasis en la estructura, nomenclatura y experiencia de usuario.

---

**Instrucción para Copilot:**  
Genera el código necesario para implementar la solución descrita anteriormente, siguiendo todas las especificaciones y recomendaciones mencionadas.
