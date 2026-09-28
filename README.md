# Informe-de-Auditor-a-de-Red-Wi-Fi-Insegura


Informe de Auditoría de Seguridad: Riesgos en Redes Wi-Fi Públicas
Contexto de la evaluación: Análisis de interceptación de tráfico en redes inalámbricas abiertas (sin contraseña o con contraseña compartida).

1. La Vulnerabilidad Base: HTTP vs. HTTPS
La seguridad del tráfico en cualquier red depende principalmente del protocolo de comunicación utilizado entre el navegador del usuario y el servidor web:

HTTP (Protocolo de Transferencia de Hipertexto): Transmite toda la información en texto plano. Si un usuario se conecta a una red pública y visita una página HTTP, cualquier persona conectada a la misma red (con un software de análisis de paquetes básico) puede interceptar y leer exactamente los mismos datos que el usuario está viendo o enviando.

HTTPS (HTTP Seguro): Añade una capa de cifrado mediante los protocolos TLS/SSL. Antes de enviar cualquier dato, el dispositivo y el servidor acuerdan una clave secreta. Si alguien intercepta el tráfico en la red pública, solo verá una cadena de caracteres incomprensibles y la dirección IP del servidor destino, pero no el contenido del mensaje.

El riesgo crítico en una red Wi-Fi pública surge cuando los usuarios navegan por sitios que aún utilizan HTTP o cuando aplicaciones en segundo plano transmiten datos sin cifrar.

2. Información Expuesta durante una Conexión HTTP
En una red pública, un atacante pasivo no necesita "hackear" la computadora del usuario; simplemente configura su tarjeta de red en modo monitor y captura los paquetes que flotan en el aire. Si el tráfico es HTTP, el atacante puede extraer:

Credenciales de acceso: Nombres de usuario y contraseñas introducidos en formularios de inicio de sesión.

Cookies de sesión: Pequeños archivos que mantienen la sesión abierta. Si un atacante roba una cookie activa, puede inyectarla en su propio navegador y acceder a la cuenta de la víctima sin necesidad de saber la contraseña (técnica conocida como Session Hijacking o secuestro de sesión).

Datos personales y financieros: Cualquier información escrita en formularios de contacto, carritos de compra o chats.

Historial de navegación: Las URL exactas de las páginas visitadas, imágenes descargadas y el tiempo de permanencia.

3. Propuesta de Mitigación: Uso de Redes Privadas Virtuales (VPN)
Para proteger a los usuarios que deben conectarse a redes Wi-Fi públicas en aeropuertos, cafeterías o plazas, la solución técnica más efectiva a nivel de usuario es el uso de una VPN.

¿Cómo funciona para proteger el tráfico?
Una VPN actúa como un "túnel" blindado dentro de la red pública. Cuando el usuario activa la VPN:

Todo el tráfico de Internet del dispositivo (ya sea HTTP, HTTPS, o de aplicaciones) se cifra localmente antes de salir del dispositivo.

Estos datos cifrados viajan a través de la red Wi-Fi pública hasta el servidor de la empresa proveedora de la VPN.

El servidor VPN descifra los datos y los envía a su destino final en Internet.

El resultado en la red local: Si un atacante en la misma cafetería intercepta el tráfico de un usuario con VPN, solo verá una conexión constante hacia una única dirección IP (la del servidor VPN) y un flujo de datos cifrado e ilegible. La VPN hace irrelevante la inseguridad de la red Wi-Fi local, ya que asume que la red es hostil y protege los datos por encima de ella.


Parte 1 – Exploración
Ingresa al siguiente sitio web: http://neverssl.com

Abre las herramientas de desarrollador del navegador (F12).
Selecciona la pestaña Network (Red).
Recarga la página.
Selecciona la primera solicitud realizada.
Observa los siguientes datos:
URL solicitada
Método HTTP
Host
Protocolo utilizado
Headers enviados

Aca poodemos observar como ya luego de haber apretado la tecla f12 se puede ver la seccion de network 

<img width="1058" height="344" alt="NETWORK" src="https://github.com/user-attachments/assets/51a0c0b8-3d48-458b-84e4-4af9cd660a24" />


Posteriormente se puede ver lo solicitado
En la parte de General podemos ver la Request URL 
El Método HTTP (Request Method): GET
El  Host se puede ver en la seccion de abajo del todo donde dice Request headers 
El protocolo utilizado lo podemos ver en la seccion de Response headers


<img width="958" height="937" alt="Screenshot_1" src="https://github.com/user-attachments/assets/329fe2bc-40ca-4cd9-8be2-e45e78e3841c" />



2. ¿Qué información puede observarse durante la solicitud?

URL solicitada: Es la dirección exacta del recurso que tu navegador está intentando descargar. En la primera solicitud, casi siempre es la ruta raíz (/) o el archivo principal (como index.html).

Método HTTP: Indica la intención de la petición. El método GET significa que el navegador solo está pidiendo leer o descargar información. Si estuvieras analizando el tráfico en el momento en que un usuario hace clic en "Iniciar Sesión" después de escribir su contraseña, aquí verías el método POST (usado para enviar datos al servidor).

Host: Es el nombre del servidor principal al que se le están pidiendo los datos. Es fundamental para el servidor, ya que una misma dirección IP física puede alojar múltiples "Hosts" o páginas web distintas.

Protocolo utilizado: Muestra la versión del lenguaje de comunicación. Hoy en día es común ver h2 (HTTP/2) o h3 (HTTP/3 - QUIC), que son más rápidos y eficientes que el clásico HTTP/1.1.

Headers enviados: Son los metadatos ocultos que tu navegador envía al servidor web antes de recibir la página. Entre los más importantes desde el punto de vista de la seguridad y privacidad:

User-Agent: Revela al servidor tu sistema operativo, versión y el navegador exacto que usas. (Útil para que el servidor adapte la página, pero también usado para rastreo o fingerprinting).

Accept-Language: Indica tu idioma preferido, permitiendo al servidor devolverte la versión correcta del sitio.

Cookie: (Si hubieras visitado el sitio antes, aparecería aquí). Envía identificadores de sesión. Si esta petición se hiciera por HTTP en lugar de HTTPS (como vimos en el ejercicio anterior), cualquier persona en la red vería este Header y podría robar la cookie de sesión.



3. ¿Qué riesgos existen al navegar mediante HTTP desde una red Wi-Fi pública?

Navegar mediante HTTP en una red Wi-Fi pública expone al usuario a riesgos críticos debido a que el protocolo transmite toda la información en texto plano (sin cifrar). Al compartir el mismo medio inalámbrico con otros usuarios, cualquier individuo conectado a la misma red puede explotar esta falta de seguridad.

Los riesgos principales incluyen:

Intercepción pasiva (Sniffing): Utilizando herramientas gratuitas de captura de paquetes (como Wireshark), un atacante puede leer exactamente el mismo contenido que la víctima está visualizando o enviando. Esto incluye el historial exacto de navegación, textos de búsqueda, imágenes y documentos descargados.

Robo de credenciales y datos sensibles: Cualquier nombre de usuario, contraseña, número de tarjeta de crédito o información personal introducida en un formulario web HTTP viaja visible y puede ser capturada en tiempo real por el atacante.

Secuestro de sesión (Session Hijacking): Cuando un usuario se autentica en un sitio web, el servidor le asigna una cookie para mantener la sesión abierta. En HTTP, esta cookie viaja sin cifrar. Un atacante puede robar esa cookie, insertarla en su propio navegador y acceder a la cuenta de la víctima sin necesidad de conocer la contraseña.

Ataques de intermediario (Man-in-the-Middle / MitM): Al no contar HTTP con certificados de seguridad que garanticen la integridad de los datos (como lo hace HTTPS), un atacante puede interceptar y modificar el tráfico "al vuelo" antes de que llegue a la pantalla del usuario. Esto permite:

Inyectar código malicioso (malware) en la página legítima.

Redirigir silenciosamente al usuario hacia una página clonada falsa (phishing).

Alterar el contenido de los archivos que la víctima intenta descargar.

4. ¿Cómo cambiaría este escenario utilizando una VPN?

El uso de una VPN (Red Privada Virtual) cambia radicalmente el escenario de riesgo en una red Wi-Fi pública. Como auditor de seguridad, si un usuario implementa una VPN, la inseguridad inherente del medio inalámbrico local deja de ser un factor crítico para sus datos.

A continuación, se describe cómo cambia el escenario a través de estos cuatro pilares fundamentales:

1. Cifrado (El fin del texto plano)
En el escenario anterior, el tráfico HTTP viajaba expuesto. Al activar una VPN, el software (cliente) instalado en el dispositivo del usuario aplica un cifrado de extremo a extremo (utilizando algoritmos robustos como AES-256) a todos los datos antes de que siquiera salgan de la antena Wi-Fi del equipo.
Si el usuario navega por una página HTTP o una aplicación envía contraseñas en texto plano, la VPN empaqueta y cifra esa información localmente. Un atacante con un sniffer capturará los paquetes, pero solo verá una cadena de caracteres criptográficos ininteligibles (texto cifrado), haciendo imposible el robo de contraseñas o el secuestro de sesión (robar cookies).

2. Túnel Seguro (Aislamiento en un entorno hostil)
La VPN crea lo que conceptualmente llamamos un túnel seguro. Funciona encapsulando los paquetes de datos originales dentro de nuevos paquetes protegidos (mediante protocolos como OpenVPN, WireGuard o IPsec).
Este túnel actúa como un "tubo blindado" que atraviesa la red Wi-Fi pública (el entorno hostil) y conecta directamente el dispositivo del usuario con el servidor de la empresa VPN. Debido a este túnel, los ataques de intermediario (Man-in-the-Middle) fracasan: el atacante en la cafetería no puede inyectar malware ni modificar el tráfico al vuelo, porque no puede romper la pared del túnel para manipular los datos que van dentro.

3. Protección del Tráfico (Seguridad integral del dispositivo)
A diferencia de HTTPS, que solo protege la comunicación entre el navegador web y una página específica, la VPN ofrece una protección del tráfico a nivel de sistema.
Todo el tráfico de red del dispositivo es forzado a viajar por el túnel seguro: actualizaciones del sistema operativo, aplicaciones en segundo plano, correos electrónicos y consultas DNS (el directorio telefónico de Internet). El atacante de la red local solo verá una única conexión constante entre la computadora de la víctima y la dirección IP del servidor VPN, ignorando por completo qué aplicaciones o protocolos se están utilizando realmente dentro de ese flujo.

4. Privacidad (Anonimato frente a observadores locales y remotos)
La VPN transforma el escenario brindando una capa doble de privacidad:

Frente a los atacantes y al administrador de la Wi-Fi pública: Ya no pueden rastrear el historial de navegación. Al estar el tráfico cifrado y dentro del túnel, es imposible saber qué páginas web (URLs) o servicios está visitando el usuario.

Frente a los sitios web destino: Cuando el tráfico cifrado llega al servidor VPN, este lo descifra y lo reenvía a la página web final. Para la página de destino (por ejemplo, tu banco o una red social), la visita proviene de la dirección IP del servidor VPN, no de la dirección IP real del usuario. Esto enmascara la ubicación geográfica e identidad del equipo, previniendo el rastreo de terceros.

5. Escribe tus 3 Reglas de Oro para navegar en redes Wi-Fi públicas.


Utilizar siempre una VPN (Red Privada Virtual): Considera cualquier red Wi-Fi pública como un entorno hostil por defecto. Activar una VPN de confianza garantiza que todo el tráfico de tu dispositivo (navegador y aplicaciones) esté encapsulado y cifrado de extremo a extremo, volviendo tus datos ilegibles para cualquier persona que intente interceptarlos en la red local.

No realizar transacciones sensibles: Incluso utilizando conexiones HTTPS, una red pública no es el lugar adecuado para acceder a tu cuenta bancaria, realizar compras con tarjeta de crédito o ingresar a paneles de administración corporativos. Reserva este tipo de operaciones exclusivamente para redes privadas seguras (como tu casa) o utiliza la conexión de datos móviles (4G/5G) de tu teléfono celular.

Desactivar el uso compartido de archivos y las conexiones automáticas: Antes de unirte a la red de una cafetería o aeropuerto, asegúrate de que el uso compartido de archivos e impresoras de tu sistema operativo esté apagado; de lo contrario, otros usuarios podrían acceder a tus carpetas locales. Asimismo, desactiva la opción de "conectarse automáticamente" a redes abiertas para evitar que tu dispositivo se conecte silenciosamente a redes falsas creadas por atacantes (ataques Evil Twin).
