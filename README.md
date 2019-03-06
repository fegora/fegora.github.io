## Descripción de estructura (JSON) a enviar

### Dte (Documento Tributario Electrónico)
| Campo | Descripción | Obligatorio | Valor por defecto | Ejemplo (s) |
| ------ | ------ | :------: | :------: | ------ |
| `tipo` | Tipo de documento a crear. Valores posibles:  `factura`, `facturaEspecial`, `notaCredito`, `notaDebito`, `notaAbono`, `recibo`, `reciboDonacion`. *No se hace distinción entre mayúsculas y minúsculas*  | NO | `factura` | `"tipo" : "facturaEspecial"` |
| `moneda` | Moneda del documento. Valores posibles: código existente en el listado oficial [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217#Active_codes). La moneda también debe ser soportada por el API en cuanto al mantenimiento de su tipo de cambio. Actualmente soportamos sólamente `GTQ` (Quetzales) y `USD` (dólares). *El valor debe ir en letras MAYÚSCULAS*  | NO | `GTQ` | `"moneda" : "GTQ"` |
| `fechaEmision` | Fecha de emisión del documento.  | NO | fecha y hora actual |  `"fecha" : "2019-03-06"`, `"fecha" : "2019-03-06T08:22"`, `"fecha" : "2019-03-06T08:22:26"` |
| `idDteOriginal` | Identificador del documento original (emitido anteriormente) al que hace referencia este documento. Este campo es **requerido** sólamente cuando el campo `tipo` es: `notaCredito` o `notaDebito`  | **SI** (cuando `tipo` es: `notaCredito` o `notaDebito`)  |  | `"idDteOriginal" : "A3F92542-FB03-47C4-BFC4-FC1BF27E93BD"` |
| `items[]` | Listado de items (detalle de productos) del documento. **Debe haber por lo menos un item** en el documento. Más adelante se describe la estructura de un item. | **SI**  | | `"items" : [ {...}]` |

 ### Item
A continuación se describe la estructura de un *item*. Los items son los objetos que se encuentran en el nodo `items` del DTE.

| Campo | Descripción | Obligatorio | Valor por defecto | Ejemplo (s) |
| ------ | ------ | :------: | :------: | ------ |
| `tipo` | Tipo del item. Puede ser `"bien"` o `"servicio"`. | NO  | `"bien"` *(1)* | `"tipo" : "servicio"` |
| `unidadMedida` | Unidad de medida que aplica a la `cantidad` del bien o servicio. Se recomienda utilizar una de las siguientes: `"UNI"`(unidad), `"MTS"`(metros), `"MT2"`(metros cuadrados), `"MT3"`(metros cúbicos), `"KGS"`(kilogramos), `"LTS"`(litros), `"PAR"`(pares), `"CBZ"`(cabezas), `"PZA"`(piezas), `"KWT"`(kilovatios), `"JGO"`(juego), `"BAR"`(barriles). *Si desea incluir una unidad personalizada lo puede hacer, pero ésta debe tener una longitud máxima de **3** caracteres* | NO  | `"UNI"` *(1)* | `"unidadMedida" : "servicio"` |
| `cantidad` | Cantidad de productos o servicios prestados. | NO  | 1 | `"cantidad" : 1` |
| `descripcion` | Descripción del producto o servicio. | **SI**  |  | `"descripcion" : "Reparación y mantenimiento de motor"` |
| `precioUnitario` | Precio del bien o servicio. Este valor **debe incluir IVA** (*salvo excepciones explicadas más adelante*). Si incluye el campo `descuento`, el `precioUnitario` **no** debe incluir la aplicación del descuento. Estos cálculos se realizan automáticamente por el API. | **SI**  |  | `"precioUnitario" : 500` |
| `descuento` | Descuento **discreto** (fijo, monto en moneda, no en porcentaje) a aplicar al bien o servicio. *Si desea incluir un porcentaje en el descuento, debe calcularlo en su sistema y pasar en este campo el resultado discreto.* | NO  | 0 | `"descuento" : 40` |
| `datosAdicionales[]` | Listado de datos adicionales que desee incluir en un item. Más adelante se describe la estructura de un dato adicional. | NO | | `"datoAdicionales": [{...}]` |

*(1): Este "valor por defecto" también se puede configurar en su cuenta de emisor.*

### Dato Adicional
A continuación se describe la estructura de un *datoAdicional*. Los datosAdicionales son los objetos que se encuentran en el nodo `datosAdiciones` del DTE o bien de un ITEM en particular.

| Campo | Descripción | Obligatorio | Valor por defecto | Ejemplo (s) |
| ------ | ------ | :------: | :------: | ------ |
| `nombre` | Nombre del dato adicional. Este nombre es utilizado por usted o por Fegora para identificar o encontrar el valor. | **SI**  |  | `"nombre" : "IdProductoSAP"` |
| `valor` | Valor del dato adicional que desea almacenar para el DTE o ITEM. | **SI**  |  | `"valor" : "GTR24-123-4"` |
| `etiqueta` | Etiqueta que desee que se muestre eventualmente en una representación gráfica (pdf, impresión). | NO  |  | `"etiqueta" : "Código de Producto SAP"` |
| `visible` | Indica si este dato adicional debe ser visible en una representación gráfica (pdf, impresión). | NO  | `false` | `"visible" : true` |
