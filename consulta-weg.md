---
type: CarCharge
_organized: true
---
# Consulta WEMOB

<http://updates.weg.net/chargingstation/WEMOB_PARKING/W-007-1C/HW3/v4-6-2.bin>

Hola, buen día.

Les escribo porque compré un cargador WEMOB y necesito ayuda con dos consultas técnicas.

Datos del equipo:

Modelo: WEMOB-W-007-R-1T2\
Número de serie: 1124136676\
Versión actual de firmware: 3.6.6-2

Estoy intentando configurar una URL de servidor OCPP distinta, apuntando a una aplicación propia compatible con OCPP 1.6J. Sin embargo, no logro que el cargador se conecte cuando utilizo una URL segura con wss://.

Lo que sí funciona:

- ws:// apuntando a una aplicación corriendo en mi red local, usando la IP local del servidor
- también pude conectarlo usando un túnel TCP

Lo que no funciona:

- URLs wss:// públicas
- tampoco parece funcionar correctamente cuando la aplicación está publicada en la nube

En estos casos, no vemos siquiera intentos de conexión llegar a la aplicación o al servidor, por lo que sospechamos que el problema podría estar ocurriendo antes del establecimiento de la sesión WebSocket, posiblemente a nivel de TLS/SSL, validación de certificados, compatibilidad de cifrados, SNI u otro requisito de seguridad del protocolo.

Mi primera consulta es:

1. ¿Podrían orientarme sobre qué podría estar impidiendo la conexión por wss://? ¿Existe algún requisito específico de compatibilidad TLS, certificados, puertos, SNI o configuración del cargador que debamos tener en cuenta para conectar con un servidor OCPP externo?

Mi segunda consulta es sobre la actualización de firmware:

2. Estoy evaluando actualizar el firmware del equipo, pero en la página oficial de actualizaciones: <https://updates.weg.net/chargingstation/products/ac/index.html>

no me queda claro qué variante corresponde a mi cargador, en particular si debo elegir HW3 o HW4. Tampoco me queda claro cuál es el procedimiento correcto para actualizar el firmware de este modelo.

¿Podrían por favor confirmarme:

- qué hardware corresponde a este equipo (HW3 o HW4),
- cuál es el firmware correcto para este cargador,
- y cuál es el procedimiento recomendado para realizar la actualización de forma segura?

Desde ya, muchas gracias por la ayuda.

Saludos cordiales,\
Sebastian.
