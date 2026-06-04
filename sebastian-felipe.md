---
type: AirOps
_organized: true
---
# Sebastian / Felipe

## PART 1

#### Playbooks y Workflows

```text
- Los playbooks no son procedurales, sino habilidades que toma un agente, con un conjunto de inputs que generan salidas encadenables con otros playbooks y workflows.
- Pueden combinarse con partes de Insight para obtener accionables sobre páginas y mejorar el tráfico orgánico o la visibilidad en el mundo agente.
```

#### AEO World y Visibilidad de Marca

```text
- Lanzada junto al anuncio de la Serie B, esta herramienta permite ver cómo se comporta una marca en el mundo agente, midiendo cuánto la citan los agentes de IA como ChatGPT.
- Un caso de uso típico es el de una zapatería que quiere saber si ChatGPT la menciona cuando un usuario pregunta dónde comprar zapatos en una ciudad determinada.
```

#### Brand Kit

```text
- Existe un Brand Kit por workspace, aunque ciertos workspaces tienen la feature flag activada y pueden tener varios.
- Una migración importante realizada el año pasado rediseñó el modelo de Brand Kit para permitir a las marcas establecer distintos dominios dentro del mismo, por ejemplo por producto.
- Marcas como Carta tenían Brand Kits separados por línea de productos, y la migración buscó encapsular esas lógicas dentro de un único Brand Kit con subdominios.
- El Brand Kit se autogenera durante el onboarding y luego puede ser revisado y modificado por el usuario.
- Existe soporte para versionado del Brand Kit, aunque hay deuda técnica pendiente en torno a su rendimiento.
- Funciona también como input en workflows, grids y playbooks, permitiendo referenciar las reglas de marca en distintos procesos.
```

#### Onboarding y Autogeneración de Métricas

```text
- Al crear un workspace nuevo, varios procesos se autogeneran mediante LLMs que consultan fuentes de datos, incluyendo competidores de la marca y un conjunto de target prompts para trackear.
- El Brand Kit se autogenera durante el onboarding a partir del dominio ingresado, y el usuario puede revisarlo y ajustarlo antes de continuar.
```

#### AEO Brand Kit como Objeto de Dominio

```text
- El AEO Brand Kit es un modelo de Rails separado que actúa como decorator sobre el Brand Kit base, agregando lógica y relaciones propias del mundo AEO sin modificar el modelo original.
- Esta separación fue una decisión de diseño para no mezclar la lógica de AEO con el modelo base, y podría haberse implementado también como un concern.
```

#### Knowledge Base

```text
- Las Knowledge Bases permiten que los LLMs busquen información vectorizada, utilizando Pinecone como vector store para hostear las bases de datos.
- Los usuarios suelen importar el sitemap completo de su sitio, lo que genera un scraping de todas las páginas filtradas, pudiendo llegar a cientos de archivos.
- También es posible subir documentos manualmente o conectar carpetas de Google Drive.
- Dentro de los workflows, existen tools explícitas como "Search Knowledge Base" y "Write to Knowledge Base" que permiten leer y escribir en estas bases como pasos definidos.
```

#### MSP Connectors y Tools

```text
- Los usuarios pueden conectar sus propios MSP Connectors desde la configuración del workspace y activar o desactivar tools específicas.
- Estos conectores pueden ser referenciados dentro de los playbooks, por ejemplo usando un conector de Notion para interactuar con esa plataforma.
```

#### Utility Steps y Power Agents

```text
- Existen workflows reutilizables llamados Utility Steps que cumplen funciones genéricas, como agregar preguntas frecuentes a un artículo.
- Los Power Agents son también workflows que cumplen un propósito específico y pueden usarse como pasos individuales dentro de otros flujos, funcionando como una librería interna.
```

#### Content Quality y Checks

```text
- El pod de Content Quality trabajó la migración del Brand Kit y desarrolla procesos para mejorar la calidad del contenido generado por la plataforma.
- Al finalizar la ejecución de un workflow o playbook, los Content Quality Checks leen la salida, aplican las writing rules del Brand Kit y generan una lista de puntos de mejora.
- Cada check individual puede aplicarse para ajustar automáticamente la salida mediante un proceso en background similar a un job asíncrono.
- Este proceso es comparable a un code review asistido por IA, donde se lee el contenido generado y se sugieren mejoras alineadas con las reglas de marca.
```

#### Langsmith y Trazabilidad

```text
- Langsmith se utiliza para gestionar los traces de ejecución de los procesos de IA, reemplazando una implementación anterior más rudimentaria.
- Cada trace muestra el desglose de tool calls, scrapes, llamadas a LLMs y sus salidas, permitiendo visualizar el flujo completo de ejecución.
- También proporciona visibilidad sobre el costo en tokens de cada proceso, lo cual es fundamental para controlar el gasto.
- Los evaluators en Langsmith permiten definir un score de calidad para las salidas, corriendo automáticamente cada vez que entra un nuevo trace.
- Estos evaluators usan un LLM como juez que evalúa la salida en base a criterios definidos, como jerarquía válida o neutralidad temática.
- También es posible configurar evals para detectar si un cambio en el modelo o en el prompt mejora o empeora el proceso antes de desplegarlo.
```

#### Evals a Nivel de Código

```text
- A nivel de código existen regression tests que validan las salidas contra un conjunto de datos conocidos con resultados esperados.
- Estos evals se corren cada vez que se realiza un cambio, permitiendo detectar regresiones en el comportamiento del sistema.
```

#### Proceso de Deploy

```text
- El proceso de deploy comienza poniendo una luz amarilla en el canal de deployments para avisar al equipo.
- Luego se ejecuta el script "boom version" desde el directorio bin, que genera un tag contra el último commit y lo pushea al repositorio.
- Las GitHub Actions se activan al detectar el nuevo tag y ejecutan el pipeline de deploy, que incluye build de imágenes, migraciones y ciclado de tasks en AWS.
- Existen dos formatos de tag, con y sin "needs reload", siendo el segundo para cambios que requieren que el usuario refresque el frontend.
- Las migraciones pueden fallar de forma intermitente por long running queries que bloquean los logs, en cuyo caso se puede reintentar el paso sin consecuencias graves.
- Si algo falla durante el deploy, se recomienda pedir ayuda en el canal de ingeniería en lugar de intentar resolverlo solo.
```

#### Staging y Flujo de Merges

```text
- El deploy a staging se dispara automáticamente con cada merge a main, sin necesidad de intervención manual.
- Al hacer un deploy a producción con "boom version", es habitual que incluya cambios de otros miembros del equipo además de los propios.
- Todo lo que se mergea a main debe estar probado y listo para deployarse en cualquier momento, ya que cualquier deploy posterior lo incluirá.
- Es posible solicitar un freeze de merges cuando se necesita probar algo específico en staging, como un upgrade de dependencias.
```

#### Variables de Entorno

```text
- Las variables de entorno se gestionan mediante un script en el directorio bin que lee las task definitions de AWS y genera una nueva versión con la variable agregada.
- Si dos personas setean variables de entorno sin hacer deploy entre medio, la segunda puede pisar las variables de la primera.
- Por esta razón, después de setear una variable de entorno es necesario hacer un deploy antes de que otra persona agregue nuevas variables.
- El script permite elegir el ambiente destino, ya sea producción, staging u otro cluster disponible.
```

#### Arquitectura de Servicios en AWS

```text
- En producción existen múltiples tasks en AWS con responsabilidades diferenciadas, incluyendo Rails Public API Service para la API pública y de uso interno, Rails App Service para todo lo que es user facing, y un servicio dedicado a requests pesadas de vector stores.
- Existen nodos específicos de Sidekiq para manejar las colas de AEO, que procesan volúmenes muy altos, superando el medio millón de jobs por día.
- El Psychic Master Scheduler actúa como capa previa a Sidekiq, leyendo la tabla de jobs en estado pendiente y evaluando si pueden ejecutarse según reglas de throttling por workspace.
- Cada clase de job implementa un método "can_run" que determina si una instancia específica puede ejecutarse, siendo esta la lógica central del scheduler.
- También existen nodos dedicados para workflows y para el procesamiento de colas de ejecución con distintas prioridades.
```

#### Gestión de Excepciones y Monitoreo

```text
- Existe un canal de excepciones que es bastante ruidoso, con un rol rotativo de Exception Master encargado de revisar y asignar las excepciones relevantes.
- Al revisar una excepción, se identifica al responsable con más contexto según el área del código afectada y se lo taggea en el canal.
- Las excepciones recurrentes y conocidas pueden ignorarse, mientras que las nuevas o de alta frecuencia requieren atención inmediata.
- El monitoreo de la base de datos se realiza a través de RDS Performance Insights, donde se identifican queries lentas y se taggea al responsable del área correspondiente.
```

#### Revisión de Pull Requests

```text
- Se requiere al menos un approve para mergear, aunque la preferencia es que quien revise tenga conocimiento de la tecnología involucrada.
- Si un PR toca frontend y backend, se busca que lo revisen personas con experiencia en cada área.
- Para código Python, se prefiere que lo revisen quienes tienen más experiencia en ese lenguaje, y para Ruby cualquier miembro del equipo con contexto puede hacerlo.
```

#### Próximos Pasos

```text
- Sebastian comenzará explorando los modelos del Brand Kit y la documentación del pod de Content Quality para ganar contexto sobre su área de trabajo.
- Se planea profundizar en el funcionamiento interno del código con otros miembros del equipo, especialmente en lo relacionado con Cloud y las features del pod.
- Felipe invitará a Sebastian al workspace de la plataforma y a Langsmith para que pueda comenzar a explorar los traces y la configuración de evaluators.
- Se acordó continuar con preguntas a medida que Sebastian vaya interactuando con la app y encontrando áreas que requieran más explicación.
```

***

## PART 2

#### Local Environment Setup

```text
- El entorno local de Sebastian fue configurado arrancando con un git pull reciente y Docker corriendo.
- El frontend se levanta con el comando npm run build con un flag de max memory específico, seguido de npm start.
- Sidekiq se levanta con bundle exec sidekiq con un parámetro de concurrencia.
- Rails se levanta con bundle exec rails s de forma estándar.
- Todos los comandos de frontend, Sidekiq y Rails deben ejecutarse desde la carpeta packages/rails-app.
- Un comando adicional para el Master Scheduler también levanta los Stripe Webhooks locales, sumando cuatro procesos principales para tener el entorno completo corriendo.
```

#### Servicios en Docker

```text
- Los servicios necesarios en Docker incluyen SmithEngineX, OAP y Clickhouse, siendo SmithEngineX el más crítico para el funcionamiento de la aplicación.
- Modal es un servicio que no se necesita de inmediato pero será relevante en algún momento.
- La base de datos corre en Docker porque el script de copia de staging la levantó automáticamente, aunque Felipe prefiere correr Postgres de forma nativa.
- Redis corre de forma nativa y debe mantenerse encendido.
- Clickhouse fue levantado por un agente para pruebas y no es estrictamente necesario.
- DBC es considerado deprecable y Redis Commander tiene un propósito no del todo claro para Felipe.
```

#### Migraciones y Acceso Inicial

```text
- Las migraciones se corren con rails db:migrate desde la carpeta packages/rails-app, de la misma forma estándar.
- Para acceder al panel de administración se navega a localhost:3000/adminops/admin.
- Un usuario administrador se crea desde Rails Console usando AdminUser.create con email y password, siendo tablas independientes del usuario regular.
```

#### Configuración del Workspace de Prueba

```text
- El workspace llamado "Jones Snowboards" es el que usa el equipo como referencia principal para pruebas.
- Desde el panel de admin ops se puede buscar el workspace por nombre y usar la función "Invite Self" para agregarse con el mismo email del admin user.
- También existe un workspace de Juanma en la base de datos de staging que puede consultarse para uso posterior.
```

#### Prueba de Funcionalidad con Workflows

```text
- Para verificar que SmithEngineX está funcionando correctamente se crea un workflow con un step de "Web Page Scrape".
- Al ejecutar el step con una URL real, si Smith está corriendo, trae el contenido correctamente.
- La ejecución del workflow corre como un job en Sidekiq, específicamente como un App Execution que ejecuta los steps del workflow.
- El step de web page scrape utiliza un servicio llamado ScrapFlight que vive dentro de Smith y tiene múltiples scrapers.
```

#### Arquitectura de Workflows y App Runner

```text
- El App Runner es el sistema que encapsula la ejecución de workflows y sus steps individuales.
- Cada step está diseñado como un workload independiente, lo cual Felipe considera una solución arquitectónica bien resuelta.
- Esta arquitectura está siendo relegada en favor de Playbooks, aunque no está siendo deprecada activamente.
- El sistema de traces dentro de esta arquitectura es considerado por Felipe como una parte que no está bien resuelta.
```

#### Configuración de Quill Copilot

```text
- Quill es el copilot recientemente lanzado que corre sobre los App Executions y los agents.
- Los agents son una feature deprecada que representó el primer enfoque de la aplicación para conversaciones con IA.
- Para que Quill funcione localmente se requiere configurar una variable de entorno específica relacionada con el OAP.
- La definición del agente de Quill es un JSON que debe cargarse en un workspace específico.
- La variable de entorno de Anthropic también es necesaria y debe configurarse tanto en el entorno de Rails como en el de Smith.
- La variable de Langsmith es requerida por Smith para el funcionamiento de los agentes de Quill.
- Vicky recomendó a Sebastian intentar no depender de Quill durante el primer mes para entender mejor la aplicación por su cuenta.
```

#### Descripción de Servicios Adicionales

```text
- Clickhouse es la base de datos secundaria donde vive toda la data de Aevo.
- Coa es un servicio en Node.js que maneja la serialización y deserialización de documentos al formato JSON que espera TipTap.
- Unstructured App es el servicio encargado de la vectorización para los Vector Stores, siendo Rodri su owner.
- Smith es un servicio en Python creado porque ciertas funcionalidades, como los agentes de Quill con Langchain, no existían en Ruby.
- August también tiene relación con Smith actualmente.
```

#### Canales de Comunicación y Recursos del Equipo

```text
- Para dudas técnicas se recomienda usar el canal de devsets o el canal A Internal en lugar de canales muy masivos.
- Existe un canal llamado "reporting" donde llegan los bug reports, que luego se evalúan y se convierten en tickets vinculados a la aplicación.
- Los tickets de ingeniería pasan por el Product Manager antes de ser asignados.
- Hay dos canales internos, uno en inglés con más actividad técnica y otro en español usado principalmente para contenido informal.
- El canal A Engineering incluye participación de otras áreas del equipo pidiendo clarificaciones o asistencia.
```

#### Documentación y Recursos de Onboarding

```text
- La sección de Agent Skills en la plataforma es un recurso recomendado para entender las capacidades del sistema.
- Los ADR son documentos generados principalmente por IA que describen features de la aplicación, como Brankets y Versioning.
- Claudia y otros miembros del equipo como Coder y Excursor son referenciados como personas que pueden responder dudas sobre la aplicación.
- Para dudas sobre Unstructured App y Smith se recomienda consultar a Rodri y Agus respectivamente.
- Shabazz, quien pasó de Solutions Architect a Ingeniería, está trabajando en temas de infraestructura y plataforma.
```

#### Próximos Pasos

```text
- Se planea dejar todos los servicios del entorno local corriendo de forma estable y resolver los problemas pendientes de Quill con ayuda de Agus o Rodri.
- Sebastian revisará los videos de onboarding, leerá documentación, modelos y documentos de la plataforma para familiarizarse con la aplicación.
- Se contempla migrar la base de datos de Docker a una instalación nativa de Postgres en el futuro.
- Cualquier duda técnica será canalizada a través de los canales internos del equipo o directamente con los owners de cada servicio.
```
