# Laboratorio 3 - Desarrollo de ETLs en Google Cloud Platform

## Objetivos

- Entender con un ejercicio práctico los fundamentos y componentes básicos de un proceso ETL.
- Extender y desplegar un proceso ETL encargado de extraer información transaccional para ser cargada en un modelo multidimensional en una bodega de datos.
- Familiarizarse con algunas herramientas del proveedor de nube GCP para el desarrollo y despliegue de ETLs como son Google Storage, Google Cloud DataPrep y DataPrep. 

## Herramientas

- [GCP Storage](https://cloud.google.com/storage?hl=es): Servicio nativo de *data lake* proporcionado por GCP para almacenar datos no estructurados.
- [GCP DataPrep](https://cloud.google.com/dataprep?hl=es): Servicio de datos que permite extraer, limpiar y preparar datos de forma visual.
- [GCP BigQuery](https://cloud.google.com/bigquery?hl=es): Plataforma de analítica de datos provista por GCP que va a desempeñar el rol de bodega de datos *Data warehouse*.

## Importante: Antes de la sesión de laboratorio

Redimir los créditos de forma individual a partir del correo que debe llegarles a sus cuentas. En esta modalidad no se deben generar gastos de tarjeta de crédito.

Tenga en cuenta que el presente laboratorio se debe desarrollar y entregar en grupos. Sin embargo, el acceso a GCP es individual y cada estudiante contará con USD$50 para propósitos de experimentación. Estos créditos son más que suficientes para la realización del laboratorio. Además, deben reservar créditos para desarrollar el proyecto 2.

## Caso de estudio

**WWI (Wide World Importers)** es una empresa distribuidora que ofrece productos variados a clientes retail y naturales al mayor y al detal en diferentes ciudades de Estados Unidos.

Actualmente, la empresa se encuentra contratando un servicio de consultoría de BI con el propósito de construir herramientas analíticas para la toma decisiones basadas en datos. 
Como proyecto piloto, con el objetivo de tener mayor visibilidad de sus procesos de inventario y ventas, WWI desea construir una bodega de datos en donde se incluya información histórica de las órdenes, con el detalle de los productos y tipos de empaque, agregadas por día. Adicionalmente, para WWI también es importante conocer cuanto generan las órdenes por dia en *Valor Bruto de la Mercancía (GMV)* .

Dado lo anterior, WWI contrata a su equipo de trabajo para que realice la implementación y despliegue de una bodega de datos y un proceso ETL que cumpla con el requerimiento planteado.

Más detalles del caso de estudio se pueden encontrar [aquí](https://learn.microsoft.com/en-us/sql/samples/wide-world-importers-what-is?view=sql-server-ver16).

## Arquitectura de la solución

Como requerimiento por parte de WWI, la solución debe ser desplegada en GCP ya que es su proveedor principal de infraestructura en la nube. 
El área de datos de la empresa le ha proporcionado un conjunto de archivos en formato CSV los cuales contienen la información a ser utilizada durante este proyecto piloto. 
Su equipo deberá cargar estos archivos al *data lake* (Storage), usando el servicio para realizar procesos de ETL (DataPrep), en este proceso debe leer los archivos para posteriormente transformarlos y cargarlos, a la bodega de datos (BigQuery) usando un esquema estrella (modelo multidimensional)como se muestra en la figura 1.

<div>
    <img style="display:block; margin:auto;" width="100%" src="202410/Laboratorio%203%20-%20ETL/images/architecture.png">
    <p>Figura 1. Arquitectura de la solución.</p>
</div>

## Actividades
**Etapa 1. Carga de datos a Google Cloud Storage e inicializacion de un conjunto de datos en BigQuery**

1.1 Descargar los datos utilizados en el laboratorio, los cuales encuentra disponibles en este [enlace](data/). 

1.2 Seguir las instrucciones básicas dadas en este [video](https://youtu.be/zmZxqjpo5UY) para subir los archivos a Google Cloud Storage. 

1.3 Seguir las instrucciones básicas dadas en este [video](https://youtu.be/5veRi0fu8Wo) para inicializar un conjunto de datos en BigQuery. 

**Etapa 2. Configurar el flujo creado en el archivo JSON**

2.1 Abrir DataPrep en GoogleCloud Platform desde el proyecto en el cual estás realizando el laboratorio (Debes ser redirigido a la página de TRIFACTA que es el proveedor de este servicio para GCP).
<div>
    <img style="display:block; margin:auto;" width="100%" src="202410/Laboratorio%203%20-%20ETL/images/dataPrep.png">
    <p>Figura 1.a. DataPrep.</p>
</div> 

2.2 En el menú lateral como se muestra en la figura que se presenta a continuación, seleccione la pestaña **Flows**. 
<div>
    <img style="display:block; margin:auto;" width="100%" src="202410/Laboratorio%203%20-%20ETL/images/flujoSel.png">
    <p>Figura 1.b. Selección de flujos.</p>
</div> 

2.3 Dar clic en el botón ImportFlow para importar el flujo compartido en el archivo JSON. Utilice la opción *ChooseFiles* o *Drag and Drop*
<div>
    <img style="display:block; margin:auto;" width="100%" src="202410/Laboratorio%203%20-%20ETL/images/importFlow.png">
    <p>Figura 1.c. Importar flujo.</p>
</div>

2.4 Abrir el flujo importado que lo podrá ver inmediatamente en el despliegue de los flujos y realice las siguientes modificaciones

    2.4.1 Actualizar la ubicación y el nombre de los CSV. Hacer clic en el nodo "Extract" y en la ventana derecha seleccione los tres puntos del nodo y la opción "Replace with dataset with parameters"
<div>
    <img style="display:block; margin:auto;" width="100%" src="202410/Laboratorio%203%20-%20ETL/images/ReplacePar.png">
    <p>Figura 1.d. Opción Replace.</p>
</div> 
    2.4.2 Borre el "path" definido y reemplazarlo por la información del "path" correcto respecto a su bucket de GCP storage y al nombre que le asigno al CSV
<div>
    <img style="display:block; margin:auto;" width="100%" src="202410/Laboratorio%203%20-%20ETL/images/actPath.png">
    <p>Figura 1.e. Opción actualizar "path".</p>
</div> 
    2.4.3 Dar clic en el botón "UpdateMatches" como se ve en la figura. 
<div>
    <img style="display:block; margin:auto;" width="100%" src="202410/Laboratorio%203%20-%20ETL/images/updMatch.png">
    <p>Figura 1.f. Opción UpdateMatches.</p>
</div>    
    2.4.4. Una vez se vea el archivo correcto en pantalla, dar clic en el botón Replace.
    2.4.5 Repita este proceso para los diferentes nodos de "Extract" (En total son 3 nodos)

2.5 Actualizar los 3 nodos "Output" 

    2.5.1 Dar clic en el nodo "Output"
    
    2.5.2 Dar clic en la opción de "Manual Settings"
<div>
    <img style="display:block; margin:auto;" width="100%" src="202410/Laboratorio%203%20-%20ETL/images/manualSetting.png">
    <p>Figura 1.g. Opción "Manual Settings".</p>
</div>    
    
    2.5.3 Dar clic en botón "Edit"
    
    2.5.4 En la opción "Publishing in actions" eliminar la acción que viene definida.
<div>
    <img style="display:block; margin:auto;" width="100%" src="202410/Laboratorio%203%20-%20ETL/images/publisAct.png">
    <p>Figura 1.h. Opción publishing Actions.</p>
</div>    

    
    2.5.5 Añadir una nueva acción y crearla seleccionando BigQuery, luego el nombre del conjunto de datos que creó. Dar clic a la opción "Create table". Cambiar el nombre de la tabla, dejar seleccionada la opción "Append to this table every run" y finalmente dar clic en el botón "Add"
<div>
    <img style="display:block; margin:auto;" width="100%" src="202410/Laboratorio%203%20-%20ETL/images/appendTabl.png">
    <p>Figura 1.i. Opción Append.</p>
</div>    

2.4 Correr cada "job" del "Ouput" para verificar que la ejecución sea exitosa en cada nodo.
<div>
    <img style="display:block; margin:auto;" width="100%" src="202410/Laboratorio%203%20-%20ETL/images/runJob.png">
    <p>Figura 1.j. Opción Run Job.</p>
</div>

**Etapa 3. Proceso ETL**

Revisar este [video](https://youtu.be/o4ddY6Ebd3s) sobre el uso de Google Cloud DataPrep para la creación y ejecución de un proceso ETL de base, que se le ha proporcionado a su equipo, el cual puede obtener en este [enlace](flow_lab3bi202401.json5). 
Como podrá observar en la interfaz de administración de DataPrep, el ETL proporcionado corresponde al siguiente:

<div>
    <img style="display:block; margin:auto;" width="100%" src="202410/Laboratorio%203%20-%20ETL/images/etl-schema-top.png">
    <p>Figura 2. Diagrama de la parte superior del proceso ETL.</p>
</div> 
    
<div>
    <img style="display:block; margin:auto;" width="100%" src="202410/Laboratorio%203%20-%20ETL/images/etl-schema-bottom.png">
    <p>Figura 3. Diagrama de la parte inferior del proceso ETL.</p>
</div>
<br />


3.1 Analice los diferentes subprocesos del ETL proporcionado. Identifique los nodos o bloques para la extracción de datos desde Storage, los diferentes filtros, *joins*, agregaciones y generadores de *PKs* que se encuentran implementados, así como los requeridos para la carga de datos a DataPrep. Describa en sus propias palabras el proceso general implementado.

3.2 Extienda el ETL para incluir una nueva dimensión denominada **PackageTypes** utilizando el archivo CSV con el mismo nombre (que debió descargar previamente), más una nueva medida denominada TotalPrice la cual debe ser calculada multiplicando las columnas Quantity y UnitPrice del archivo OrderLines. Adjunte el diagrama de bloques final generado en DataPrep, así como el archivo JSON que se exporta desde la misma interfaz de administración. Documente el diseño de estos procesos ETL, utilizando la plantilla que está disponible en este [enlace](PlantillaDiseñoETL.xlsx).

3.3 Revise las tablas creadas por el proceso ETL. Revise si lo requiere nuevamene el video donde se muestra esta parte. 


***Importante:** Para evitar errores durante la ejecución consecutiva del ETL, se recomienda que elimine manualmente las tablas creatdas utilizando DataPrep antes de realizar una nueva ejecución. 
Recuerde que actividades de cambios en el esquema, no son comunes en la práctica normal y, que en este laboratorio se hizo de esta manera por razones pedagógicas.*


**Etapa 4. Modelo multidimensional y análisis**

4.1 Reconstruya el modelo multidimensional que se genera en DataPrep tras la correcta ejecución del ETL con un ejercicio de ingeniería inversa. 

4.2 Diseñe y documente el resultado de tres consultas SQL que tengan sentido para WWI (*Business questions*) y que involucren las medidas implementadas filtrando por algunas de las diferente dimensiones disponibles.

4.3 Finalmente, responda las siguientes preguntas:
- ¿Qué diferencia existe entre una arquitectura *Data warehouse* y un *Data lakehouse*? ¿Qué tipo de arquitectura le recomendaría a WWI para complementar lo desarrollado en este laboratorio incluyendo fuentes de datos no estructuradas, análisis en tiempo real que incluyan simultáneamente tanto datos estructurados como no estructurados?
- ¿Qué ventajas y desventajas observa al momento de implementar un ETL utilizando este tipo de herramientas respecto a desarrollarlo utilizando Python, Pandas y demás herramientas vistas durante la primera parte del curso?
- ¿Qué tipo de esquema, estrella o copo de nieve, representa el modelo multidimensional construido en este laboratorio? Justifica tu respuesta.
- ¿Qué tipo de tablas de hechos y de medidas se identifican en el modelo multidimensional dado? Justifica tu respuesta.
- Suponga que la dimensión StockItemDim cambia el manejo de la historia de tamaño y precio del producto a un tipo 2 (*Slow Change Dimension*). ¿Qué ajustes a la dimensión relacionada con el producto, a la tabla de hechos y al proceso ETL se deben realizar para que al cargar la información se incluya este manejo de historia.
- ¿Qué ajustes al proceso ETL construido en este laboratorio hay que realizar para cargar nueva información que sea reportada por WWI. Esto se considera en la literatura una carga incremental?
- ¿Qué errores se le presentaron en el desarrollo del laboratorio y qué solución plantearon? Haga énfasis en los que fueron más difíciles de solucionar.

## Instrucciones de Entrega

- La entrega se debe realizar en grupos de mínimo 2 estudiantes y máximo 3.
- Recuerde hacer la entrega por la sección unificada en Bloque Neón, antes del sábado 11 de mayo a las 20:00. Este será el único medio por el cual se recibirán entregas.

## Entregables

- Diseño del proceso ETL, utilizando esta [plantilla](PlantillaDiseñoETL.xlsx), de todo lo realizado en este laboratorio.
- Descripción con sus propias palabras del paso a paso del proceso ETL final. 
- Implementación del proceso ETL generado en DataPrep, así como el archivo JSON que se exporta desde la misma interfaz de DataPrep.
- Modelo multidimensional que representa lo que se carga en el ETL trabajado en este laboratorio. 
- Consultas SQL utilizadas para obtener la información solicitada en la actividad 3.2 y evidencia de su correcta ejecución desde la interfaz gráfica de BigQuery.
- Respuestas a las preguntas planteadas.
- Video descriptivo de la ejecución del proceso ETL.

## Rúbrica de Calificación

| Concepto | Porcentaje |
|:---:|:---:|
| **Estudiante 1:** Diseño del ETL final (el proceso original + la extensión) | 15% |
| **Estudiante 2:** Implementación del proceso de ETL final en DataPrep y los archivos asociados - Diagrama y JSON del ETL final | 25% |
| **Estudiante 3:** Descripción del paso a paso del ETL final y diagrama y descripción del Modelo multidimensional| 20% |
| Video de máximo 3 minutos mostrando la correcta ejecución de los trabajos (*job*) creados en DataPrep para el proceso del ETL final| 10% |
| Consultas SQL y evidencia de su correcta ejecución | 15% |
| Respuestas a las preguntas | 15% |


## Comentarios y recomendaciones
- Buckets y Bigquery se cobra por mes y por almacenamiento
- DataPrep se cobra al ejecutar las tareas. Dado que el proceso ETL se ejecuta de forma manual y no está automatizado para que se ejecute frecuentemente, no debe existir problemas con la cantidad de créditos compartidos para el desarrollo de este laboratorio y del proyecto.
- El Json debe ser editado desde la herramienta DataPrep. No puede ser ajustado de forma manual.
