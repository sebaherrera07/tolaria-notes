---
type: INGEA
_organized: true
---
# Reunión Liquidación de Servicios

#### Adición de Atributos a Nivel de Proyecto y Reporte

```text
- Un issue recién creado (versión 0.5.76) requiere agregar el nombre del cliente y del proyecto como atributos tanto a nivel de proyecto como de reporte.
- Al generar un proyecto, se debe asociar un código de proyecto y un código de cliente, siendo el nombre del proyecto generalmente consistente aunque puede variar en algunos casos.
```

#### Eliminación del Punto en la Liquidación de Servicio

```text
- En la liquidación de servicio, se decidió eliminar el uso del punto como separador o identificador.
- Las liquidaciones ya cargadas deberán revisarse, pero como medida inmediata se acordó que el punto deje de visualizarse para evitar que se continúe utilizando.
```

#### Estructura de la Liquidación de Servicio con Dos Documentos

```text
- La liquidación de servicio se compone de dos documentos diferenciados: una liquidación comercial y una liquidación técnica.
- Ambos documentos están destinados a personas distintas dentro del proceso de aprobación y envío al cliente.
```

#### Formato Actual del Reporte Técnico y sus Limitaciones

```text
- El reporte técnico se genera descargando un archivo desde el módulo de reporte de horas aplicando filtros de mes y proyecto.
- El formato actual del archivo descargado no corresponde al formato que los clientes solicitan, ya que estos requieren la información ordenada y presentada por persona.
- Actualmente el archivo presenta filas con diferencias de color aplicadas manualmente, lo que implica intervención humana y genera riesgo de error.
```

#### Necesidad de Automatizar el Formato del Reporte Técnico

```text
- El objetivo principal es eliminar la intervención humana en la generación del reporte técnico, evitando la necesidad de copiar, pegar y dar formato manualmente.
- Lo que se requiere es que el software genere automáticamente una tabla dinámica con un resumen por persona y por servicio, indicando la cantidad de horas, junto con el detalle correspondiente.
- Una carátula con datos fijos como cliente, proyecto, nombre del documento, descripción, fechas de inicio y fin, y destinatarios de distribución también debe incluirse en el documento generado.
```

#### Flujo de Creación de una Liquidación de Servicio en el Sistema

```text
- El flujo propuesto consiste en que Sebastian ingrese al módulo de administración, acceda a liquidaciones de servicio y cree una nueva liquidación ingresando los datos macro como fecha, cliente y concepto.
- Al inicializar la liquidación, el sistema debería permitir aplicar los mismos filtros del reporte técnico y generar el archivo con el nuevo formato directamente desde esa instancia.
- Se propuso manejar estados para la liquidación, comenzando con un estado "inicializado" y avanzando hacia estados como "pendiente" o "lista para enviar" a medida que se completan los datos.
- Campos como total de horas productivas y montos no deberían ser obligatorios al momento de la creación, ya que estos datos se conocen recién después de descargar y validar los reportes.
```

#### Corrección del Número de Servicio en el Reporte

```text
- En el reporte técnico se identificó que el número de servicio aparece con el sufijo "guión 1" en todos los casos, cuando debería mostrar el código correcto del proyecto, por ejemplo "guión 389".
- Este dato debería derivarse del código del proyecto asociado a la liquidación.
```

#### Estructura y Contenido del Reporte Comercial

```text
- El reporte comercial es más complejo que el técnico e incluye un resumen por colaborador con tarifa, un resumen por disciplina o especialidad de servicio, y una hoja por contrato segregada por etapa del proyecto.
- Las etapas que el cliente solicita desglosar incluyen proyecto, ingeniería, compras, construcción, comisionamiento, puesta en marcha, mantenimiento y operación.
- Además de horas, el reporte comercial puede incluir conceptos como traslados u otros ítems unitarios dependiendo del acuerdo contractual, los cuales deben tener una tarifa asociada en el sistema.
```

#### Factor de Rendimiento en la Liquidación Comercial

```text
- Existe un factor de rendimiento que no siempre es del 100%, aplicable cuando una tarea tomó más tiempo del esperado.
- Este factor es definido por quien aprueba técnicamente la liquidación y actualmente se aplica de forma manual fuera del sistema.
- Se reconoció que este campo es subjetivo y que las reglas pueden cambiar, por lo que su incorporación al sistema requiere que las reglas estén claramente definidas antes de automatizarlo.
```

#### Próximos Pasos

```text
- Se acordó avanzar en la implementación del nuevo formato del reporte técnico como prioridad inmediata, con inicio de trabajo previsto para el día siguiente.
- Se reconoció que dado que la reunión se realizó el 20 de abril y el fin de mes está próximo, es posible que no todos los cambios queden completados, por lo que se irá evaluando el avance.
- El objetivo central a alcanzar es eliminar la intervención manual en la generación de liquidaciones de servicio para reducir el riesgo de error humano.
```

***

Agustin | 00:23\
Vamos a la revísima. Che, te comparto acá. Necesito... Ya recin subí un issue... Necesito agregar de acá el viernes es una boludez igual y de cliente.. De proyecto, hice un proyecto y nombre Al En realidad esto es atributo a nivel de proyecto.\
Sebastian | 00:44\
Reporte.\
Agustin | 00:49\
Al reporte tambin Gracias a todos. Es el issue que recin se creó de 0.5.76.\
Sebastian | 00:59\
Adiós.\
Agustin | 01:01\
¿La oíste? Sí.\
Sebastian | 01:03\
Para el nombre identificado de cada proyecto. Por eso, donde... Queda claro.\
Agustin | 01:10\
Cuando yo creo el proyecto, así como genero el código del proyecto... Se llama Lingea y el nombre del proyecto y que Tengo que generarle un código de proyecto. Del cliente eso tambin tiene un código.\
Sebastian | 01:25\
Í va Porque el.\
Agustin | 01:28\
Que no es el mismo TAF, porque todo eso. Y nombre tambin El nombre debería ser siempre el mismo.\
Pero está, a veces puede cambiar. Eso por un lado, es una boludez y es rápido, pero eso... Te comparto ...lo que sí, lo que sí, bien hace tiempo que lo... Lo venimos hablando y no... Y no lo me chambo. Es el tema de las liquidaciones. Yo hoy... Mira. Acá tengo una liquidación de un servicio.\
Sí, exacto. Lo pongo hoy a ojer en la liquidación de servicio. No usamos más punto.\
Sí, se si podemos eliminarlo Buenísimo y todas las otras vemos que hacemos.\
Sebastian | 02:10\
Puede sacar.\
Agustin | 02:17\
Todas las cargadas las tenemos que mirar, pero por lo menos que no se visualice más. Así no se carga más nadaí. Y le agregamos... Dos documentos, ¿tá? Estos dos documentos Es una liguización comercial y una liguización tcnica. ¿Verdad? La liquidación tcnica. Lo que hace ¿Cartan Excel? Gracias... Agarra Ahora. Entiendo que... Un segundo, voy a la... Aleksandr.\
¿Rando?\
¿? La que hace esto...\
Sí. ...\
Gracias.\
Esto lo tengo que presentar así. No sé cómo estamos... Yo voy acá y voy al reporte de horas.\
Pongo los filtros. ¿Y el mes? Descargo archivo.\
Sebastian | 04:12\
Sí.\
Agustin | 04:17\
Y que descargó tiene que ser Me la muestro así. ¿Va? Gracias. Esto así como está... Y me muestra todo esto que...\
Esto en realidad... No debería. Lo pusimos... Yo ¿no?\
Sebastian | 04:44\
Creo que hay otro reporte, porque está el reporte de horas, pero hay otro que ya te incluye las tarifas y eso, el tcnico.\
Agustin | 04:51\
No, pero ese es el comercial. Ese.\
Claro. Está el comercial y está el... Nosotros tenemos los dos archivos porque es para personas diferentes Entonces, Ese... Se descargó. Y genera. Archivo que tiene esto así. No es como lo piden los clientes al tcnico. Los clientes me piden...\
Mostrármelo.\
Mostrármelo así Por persona.\
Sebastian | 05:46\
¿Y no está ahora ordenado por persona?\
Agustin | 05:51\
Hoy. Sí, no...\
Sebastian | 05:57\
No tiene la separación de color y eso está obviamente, pero no está ordenado ya por persona.\
Agustin | 06:02\
Ordenado, sí hoy está ordenado por Saní y por Díaz Oye.\
Sebastian | 06:05\
Chau.\
Agustin | 06:12\
La edición no sé si está ordenado por proyecto sí, tambin está ordenado ¿Pero está?.. Malo Gracias. ¿Qué es lo que se llama?\
No sé si... Y no se hace fácil.\
Sebastian | 06:47\
Gracias.\
Agustin | 06:58\
Y esto es tcnico. Presento servicio Acá quedó que errar el número de servicio, el guión 1 ese creo que quedó mal.\
Sebastian | 07:14\
Número 1.\
Agustin | 07:19\
El número de servicio... Sí. En todos está guión 1. Y entiendo que debería ser guión 389.\
Sebastian | 07:30\
Pero eso es de donde sale, eso no... Gracias.\
Agustin | 07:38\
El colectiviente ya está no, pero es el código del cliente. Gracias. Podía proyecto.\
Con ese proyecto...\
Siente... Sí, más. Acá no, hombre. Saludos. El código no me interesa. De ti. A verle que para que esté igual Es simplemente... Nombre del proyecto.\
Despus fecha. Chao. Adiós.\
Y despus la extorsión. Es esto, ¿no?\
Sebastian | 08:59\
Sí, parece ser.\
Agustin | 09:01\
Hola, para el Teo Sumar Provertís. Gracias.\
Adiós.\
Ahora me fijo bien, pero creo que.\
Sebastian | 09:36\
Hasta ahora lo que quers lograr es como que a partir del reporte de horas...\
Agustin | 09:41\
Yo lo que quiero es Yo lo quiero eliminar.\
Sebastian | 09:43\
El límite.\
Agustin | 09:46\
Sí. Sí, la información está... Yo tengo que eliminar el uso humano acá Eso... Que no No, tengo que descargar.\
Sebastian | 09:54\
Tenga que ir a la persona a copiar, pegar...\
Agustin | 10:00\
Sí, te voy a declarar y enviar. No. No puede haber una persona... Le ramo blancos.\
Sebastian | 10:08\
Sí, obvio. Porque hay unos celestes y otros está.\
Agustin | 10:13\
Porque los modificamos. Celeste fue una modificación a la mano.\
Sebastian | 10:17\
Pero eso no es problema del programa. No.\
Agustin | 10:22\
Sí, correcto.\
Sebastian | 10:24\
¿?\
Agustin | 10:26\
Igual esto así como lo veo no debería ser más que hacer esta tabla dinámica no puedo copiar y pegar pero es capaz que darle formato, pero está... Prácticamente es eso. En esto de la I... No lo había intentado hacer dinámico.\
Pero hasta acá entiendo que sería solamente esa dinámica. Y queda si lo hace el software, lo que yo necesito es software Acá lo que me hace una de resumen. Queí me filtra por persona por de servicio La cantidad de horas que tuvo, es un resumen... Chamale, es un resumen. Es el detalle y es el resumen.\
Sebastian | 11:11\
Claro, sí.\
Agustin | 11:12\
¿Verdad? Y esta es una carátula. Que tiene datos que a ver, todo esto da todo eso los tens, cliente, proyecto, nombre documento fijo, descripción tambin fijo, inicio, fin lo tenemos. Y del documento, se lo asignamos cuando generamos el documento, a quien se distribuye tambin. Esto es una... Es el concepto de carátula que lo tenemos en todos los documentos. Salud.\
Pero no crítico. Eso, yo lo que necesito es... Estas dos pestañas se va a descargar y que queden así Miren.\
Sebastian | 11:48\
Que Eso para un paso antes.\
Agustin | 11:52\
Acá hay un factor de rendimiento.\
Sebastian | 11:58\
Eso sería... Vos estás en la web. Así es. ¿qué vas a hacer? Apretar nueva liquidación de servicios. Ponele. ¿En qué momento vas a bajar esto?\
Agustin | 12:12\
Lo ideal sería si en el momento cuando voy a hacer una liquidación de servicio. El reporte tcnico que ya está... Está bien, dejmoslo como está.\
Si quers usar lo que puedas usar. Pero no... Eeeh Es antes de que, en.\
Sebastian | 12:30\
Realidad... Vos vas a primero bajar el archivo ese y despus vas a crear una liquidación de servicios.\
Agustin | 12:39\
En realidad hoy por hoy a mí lo que me gustaría que hagamos es, yo vengo acá de administración, clientes liquidaciones servicios clientes Nueva liquidación de servicio. Y acá le pongo los datos macro ¿Está?\
Sebastian | 12:57\
Sí.\
Agustin | 12:59\
Y me encantaría poder ponerle los filtros que le pongo al reporte tcnico acá a reporte de detalles tcnico Sí. Y que genere el ... Tener el... El reporte ese descargar con ese cambio de formato Porque reporte es en cualquier momento. Reporte puede ser... El que está en reporte tcnico, yo...\
Si alguien me pide cómo venimos con el avance de tal cosa cosas, descargo esto y se lo mando.\
Sebastian | 13:36\
O.\
Agustin | 13:41\
No es una liquidación de servicios. Es un reporte de horas como venimos con el proyecto.\
Pero sí tiene carácter de liquidación de servicio en donde yo... Tambin estoy teniendo problemas a veces de que nos generamos, no venimos acá y hacemos esto.\
Sebastian | 13:58\
Í lo que me queda la duda es, porqueí te está pidiendo ponerle total de horas productivas. Montos, monedas, no sé qué. Todo eso en realidad vos no lo sabs hasta... Chau, Erick. He sacado el reporte. Comercial y tcnico y a ver. Validado, tantas horas, tanta plata.\
Agustin | 14:19\
Pero tal, que no me lo pida como obligatorio.\
Sebastian | 14:23\
Claro, capaz que sean dos pasos. Pods crear una liquidación de servicio que está como pendiente. Y luego cuando la vayas a pasar a aceptar o pronta para enviar o como le queramos.\
Agustin | 14:37\
Llamar. Acá tenemos un estado tcnico. Que yo ahora voy a si quiero liquidar nueva liquidación genero Le pongo ya, acá ya puse la fecha del reporte, marzo. Concepto de guiación que sea liquidación en marzo.. Ac, Y esto estaba mal. ¿Te acordás que hubo un issue que yo puse que en realidad esto va todo junto? Son diferentes pestañas de la liquidación. Adiós. Sentimos Esto en realidad sería... Acá vamos a ponerle otro, porque no Esto quedó vacío. Esto es así, esto todavía no tengo datos, evidentemente... Esto es la alineación de servicios, no es la factura... Estado tcnico Acá tenemos el estado, entonces ponerle que ahora... En vez de ser pendiente, el estado ese que vos decís es inicializado. ...el estado administrativo... ...pendiente... Comentario.\
Sin comentario. Y no le agrega ningún archivo y pongo guardarí yo inicialicé la liquidación. No sé por qué. Puse más. No. Ya inicialicé la liquidación. Ahora tengo que entrar a generar, descargar reportes. Acá si quers que Sí.\
Sebastian | 16:10\
Te baje el reporte ya con los filtros aplicados Ya te lo bajé con el formato nuevo con el que estábamos hablando, base parecida al...\
Agustin | 16:17\
Ya acá tengo la fecha ya tengo el cliente.\
Sebastian | 16:27\
Al otro reporte, pero ya con el formato correcto.\
Agustin | 16:31\
Sí.\
Sebastian | 16:32\
Correcto. Bien, ta.\
Agustin | 16:35\
Entiendo. Eso, ya estamos a la hora. Eso es la liquidación tcnica. Y agregarle tal... El idea del... Del bien.\
Sebastian | 16:46\
Bien, del lado del cliente.\
Agustin | 16:49\
Claro, pero eso va a estar todo ya, si lo agregamos ya cuando se inicializa, se hace esto. ...ya se tiene ese dato...\
Sebastian | 16:56\
Sí. Chao.\
Agustin | 16:58\
Lo tomamosí. Sí. Eso y despus la comercial es la más... Hasta más. La que sí creo que tiene un poquito más de magia. En esta, el rendimiento, nosotros no siempre vamos al 100%. Acá hay veces que decimos, hicimos las cosas mucho más tiempo del que debería haber llevado. Y tenemos un momento en donde... Le...\
Pero esto hoy no está en el sistema. Claro, Y nada, sí.\
Sebastian | 17:30\
Esto es a mano. Sí.\
Agustin | 17:37\
A priori no pasa nada que sea mano. ¿sabes?..\
Sebastian | 17:42\
Y no porque por lo que me decís es como muy subjetivo a menos que esté las reglas bien claras, pero las reglas imagino que pueden cambiar.\
Agustin | 17:51\
En la aprobación tcnica esto lo tiene que definir el que aprueba tcnicamente esa liquidación tcnica. El que aprueba tcnicamente dice..... Va a ser, esto tiene que estar en el sistema lo que te decía es que no frenar ahora generar esta modificación para que no intervenga la mano de nadie. Por agregar atributo En... Esta funcionalidad. ¿sabes? Para allá, ya sé que tienes que ir, última pregunta Introduzco la comercial. Y la comercial... Agarramos el reporte que surge hoy comercial que es que sigue la si correcto tiene eso y De eso hacemos estos...\
Sebastian | 18:30\
Tarifa o algo así Bien.\
Agustin | 18:39\
Resumen que lo que te hace es el mismo resumen tcnico pero comercial Chao. Colaborador, por...\
Despus resumen de servicio.í es por disciplina. ¿Va? Ya no tanto de nada, ni de nada. No con nombre y apellido, sino con... Con disciplina y acá es La tapa. En esta etapa... Lo que tenemos es una hoja por contratos Segregado por etapa. Qudate.\
Sebastian | 19:22\
Sí, no metan hilo de una hoja.\
Agustin | 19:25\
Acá viste lo de acá. Es una pestaña, pero está partido en hoja.í va. Para no llenar las pestañas.\
Pero esto está... Por su proyecto nos pide el cliente que le digamos vos ¿Cuánto fue en proyecto? ¿Cuánto fue en ingeniería? ¿Cuánto fue en compras? ¿Cuánto fue en construcción? ¿Cuánto fue en comisionamiento? Cuánto fuente de marcha cuánto mantenimiento y cuánto de operación Por... Tu proyecto Esta es la que tiene más cambios, toma de esto Es fácil, pero tal.\
Sebastian | 20:08\
Es tomar hacer un resumen por persona y por tarifa, me imagino que es esto, algo así. Sí.\
Agustin | 20:16\
Correcto, esto es un resumen por... Sí.\
Sebastian | 20:18\
Y la otra es por etapa.\
Agustin | 20:22\
Estas por servicio esta es por persona ¿Estás por servicio?\
Sebastian | 20:28\
Por servicio... Servicio que sería...\
Agustin | 20:32\
Y ingeniería en automotriz. Iván. Fue tanto.\
Sebastian | 20:37\
Bien. No, es como por...\
Agustin | 20:40\
Sí, por disciplina y especialidad.\
Sebastian | 20:44\
Sí, no me acuerdo cómo le llamamos en el sistema, pero sí, en algún lado está.\
Agustin | 20:50\
Es por de servicio. Acá tambin a veces tenemos traslados, a veces tenemos... Depende del acuerdo que se tenga. Eventualmente en ese contrato cómo se maneja, es servicio, no es horas, eso es lo que yo quiero. Quiero que no hemos podido hacer el cambio, pero acá puede ser unitario 000010 Igual cuando te pongas a hacerlo, revisás y resuelvelo. Acá es, no sé, por ejemplo... Y nosotros ya tendríamos que tener una tarifa asociada Sí, men. Traslado Por ejemplo. Bye, Yagal. 5 esto es lo que tenemos que... Yo necesito sacar esto, que mes ya salga así.\
Sí podemos.\
Sebastian | 21:55\
Porque estábamos Y arranco con eso mañana capaz.\
Agustin | 21:56\
Teniendo varios errores Te...\
Sebastian | 22:06\
Dale. Hay que verte y llegamos a fin de mes. Ya estamos a 20 y... Chao. Capaz que algunos avances logramos, pero no sé si vamos a llegar con todo. Vamos viendo.\
Agustin | 22:18\
Dale, vamos viendo. Yo lo que necesito es eliminar el errorí humano que... Que es muy delicado, lo derramo acá y... Y laburamos el PEDE.\
Sebastian | 22:28\
Perfecto. ¿sá? Dale. Hablamos.
