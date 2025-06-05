## Objetivo
Ayúdame a crear una solución para consultar facturas canceladas a través de una API. Utiliza el archivo `README.md` como referencia para seguir los estándares de estilo y buenas prácticas de codificación. Detallo a continuación los requisitos por capa y archivo.

---

### Frontend

**Archivo principal:** `CapFacturaCancelada.aspx` (usa la MasterPage `MasterPage01_bootstrap`).

#### Requerimientos HTML y CSS
- Integra las siguientes librerías: **Bootstrap, jQuery, Alertify, DataTable**.
- Usa estilos Bootstrap en todos los elementos HTML.
- Agrega un `div` contenedor con un botón **"Descargar en Excel"** (color azul, alineado a la derecha).
- Agrega una tabla con id `tblFacturas` con las siguientes columnas:
  - `"RFC Receptor"`, `"Razón Social"`, `"Serie"`, `"Folio"`, `"FolioFiscal"`, `"Fecha de emisión"`, `"Fecha de Sol. Canc."`, `"Tipo Documento"`, `"Estado SAT"`, `"Subtotal"`, `"IVA"`, `"Total"`, `"Folio Relacionado"`, `"Serie Relacionada"`, `"Folio Fiscal Relacionado"`, `"Tipo Documento Relacionado"`.
- Implementa funcionalidad de **Loading**: incluye el código CSS y los elementos HTML necesarios.

#### Requerimientos JavaScript
- **Archivo:** `SIANWEB/js/Facturas/CapFacturaCancelada.js`
- Estructura el código JavaScript como **singleton**.
- Utiliza Alertify para mensajes de alerta, validación y error.
- Usa la librería DataTable para la tabla `tblFacturas`.
- Declara una variable global para la URL base del API (`ApplicationUrl`).
- Realiza llamadas al API usando **fetch** con rutas formadas por `ApplicationUrl + controlador + método`.
- Al cargar la página, inicializa los elementos y llama a `ConsultarFacturasCanceladas` para llenar la tabla.
- Implementa el evento click del botón "Descargar en Excel", que consulta el API (`DescargarReporte`) y descarga el archivo Excel en el navegador.

---

### Backend

#### Capa Entidad

**Archivo:** `entFacturaCancelada.cs`
- Crea la entidad con las propiedades:
  - `strRfcReceptor`, `strRazonSocial`, `strSerie`, `intFolio`, `strFolioFiscal`, `dtFechaEmision`, `dtFechaSolCanc`, `strTipoDocumento`, `strEstadoSAT`, `decSubtotal`, `decIVA`, `decTotal`, `intFolioRelacionado`, `strSerieRelacionada`, `strFolioFiscalRelacionado`, `strTipoDocumentoRelacionado`.

#### Capa Controlador

**Archivo:** `capFacturaCanceladaController.cs`
- Métodos:
  - `ConsultarFacturasCanceladas`: sin parámetros, retorna un objeto con dos listas de la entidad: `lstTotalFacturaCancelada` y `lstFacturaCancelada`. Utiliza los parámetros `Emp_Cnx`, `Id_Cd`, `ref lstTotalFacturaCancelada`, `ref lstFacturaCancelada`.
  - `DescargarReporte`: también consulta las facturas canceladas y genera un archivo Excel usando **ClosedXML.Excel** con los encabezados en la fila 3, columna A, formato filtro. Ordena `lstTotalFacturaCancelada` por `decTotal` descendente. Llena los datos usando dos foreach anidados: el primero para `lstTotalFacturaCancelada`, el segundo para `lstFacturaCancelada` filtrando por `strRfcReceptor`.
- Ambos métodos retornan `HttpResponseMessage`.
- Agrega manejo de excepciones en el código C#.

#### Capa Negocio

**Archivo:** `CN_FacturaCancelada.cs`
- Método: `ConsultarFacturasCanceladas`.

#### Capa Datos

**Archivo:** `CD_FacturaCancelada.cs`
- Método: `ConsultarFacturasCanceladas`.
- Dentro, instancia la clase `CD_Datos` y utiliza el método `GenerarSqlCommand('spFacturasCanceladas', ref dr, Parametros, Valores)`.
- Los nombres de los datos resultado deben coincidir con las propiedades de la entidad.

---

### Notas generales
- Usa los ejemplos y las convenciones de estilo del archivo `README.md` para nombrar variables y elementos HTML.
- Implementa correctamente el manejo de errores y validaciones.
- Comenta el código donde sea relevante.
- Sigue la estructura por capas: entidad, datos, negocio y controlador.

---

#### Resultado esperado:
Una solución funcional, clara y mantenible que siga las mejores prácticas de desarrollo, con especial atención a la estructura, nomenclatura y experiencia de usuario.
