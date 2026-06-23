---
type: AirOps
_organized: true
---
# Project - Feedback Loop From Comments

<https://staging.airops.com/1ma-0/grids/221/sheets/242>

## Outline

#### Alcance de la Fase 1: Procesamiento de Comentarios en Playbooks

- Definida la Fase 1 como la incorporación de comentarios al proceso actual del Feedback Loop para mejorar el Brankit.
- El objetivo inicial era traer los comentarios de los artefactos de los playbooks y utilizarlos para actualizar el Brankit.
- Identificado un problema crítico: la mayoría de los comentarios no se encuentran en los artefactos de los playbooks, sino en las celdas del grid HTML.
- Señalado que si no se incluyen los comentarios del grid, el procesamiento no tendría utilidad práctica.

#### Estructura Técnica de los Comentarios en el Grid

- Planteada la necesidad de determinar qué fuente concentra la mayor cantidad de comentarios: los artefactos de los playbooks o las celdas de columnas del grid HTML.
- Indicado que para acceder a los comentarios del grid sería necesario consultar a TipTap, ya que los comentarios de las celdas de output probablemente se almacenan allí.
- Explorado un ejemplo concreto de una columna de output de tipo HTML en un grid, donde se encontraron comentarios asociados a un workflow de comparación de contenido.
- Observado que visualmente no existe una forma directa de identificar de qué workflow proviene una columna de output en el grid.

#### Reconciliación de Comentarios del Grid con el Brankit

- Explicado que para reconciliar los comentarios de una celda del grid con el Brankit es necesario identificar el workflow que generó esa columna y verificar si tiene un Brankit como input.
- Identificados dos casos posibles en la configuración de inputs de una workflow column: uno donde el Brankit se obtiene desde otra columna del grid, y otro donde se selecciona manualmente.
- Detallado que cuando el Brankit proviene de una columna, el valor almacenado en la configuración del input es el ID de esa columna, lo que requiere un paso adicional para recuperar el Brankit real utilizado.
- Mostrado que cuando el Brankit se selecciona manualmente, el Brankit ID queda grabado directamente en la configuración del input del workflow, representando el caso más sencillo de reconciliación.
- Confirmado que existe una tabla intermedia en la base de datos llamada Brand Kit Sales, asociada a una grid cell cuando la grid column es de tipo Brankit, que contiene el Brankit ID como atributo directo.

#### Determinación de Elegibilidad para Workflows con Comentarios

- Propuesto un criterio de elegibilidad similar al de los playbooks para identificar los workflows a procesar, basado en workflows que no hayan sido procesados y que tengan un Brankit como input.
- Aclarado que los workflows no requieren el criterio de inactividad de 24 horas que se aplica a las sesiones de playbooks, ya que no tienen una sesión o chat intermedio.
- Precisado que el objeto de interés no es el workflow en sí, sino la workflow column, que es la columna donde se ejecuta el workflow y donde se generan los outputs.
- Discutida la posibilidad de usar webhooks para agregar una condición de elegibilidad basada en la presencia de comentarios, mediante una columna que registre el último comentario.
- Señalado que el contador de comentarios ya funciona en la interfaz, lo que sugiere que la información de cantidad de comentarios ya está implementada en el sistema.

#### Información de Dimensiones del Brankit como Input Adicional

- Planteada la posibilidad de utilizar la metadata de configuración del Brankit, como los product lines, content items, regiones y audiencias seleccionadas, como input adicional para el modelo que genera sugerencias.
- Argumentado que pasar esta información al modelo permitiría que las sugerencias estén más enfocadas en los elementos específicos que se utilizaron durante la ejecución.
- Reconocido que esta misma mejora podría aplicarse al proceso actual de playbooks, ya que cada sesión tiene un Brankit con dimensiones configuradas que actualmente no se pasan al modelo.
- Acordado que esta optimización no se implementará en la versión inicial, pero queda identificada como una mejora futura relevante.

#### Capacidad del Modelo para Ubicar Sugerencias en el Lugar Correcto del Brankit

- Discutida la complejidad de que el modelo identifique correctamente en qué parte del Brankit debe aplicar una sugerencia, dado que el Brankit tiene múltiples secciones como product lines, template outlines y reglas de escritura.
- Confirmado con un ejemplo concreto que el modelo ya es capaz de determinar el lugar adecuado, habiendo añadido correctamente una writing rule al content type Articles a partir de un comentario de usuario.
- Observado que algunos insights se ubican en las Global Writing Rules y otros en secciones específicas de content types, lo que indica que el modelo ya maneja esta distinción.

#### Modelo de Datos de Insights y Session Extraction

- Explicado que cada insight generado tiene un atributo llamado target subentity, que es un texto libre que apunta a un lugar específico del Brankit, como una writing rule con su ID.
- Introducido el concepto de Session Extraction como el objeto que agrupa un conjunto de Session Insights y que está asociado a un Brankit.
- Propuesto que el proceso de extracción de comentarios de workflows también debería generar una Session Extraction asociada al Brankit correspondiente, para que el proceso posterior continúe de forma natural.
- Confirmado que al asociar cada insight a una Session Extraction vinculada a un Brankit, el paso 2 del loop ya opera por Brankit y no por sesión, lo que resuelve un problema de diseño previo.

#### Redefinición del Alcance y Estructura del Feedback Loop

- Concluido que el alcance de la Fase 1 cambió considerablemente al incorporar el procesamiento de comentarios de grid workflows, pasando de una estimación de una semana a aproximadamente dos o tres semanas.
- Identificadas tres grandes partes para la Fase 1 redefinida: el procesamiento de comentarios de playbooks con generación de insights ya especificado, la elegibilidad y procesamiento de grid workflows con sus comentarios, y la generación de insights a partir de esos comentarios de workflows atados a un Brankit.
- Reconocido que la estructura del Feedback Loop cambia significativamente, ya que el entry point actual itera sobre sesiones de playbooks y se necesita un nuevo entry point para workflows.
- Propuesto que ambos loops, el de playbooks y el de workflows, funcionen como tareas separadas que convergen en el paso de generación de sugerencias, el cual ya opera por Brankit.
- Confirmado que las sugerencias se generan una vez al día, lo que permite que ambos procesos alimenten ese paso de forma independiente y luego se unifiquen.

#### Consulta Pendiente sobre Consolidación de Playbooks y Workflows

- Sugerido consultar a Tinte para conocer qué tan cerca está la plataforma de permitir hacer la comparación de contenido directamente en un playbook, sin necesidad de usar workflows externos.
- Señalado que si esa funcionalidad estuviera disponible o próxima, el flujo actual que combina playbooks y workflows podría consolidarse completamente en playbooks.
- Considerado que Tinte podría tener ya un camino definido para esta evolución, lo que impactaría directamente en el diseño del Feedback Loop.

#### Próximos Pasos

- Acordado actualizar el spec de la Fase 1 para reflejar el nuevo alcance que incluye el procesamiento de comentarios de grid workflows.
- Planificado armar una lista de tareas a gran escala que cubra todo lo necesario para completar la Fase 1 redefinida, con el objetivo de identificar qué partes pueden trabajarse en paralelo.
- Previsto comunicar al stakeholder el lunes que el alcance es mayor de lo esperado debido a la necesidad de procesar grids, y presentar una estimación más precisa en esa misma instancia.
- Contemplado consultar a Tinte sobre la posibilidad de consolidar el flujo de comparación directamente en playbooks, para evaluar si eso simplifica el diseño del loop.
- Definido que Sebastian y Juanma se coordinarán una vez que la lista de tareas esté lista para dividirse el trabajo y avanzar en paralelo donde sea posible.

## Transcript

**Sebastian Herrera | 00:00**\
\
Me encanta la musiquita cuando te llaman por joder, es buenísima.\
\
**Juanma | 00:04**\
\
No la escuché nunca.\
\
**Sebastian Herrera | 00:07**\
\
¿No la escuchaste? La debo haber escuchado, sí. Lo tengo cosas definidas, empecemos porí cosas definidas, fase 1 sobre playbooks eso creo que estamos todos...\
\
**Juanma | 00:15**\
\
Siempre en silencio. \
\
Para fase 1 es sobre traer comments y meterlos al proceso actual del FIBA Club para mejorar Sí, ojo que...\
\
**Sebastian Herrera | 00:35**\
\
El ranking. Exacto, eso mismo.í estamos, sin bloques.\
\
**Juanma | 00:43**\
\
Ojo, que tens que agregar a tu plan esto de traer los comments no solo de los artefactos, sino tambin de la... Esta es una cagada atómica.\
\
**Sebastian Herrera | 00:54**\
\
Pero eso ya va pensando si es Gritz Och bara bli också en vän.\
\
**Juanma | 01:02**\
\
Es que... Es que la mayoría de los comentarios hoy en día están en el grid.\
\
**Sebastian Herrera | 01:07**\
\
Í va, bien. Entonces, si no va a ser de ninguna utilidad.\
\
**Juanma | 01:12**\
\
Exactamente. Pero podis si queris revisarlo No sé cuál sería la forma más fácil. Capaz que tirar algunos... Capaz charllo con Claudio no sé cuál es la forma más fácil de revisar esto ¿Qué fuente es la que tiene más comentarios? ¿Los artefactos de los playbooks? O las grid cells que están en las columnas de un grid, ¿no? El grid HTML.\
\
**Sebastian Herrera | 01:32**\
\
Bien.\
\
**Juanma | 01:34**\
\
Si responds esa pregunta...\
\
**Sebastian Herrera | 01:35**\
\
A ti no, podrás saber... ¿Qué? Fantino podrá responder.\
\
**Juanma | 01:42**\
\
Juan Santiago capaz te puede guiar un poquito pero no te va a responder Porque si vive en TikTok, Santino no puede pegarle a la app TikTok porque no tiene acceso a las embeds.\
\
**Sebastian Herrera | 01:52**\
\
¿Y por qué precisamos...? Claro, porque los comentarios están en el TikTok, está bien.\
\
**Juanma | 01:59**\
\
Claro. Si la respuesta es, la mayoría de los comentarios están en el grid, en grid 6, que son en columnas grid HTML, grid columnas HTML, entonces no tiene sentido que vayamos a analizar solamente los comentarios de los artefactos en un playbook. Porque no estání, no van a estarí.\
\
**Sebastian Herrera | 02:14**\
\
¿Y para dónde se guardan esos comentarios? Los que están en un output del grid. Si no es en TipTap. Buena pregunta. O tambin es en TipTap.\
\
**Juanma | 02:26**\
\
Me parece que es en típtamo, a menos que haya cambiado. Si quers podemos... Pregúntale a tu agente eso capaz. Podríamos buscar en el Codeway si no, pero me parece que está solamente en.\
\
**Sebastian Herrera | 02:36**\
\
TikTok. Me imaginaría que sí... Entonces, de vuelta, fase 1 que es usar los comentarios para actualizar el brinca... Brinca... Brinca... At TriangCity.globe Tenemos todo...\
\
**Juanma | 02:55**\
\
No tenemos todo. Acordate de caso particular, porque si hay que ir a la gripe... Grid column html tens que de alguna forma reconciliar esa columna con el branquita. Esa reconciliación Es difícil. Porque para llegar a eso vas a tener que Vamos a ver un ejemplo, porque está muy necio. Deci nu ar trebui de cert.\
\
**Sebastian Herrera | 03:22**\
\
Se complicó.\
\
**Juanma | 03:27**\
\
Yeah. Thanks. Creer a un cliente de la verdad el campo que puede entrar recovery y vemos cómo hicieron las cosas Gracias. לא שם רכודי קונגויט. \
\
Mirá. Me voy al grid. En el gris tengo... Mira, en estuvo Vicky hoy, Content Creation Faces. Content Creation de hace dos días. A ver, veamos Content Creation. When I went to integration with DNA, No tiene DIF.\
\
**Sebastian Herrera | 04:19**\
\
No entendí eso del D. Y no tiene comentario. Crean un workflow que te compara un documento con otro. Yí se le llaman div.\
\
**Juanma | 04:27**\
\
T Sí, te acuerdo. Mira, vamos a buscar un ejemplo así, tal cual, content refresh. Muy seguramente te...\
\
**Sebastian Herrera | 04:38**\
\
Igual que haya ese div o no importa. Lo que nos importa es que haya comentarios en algún ladoí ponele.\
\
**Juanma | 04:45**\
\
De un grid. Correcto.í. Sí, acá hay un comentario. Hay un comentario, pero... Acá no hay playbooks. This is like a game water flows.\
\
**Sebastian Herrera | 04:56**\
\
Pero igual en caso tambin lo deberíamos procesar, yo me imagino.\
\
**Juanma | 05:03**\
\
Entramos en otro mundo si hacemos eso.\
\
**Sebastian Herrera | 05:08**\
\
Yo me imaginaría que sí, porque incluso... Creo que hoy en día, puedo estar errado, pero todavía se siguen usando mucho más los grids que los playbooks.\
\
**Juanma | 05:24**\
\
Los Warthogs de los Playbots. Sí, puede ser. Entonces, consideramos ese caso. Fíjate que acá hay comentarios, ¿no? Ponte un par de esos.í ponele eso.\
\
**Sebastian Herrera | 05:36**\
\
Va.\
\
**Juanma | 05:37**\
\
Comentarias, buscarías todos los comentarios. Detta Es una output column. ¿Ticareá? Esta columna que es de HTML a su vez es una columna de output. Thank you. De otra columna Entonces, si yo voy...\
\
**Sebastian Herrera | 05:56**\
\
Thanks.\
\
**Juanma | 06:00**\
\
Visualmente ni siquiera hay forma de buscar ¿De qué workflow sale esta content comparison? Pero yo asumo que sale de la que está más cercano a la izquierda. Claro. Entonces, si abro de esto... ¿Y ahora esto? Your sound boots. Justo, lo encontré. Conta en Comparison. Tiene el mismo nombre, así que asumo que es esto.\
\
**Sebastian Herrera | 06:20**\
\
Para.\
\
**Juanma | 06:21**\
\
Poder reconciliar... Esta celda con un brankit tendrías que ver si el workflow asociado que generó esa columna.\
\
**Sebastian Herrera | 06:31**\
\
Como input, teníamos un bracket.í va. Es una linda... La linda lógica de eligibility sería. Esto sería todo hasta ahora, aparte del eligibility query, algo así.\
\
**Juanma | 06:47**\
\
Ni siquiera, capaz que sí, tal cual, de la HLV Exacto, porque tiene que tener como input un ranking Sí, tens razón.\
\
**Sebastian Herrera | 06:54**\
\
Sería como, vamos a buscar los... Sería como más o menos lo mismo que tenemos hoy en Playbook. Los Playbook que no han sido procesados aún y tienen más de 24 horas sin actividad. Precisaríamos algo similar para... Workflows Sería, dame los workflows que... No han sido procesados. Y no han tenido actividad 24 horas Pero se les agrega una magia que es y además tengan un bracket como input.\
\
**Juanma | 07:32**\
\
You wouldn't be sure. Los workflows no necesitan el criterio del 24 horas de inactividad porque no hay un chat, no hay una sesión en el medio. No necesitan ese criterio.\
\
**Sebastian Herrera | 07:44**\
\
Ejecuta el chau.\
\
**Juanma | 07:46**\
\
Ejecuta y chao. Y no es el workflow en sí que te interesa, es la workflow column.\
\
**Sebastian Herrera | 07:50**\
\
Ela é o empoderador. No.\
\
**Juanma | 07:53**\
\
El output sería esta de acá. El Content Compiler, Refresher Article, todos estos son Outputs. Pero la workflow conmigo es la que nos interesaría acá. Tu lógica venía bien. Workflow column, que tenga su input esquema. Un input de Grand Git.\
\
**Sebastian Herrera | 08:09**\
\
Bien.\
\
**Juanma | 08:11**\
\
Hastaí estamos bien. Cuando vos quers reconciliar los comentarios de una celda al Brankit, Tenis dos casos. Fíjate que voy a configurar inputs de esta workflow call. Un saludo acá. Y acá dice, Brankit Column. Acá dice, Brankit, use values from grid. Branky column, Branky. ¿Qué significa esto? Que el brandkit lo está sacando de otra columna en grid, en caso, esta de acá. Evet. Entonces vos para poder reconciliar vas a tener que ir, buscar el esquema, ver que usted está usando una columna, ir a buscar esa columna y recin deí sacar de la senda de esa columna el bránquil actual que se usó. Que sea seleccionar manualmente y ese es tu caso más feliz porque el BrankID va a estar grabadoí mismo en la configuración del input de Keynote.\
\
**Sebastian Herrera | 09:09**\
\
Bien.\
\
**Juanma | 09:10**\
\
Voy a mostrar por acá, por encima.\
\
**Sebastian Herrera | 09:12**\
\
Y hastaí... Det kanskje det som kommer frem, men så videre Todavía ni siquiera nos estamos fijando en... Y que además tengan comentarios.\
\
**Juanma | 09:26**\
\
Cara, nossa. Claro, nada. Porque para saber que hay comentarios hay que ir y preguntarle a Tita, probablemente. Habría que ver cómo está hecho numerito de acá, porque numerito...\
\
**Sebastian Herrera | 09:39**\
\
Papá, es que con los webhooks, a mí el Claudio me tiró algo de que si queríamos meter condiciones de si tiene comments. En la eligibility podíamos usar el webhook. Para actualizar una columna que sea last comment. At. Y usar eso. Si está vacía, no tiene ningún comentario. Si tiene algo, te dice de cuándo es el último comentario.\
\
**Juanma | 10:13**\
\
Bien, puede ser. Suena raro de todas formas porque... Porque esto ya está funcionando. Y no sé si funciona con Webhooks, ¿no? Yo cargo la página y me aparece numerito de la cantidad de comentarios que tiene. Tiene cuatro. Permítanme que esté implementadoí.\
\
**Sebastian Herrera | 10:32**\
\
Tá.\
\
**Juanma | 10:33**\
\
Esto es... Un agujero negro, Seba, así que mientras más atajos tomen para resolverlo, mejor.\
\
**Sebastian Herrera | 10:43**\
\
Liên.\
\
**Juanma | 10:46**\
\
Para que te des una idea, solamente te muestro esto por encima. La workflow column, esta de acá, AirRefresh, que tiene estos configure inputs Branky Collins, mirá, vamos a ver cómo se guarda. Vamos a buscar. Voy a copiarme esta celda. Thanks you Le hago clic derecho y nos ve.í está. Un saludo. Esta es la columna con ID. Me voy a venir acá. Le voy a decir [web.id](http://web.id). Gracias. Y si vos te traes solamente los inputs... Acabas de tener... And Hashem and Shaitan que dice el primer input Se llama Branquit. Es de columna y el valor es número. Cuando es de columna, el valor es el ID de la columna. Si vos acá podrías traernos ese otro... Esa otra columna, ¿no? Para ver cuál es. Acá yo te digo y de itaipo Y justamente, ¿no? La que me trajo primero... Lo trajo al revs. La que termina en 9-3... Es la workflow column. Lo podemos poner acá, data type. And a better presentation.\
\
**Sebastian Herrera | 12:03**\
\
La workflow column es la que En la UI es la que ejecutás.\
\
**Juanma | 12:08**\
\
La workflow column es esta, mirá. - But you're not up execution and all that. Exacto, en la que ejecutas Entonces de vuelta, miren los impulsos. Un branque, un column, value tanto.\
\
**Sebastian Herrera | 12:24**\
\
Así como obtendría... El Branket para una columna de.. וואל פלום Pero me dijiste que habían dos formas de que... De que pueda tener una columna Es esta.\
\
**Juanma | 12:40**\
\
Que es esta, sí. Y hay otra que es directamenteí. A pulmón, vamos a ver acá, mirá Esta no necesita branquil. A ver ya es el refresh. Tambin está usando el grid. Vamos a ir a mi... Mi workspace pack me... Y vamos a poner un ejemplo... There's exciting content. Tiene una columna branquita, pero no importa, se lo voy a cambiar acá. S9, a ver.. But I'm keen. Use value from email. Everyone select manually. Que bon coup c'est Mario. \
\
Esto es un domario. Me voy a... Copiar esto En la columna sexta me vengo a la query. La comuna de Sexta. Y fíjate que acá apareció Type Brankit. Value y un hash.\
\
**Sebastian Herrera | 13:52**\
\
Í vamos.\
\
**Juanma | 13:54**\
\
Branky ID y Product Line ID vacío porque no seleccioné nada. Si yo fuera a seleccionar algo... And settings really inclusive. The reeds. Meeting and events like post 25 - Thank you, Adrian. Y vuelvo acá. Toda esa información queda en ese JSONí... Pero no nos interesa porque lo único que nos interesa es el.\
\
**Sebastian Herrera | 14:26**\
\
Ranking. Entonces las dos opciones son... Type No, ¿cómo es? Tiene como ya un atributo que es Brankit.\
\
**Juanma | 14:40**\
\
Tiene tight branky type brankit value se llama brankit y una columna Como de acá, type No hay ningún atributo granchi dentro de la iglesia.\
\
**Sebastian Herrera | 14:46**\
\
Ese brankit y el otro eraí va 안녕 column. Entonces hay un atributo bracket en los dos, algo así.\
\
**Juanma | 15:07**\
\
Eso no es el nombre de Input.\
\
**Sebastian Herrera | 15:07**\
\
Es un atributo.\
\
**Juanma | 15:13**\
\
Puede ser cualquier cosa, Pero es justo ese de branquita, y como es ese de branquita, Tiene estos values.\
\
**Sebastian Herrera | 15:14**\
\
La verdad. Podría ser cualquier cosa, está. Yeah. Good.\
\
**Juanma | 15:23**\
\
El otro que es de column en el value va a tener solamente la.\
\
**Sebastian Herrera | 15:26**\
\
Id. A ver, capaz conviene hacerte... Que aparezcan los dos en la pantalla y hacen un screenshot o algo así ya me quedan la vuelta.\
\
**Juanma | 15:35**\
\
Amen. エ テ シ ル エ ム.\
\
**Sebastian Herrera | 15:37**\
\
Sí y el anterior. El que Las dos opciones. Así que con esto de que... Hay que usar los grids, se ha armado un relajo bárbaro. A mí me parece que el scope cambió considerablemente.\
\
**Juanma | 15:55**\
\
Cambió un montón.\
\
**Sebastian Herrera | 15:57**\
\
Acabó de pasar de una semana a dos semanas. Para decir algo. Okay.\
\
**Juanma | 16:04**\
\
Por decir algo, sí, porque parece que es donde más se hacen los comments, ¿no?\
\
**Sebastian Herrera | 16:08**\
\
Claro, y aparte el otro era como que ya toda la estructura del Feedback Loop era... Estaba toda armada, era agregar unas cositas en algunos lugares y salía. Ahora que te cambia bastante la estructura.\
\
**Juanma | 16:23**\
\
Definitivamente. It's an algorithm. Miren esto. Huele gris. Volumen, ID Esta columna que es de branquita. Acá me pedí las celdas de esa columna de Brankit. Y el value está vacío. Hay que guardar un...\
\
**Sebastian Herrera | 16:49**\
\
Andamos un segundo al baño, que no me dio el tiempo de ir entre reunión y reunión. Vale. \
\
Con Luis. Mirá. Me querías mostrar algo que no me quedó muy claro.\
\
**Juanma | 17:56**\
\
Sí, nada, esta es una verdad. Quiero que veamos dónde se guarda la data. What I did in... \

- Thank you.\
  **Sebastian Herrera | 18:24**\
  ¡Gracias! And...\
  **Juanma | 18:33**\
  Mira, hay una tabla intermedia... Hay una tarjeta de medios que se llama Brand Kit Sales. Que está asociada a una grid cell cuando la grid column es de See you.\
  **Sebastian Herrera | 18:50**\
  Branching. Eso no sirve, ¿?\
  **Juanma | 18:54**\
  Sí, eso sí, Rui. Y acá te vas a encontrar con el Brankit ID como un atributo, como una columna directamente de la tabla en la base de datos.\
  **Sebastian Herrera | 19:02**\
  Para una pregunta, ¿el Brankit en realidad no está versionado? ¿El Brankit...\
  **Juanma | 19:09**\
  Acá se guarda la referencia... Fija, no la versión ¿Y Loop.\
  **Sebastian Herrera | 19:14**\
  Nos importa la versión? ¿O se usa la versión pública y listo? En el Feedback eso está.\
  **Juanma | 19:19**\
  Siempre se usa la versión pública acá. En el Feedback Loop se actualiza el draft. Bien. Siempre queremos actualizar el draft y eventualmente cuando el usuario configure el autopublish ¿Se hará o no se hará el publish a una versión? See you. Bye. Y fíjate el modelo, Frank Hitzel, porque acá está más convolucionado que otra cosa, porque... La tabla y la base de datos nos van a poner el Branky ID, que es lo que te interesa, y la Grid Cell ID, que probablemente tambin te interese. Va a ser más fácil. Determinar eso Pero despus hay modelos intermedios, ¿ves? Product line selections... Te guarda ¿Cuáles product lines fueron seleccionados por el usuario? Cuando vino... E hizo esto. Пока.\
  **Sebastian Herrera | 20:11**\
  And. Eso no nos interesa a nosotros.\
  **Juanma | 20:16**\
  Buena pregunta. Yo pensaría que en una versión inicial podemos ignorarlo. Pero si quisiramos ser ultra... Si quisiramos ser capaces de hacer que las sugerencias sean más focused o escopeanza algo Podríamos usar esto como input. Porque medio que sabemos... ¿Cuál fue el input del brand kit? Cuáles fueron los product lines que se usaron, cuáles fueron los content items, los regions, las audiencias, Si esta información se la pasas a la LDM que genera la sugerencia...\
  **Sebastian Herrera | 20:45**\
  Yeah.\
  **Juanma | 20:50**\
  Como para que pueda más fácil, como para que tenga como un empujón O no sé si un empujón, como una especie de guía para decir, ok... ¿Mi sugerencia? Pueden estar más enfocadas a estas cosas que se usaron. Bien. Porque usted tiene que ir a buscar de todo el mundo del bránquil, ¿no? Claro, como lo hace ahora. Hoy.\
  **Sebastian Herrera | 21:18**\
  Por hoy las reglas, porque cuando se actualiza el bránquedico en el Feedback Loop, ¿qué es lo que actualiza? Reglas. Todo. Se puede actualizar cualquier cosa. Qualche cosa.\
  **Juanma | 21:34**\
  Deci, el o poate vedea ca Hoy, por ejemplo, OGM... Actualizo el template outline del content type. I've added a template note to the article Hope you mean. Entonces.\
  **Sebastian Herrera | 21:52**\
  La... Hay mucha superficie, me parece... Ve la sugerencia. No sé, no uses emdash, vamos con ese ejemplo de siempre. باید برانکیت Y si tens 10 product lines con no sé qué, template outline, regla de escritura, no sé qué... Tiene que decir en donde de todos esos lugares. Area de arregla.\
  **Juanma | 22:23**\
  Sí, eso es lo que lo hacen.\
  **Sebastian Herrera | 22:25**\
  Es media locura. Que lo logra hacer En el lugar más adecuado. Que lo haga, lo entiendo. Que lo haga en el lugar adecuado. Probablemente es muy difícil que lo logre de forma correcta, consistente.\
  **Juanma | 22:42**\
  No, ya lo hace, mira. Acá el usuario puso we don't need to link to Easy Llamas features and every mention. Just the first mention is fine. Y entonces el loco dijo, añadí una Rating Rule a las writing rules de content type es el content type que se llama Articles. Y lo pusoí porque evidentemente estaban trabajando en un article.í va. Entonces todos los articles tienen esta... Esta regla, link to Easy Lama product page.\
  **Sebastian Herrera | 23:12**\
  Í va.\
  **Juanma | 23:15**\
  Despus hay algunos que los pone a las Global Rating Rules. A ver, no hay ningún ejemplo.\
  **Sebastian Herrera | 23:22**\
  Í va. Cero.\
  **Juanma | 23:25**\
  Take care. Estos son todos de Content Fives. Time. Es de... Esta es de editar una Rating Rule. They glow at rock and roll, terrifying support creep. Placement and unified medio que...\
  **Sebastian Herrera | 23:44**\
  Í va, tal, de alguna manera lo resuelve, bien, no hay que preocuparse, digamos.\
  **Juanma | 23:51**\
  Yo creo que no, me parece que ya está medio cocinado, habría que... Seguir usando esto. Acá, por ejemplo, agregó una global rating rule.\
  **Sebastian Herrera | 24:01**\
  Pero lo que decías es que si le diramos la info de qué product line se usó o alguna otra metadata. Puede ayudar a que tome esta decisión mejor.\
  **Juanma | 24:13**\
  Correcto. Y esto... Esto se me acaba de ocurrir en realidad, pero no hace falta que lo hagas. Porque si te pones a pensar, eso es lo que tambin podríamos hacer para el proceso actual Porque cada Playbook Session tiene un input y ese input tiene un Brankit y ese Brankit tiene sus dimensiones configuradas. Sin embargo, hoy en día no se lo estamos pasando.\
  **Sebastian Herrera | 24:33**\
  Bien, entonces por ahora no lo vamos a hacer, pero es algo que necesitamos. Listo. Entonces, a ver, haciendo un recap..... Hay que empezar de vuelta en el sentido de... Hay que pensarlo ya partiendo de la base que... Hay que procesar los comentarios de los grid workflows.\
  **Juanma | 25:03**\
  Algo que pods anotar queí en el medio... Es preguntarle a Tinte o escribirle a Tinte por encima para ver ¿Qué tan lejos estamos? De permitirles a los S6 o a los usuarios de Irox en general no necesitar workflows para flujo. El flujo es el que vimos recin, ¿no? Corre en un playbook. El output se lo pasan como input de un workflow. Hacen la comparación. Y escupen en otra columna el resultado y hacen comentariosí. La pregunta es, ¿qué tan lejos estamos de poder hacer la comparación directa en un playbook? Y tener todo en Playbook sin necesitar workflows. Capaz que Tinte tiene una mejor Bien. O tiene el camino ya marcado, no sé.\
  **Sebastian Herrera | 25:47**\
  Ciao.\
  **Juanma | 25:49**\
  Puedes rendir pronto, darle y estar al tanto.\
  **Sebastian Herrera | 25:53**\
  Да. Sí, creo que está bien saberlo.\
  **Juanma | 25:58**\
  Pero esto va a necesitar actualizar tu spec seguro porque...\
  **Sebastian Herrera | 26:02**\
  Sí, no, ya cambió Cambió todo, sin duda.\
  **Juanma | 26:03**\
  Todo. No todo, pero la parte importante que es traer comentarios y reconciliarlos a un brand kit se picó. Claro. Justamente...\
  **Sebastian Herrera | 26:17**\
  Claro. Lo que estoy pensando es porque hoy lo que hace las eligibility rules es Me fijo los playbooks que ejecutaron en más de tantas horas, lo que sea y despus Cuando va a empezar el loop, digamos... Es... ¿Por Playbook o es por Brankit?\
  **Juanma | 26:49**\
  Okay. Cuando va a empezar el loop.\
  **Sebastian Herrera | 26:53**\
  Dice, ya tengo la... Gracias. El grueso de la información que quiero procesar. Hoy en día... Y Tera sobre... ¿Qué cosa? Hoy en.\
  **Juanma | 27:05**\
  Día el primer paso ítera sobre sesiones. Las que son elegibles. Sesiones de playbooks. Traen los insights, sesiones de.\
  **Sebastian Herrera | 27:12**\
  Playbooks. Entoncesí tenemos un... Tambin un problema interesante que es que estamos iterando sobre sesiones de playbooks.\
  **Juanma | 27:26**\
  En Toda la razóní. Necesitamos un nuevo entry point del.\
  **Sebastian Herrera | 27:33**\
  Feedback loop. ¿Papá qué es un loop nuevo? Un loop de workflows Sí. No me gusta la idea de tener dos distintos. Pero me No, más que nada por el...\
  **Juanma | 27:46**\
  Están malendo.\
  **Sebastian Herrera | 27:50**\
  Claro, más que nada por el feature parity, digamos, que...í va, lo que queremos en realidad es que converjan rápido.\
  **Juanma | 28:01**\
  Sí, yo creo que no está mal y no le veo un problema con Future Party porque nuevo proceso debería generar insights. De la misma manera en que el proceso actual genera Y eventualmente una vez al día todos esos insights se agarran Y se generan ser, podría ser.\
  **Sebastian Herrera | 28:12**\
  Insights. Exacto. Suggestions. Porque son como dos loops aparte. Son como dos tareas aparte. Podría digo, hoy en día no es, hoy en día no es ya.\
  **Juanma | 28:25**\
  Pero.\
  **Sebastian Herrera | 28:30**\
  Hoy en día tens el loop que procesa los... Playbook Sessions Y no es que enseguida... Ya arranca a hacer la suggestion. ¿Es otro loop? No. Bien, entonces sí, los podemos conectar de esa manera.\
  **Juanma | 28:47**\
  Í va, me gustó. Sí, yo creo que sí, por mí. Son las tres fases. La primera parte acabar con su propia...\
  **Sebastian Herrera | 28:52**\
  Tendría que.\
  **Juanma | 28:54**\
  Claro. Y despus, a la hora de generar su gestióní se juntan y siguen. Las sollovias se generan una vez al día a las Creo que 4 de la mañana UTC.\
  **Sebastian Herrera | 29:04**\
  Algo así. Y para, ya cuando ese paso 2 de, agarro las suggestions, y veo que hago Aisha es Por Brankit. O seguimos con el mismo problema de que es por sessions.\
  **Juanma | 29:20**\
  No, tens razóní ya es por Branky.\
  **Sebastian Herrera | 29:23**\
  Entonces estamos bien, entoncesí estamos notables.\
  **Juanma | 29:26**\
  Porque de hecho me parece que la Insight guarda esto tambin. ¿Guarda? ¿A dónde estás apuntando? Si vos entras al modelo Insight RD... Session Insight RB, vas a ver que hay un atributo que se llama... Et un tributo che si chiama Den dyktas. E Tärge suu endidi.\
  **Sebastian Herrera | 29:59**\
  Sí, me suena a lo visto en algo del TechSpec. Sí, creo.\
  **Juanma | 30:04**\
  Sí. Coagulant station extraction Session Extraction es como el que adupa.\
  **Sebastian Herrera | 30:12**\
  Station extraction no me suena, pero el target subentity sí me suena. 단다아보라오이다. Para algo.\
  **Juanma | 30:22**\
  See? Cada insight que se generó tiene un target subentity que puede ser.. Es un texto libre, que apunta a un lugar del branquín. Puede decir Hiding rules, id, igual tanto para que referencia a esa writing rule. But you have the Session Extraction, which is what encompasses a whole group of Session Insights. Si quiero chusmearlo tambin. Yí es que veo que la session extraction está asociada a un bránquil. And Eso probablemente... Es lo que necesites hacer tambin a la hora de extraer comentarios, ¿no? ¿Extraer comentarios, generar insights? Tambin generas una session extraction, lo asignas a un brand kit. Cosa que despus el proceso siga naturalmente Cada insight va a estar asociado a un ranking. Por defecto.\
  **Sebastian Herrera | 31:15**\
  And No, está, ahora en resumen me queda que el grueso...\
  **Juanma | 31:17**\
  ¿Está medio entregarado o cómo lo ves?\
  **Sebastian Herrera | 31:26**\
  El grueso, en realidad veo tres grandes... Siempre hablando de fase 1, veo tres grandes partes. Una es la que ya habíamos hecho el spec, que es procesar los comentarios. Day of Playbooks generar insights y que una vez que están los insights los chupa el Paso 2 y paso 3.\
  **Juanma | 31:51**\
  King.\
  **Sebastian Herrera | 31:52**\
  Ese es un grueso. Grueso 2 va a ser... La eligibility Sí. Los Grid Workflows y sus respectivos comentarios. El... Sí. Determinar cómo elegimos qué workflows... Vamos a procesar. Ese le veo como el grueso 2. Y el grueso 3 es... Agarrando eso.. Generar insights tambin. A partir de eso.\
  **Juanma | 32:34**\
  Y en ese proceso de generar insights tambin tens que Atarlos a un branquín.\
  **Sebastian Herrera | 32:41**\
  Claro, sí, porque en realidad ya lo tendríamos del eligibility. Me imagino que tendríamos un grid column o no se que cosa, asi como en el otro tenes session id Seguir column ID Y blanket. \
  Sim, eu acho Creo que son esas las tres áreas y estamos hablando de que... Hasta hace un rato el scope era solo... La primera parte. Y ahora tenemos estas tres piezas.\
  **Juanma | 33:16**\
  .. Lo mejor que nos podría pasar es negociar para que los comentarios que se hacen en un workflow no entren. Pero... La realidad es otra. Sí, la.\
  **Sebastian Herrera | 33:32**\
  Veo difícil. La veo difícil porque... Ya viste que Rafa mostró un ejemplo Ne da full workflow.\
  **Juanma | 33:42**\
  See.\
  **Sebastian Herrera | 33:45**\
  ¿Está? Lo que sí...\
  **Juanma | 33:49**\
  Thank you.\
  **Sebastian Herrera | 33:51**\
  No, que ahora ya no hay más reunión con él, pero lunes. Plantearle esto y decirle que son varias etapas. Sin hablar todavía de Live Updates, Live, no sé cómo se llama, Nier Live, no sé qué.\
  **Juanma | 34:08**\
  Live... Nier.\
  **Sebastian Herrera | 34:11**\
  Sin entrar en eso, estamos hablando de un alcance... ...capaz que dos semanas... ...o tres, no sé... Dependiendo de qué tan libre esté WoW.\
  **Juanma | 34:22**\
  Sí, yo te voy a dar una mano en la semana que viene. Capaz que eso estaría que logres armar. Una lista de las tareas.. A gran escala, ¿no? Bien. Visto desde muy lejos. De todo lo que necesitamos hacer para completar esto. Claro. Y cuando tengas esa lista, podemos... Dividirnos entre nosotros dos cuáles tareas creemos que podemos atacar en.\
  **Sebastian Herrera | 34:46**\
  Paralelo. Síí va, por eso va a depender mucho de si vamos a poder trabajar en paralelo. Yo creo que sí. Pero vos no sé.\
  **Juanma | 34:56**\
  Tengo un par de cositas dando vueltas, no sé, estoy con el autopublish ahora... Lo del feedback loop actual sigo haciendo review. Pero no mucho más que eso. El auto polish sale en dos patadas. Ciao cuando termines de cruzar el spec si quers mandame esa riquita de 那 你 Vistas muy por encima y... Y nos organizamos desdeí Pero el lunes lo encargamos con todo y va a salir... Güzel. Va a salir así.\
  **Sebastian Herrera | 35:32**\
  Perfecto.\
  **Juanma | 35:34**\
  Está picante, pero lo vamos a sacar. Okay.\
  **Sebastian Herrera | 35:38**\
  Excelente. Y decís que vale la pena avisarle a... Ahora ya ponerle che el alcance con esto de que hay que procesar grids. Es un poquito más grande de lo que esperábamos, pero está. El lunes te pasamos la estimación bien. Está bien si le decimos eso. Suena I didn't hear.\
  **Juanma | 35:57**\
  Bien. Sí, suena bien decirle eso. Echt.\
  **Sebastian Herrera | 36:05**\
  Juanma, gracias a vos. Estamos hablando. Buen fin de semana. Chao.
