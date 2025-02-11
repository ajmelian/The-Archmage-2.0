# The Archmage: Reincarnation from Hell 2.0

## ¿Qué es?

The Archmage es un juego de rol y estrategia (MMORPG) ambientado en un universo místico en el que cada jugador se convierte en un poderoso mago y rey de un imperio. Al iniciar su aventura, el jugador elige entre distintos tipos de magia disponibles (Oscura, Erradicación, Ascendente, Fantasmal, y Natural) cada una de los cuales otorga recursos y habilidades únicas, definiendo su estilo de juego.

En este mundo, la magia no es solo un recurso, sino el motor que impulsa batallas épicas, misiones intrigantes y desafíos estratégicos. Los jugadores explorarán reinos enigmáticos, descubrirán secretos ancestrales y se enfrentarán a enemigos formidables, perfeccionando sus habilidades y personalizando su experiencia de juego; todo mientras se sumergen en una experiencia mágica donde la estrategia, la aventura y el dominio de la magia se unen para forjar leyendas. Cada hechizo, cada batalla y cada elección determinan el destino en este vibrante universo lleno de misterios y desafíos.

Inicialmente desarrollado por la extinta empresa coreana MARI Telecomunications (cerrada en 2004), esta versión del juego tiene como objetivo rescatar el diseño y jugabilidad de su primera versión, adaptándola a la tecnología actual, y permitiendo su disfrute en cualquier dispositivo y en cualquier idioma.

## Roadmap

Para llevar a cabo este proyecto, se desarrollarán los siguientes módulos:
1. Gestión de Sesiones y Mensajería (Interna y Externa)
2. Gestión de Turnos
3. Módulo de Magia: 
      - Investigación, Invocación, Asignación y Dispersión de Hechizos;
      - Objetos
  - Habilidades
  - Maná
4. Módulo de Interior:
  - Exploración de Tierras
  - Construcción y Destrucción de Edificios
  - Recaudación de Impuestos
  - Reclutamiento y Despidos de Unidades
  - Defensa del Reino: Asignación de Hechizos y objetos para la defensa del Reino
5. Módulo de Sociedad:
  - Gremios: Listado, creación, gestión, y eliminación de Gremios
  - Mensajería gremial
  - Chat Gremial
  - Ranking
6. Módulo de Diplomacia:
  - Alianzas entre Magos: Creación, solicitudes, y eliminación
7. Módulo de Guerra:
  - Defensa del Reino: Asignación de Unidades para la Defensa del Reino
  - Listados de Magos con acceso a batalla (no se podrá combatir con magos demasiado poderosos o demasiados débiles, además de aquellos que estén en periodo de gracia o inactividad)
  - Batallas:
    a. Infiltración y Robos
    b. Guerra
  - Espionaje
  - Informe de Batalla o Espionaje
8. Módulo de Oráculo:
  - Mercado Negro
  - Arena
  - Cementerio
  - Clasificación o Ranking
  - Enciclopedia
  
## Pautas de Desarrollo

El desarrollo de este MMORPG se realizará con el seguimiento estricto de las siguientes pautas y metodologías de desarrollo:
- Empleo de CamelCase y Tipado de funciones, métodos y variables.
- Documentado bajo el estándar PHPDoc.
- Todo parámetro de entrada y salida será verificado mediante regex tanto en front-end como en back-end.
- Las sesiones estarán securizadas y protegidas ante ataques CSRF, XSS, etc.
- No se permiten las sesiones multidispositivos, salvo aquellas donde los dispositivos sean de distinta tipología; por ejemplo, se permite un ordenador y un smartphone, pero no dos ordenadores.
- Se controlará la ubicación y uso de VPN o Proxies en la medida de lo posible.
- Se cargarán los archivos mínimos necesarios en cada vista, optimizando estas al máximo, para una mejor experiencia de usuario y evitar el código infrautilizado.
- Toda librería externa (JS, CSS, etc.) deberá ser almacenada en el servidor, en el directorio correspondiente, y cargada sólo cuando sea estrictamente necesario.
- Los datos personales de cada usuario serán los mínimos indispensables para el cumplimiento de lo anteriormente descrito y el funcionamiento de la aplicación.


### Objetivos y Funcionalidades

- **Gestión de Usuarios y Autenticación:**  
  Se ha extendido el paquete Shield para permitir el registro de usuarios (jugadores) con validación de datos (mediante expresiones regulares y protección CSRF) y la generación de un token de verificación enviado por email. Esto garantiza que los usuarios verifiquen su correo antes de poder acceder a la zona del juego.

- **Control de Sesiones:**  
  El sistema de sesiones incorpora medidas de seguridad como la regeneración del ID de sesión, la validación del tipo de dispositivo (desktop, mobile o tablet) y la prevención de sesiones duplicadas en el mismo tipo de dispositivo. En caso de detectar un acceso duplicado, se notifica al usuario vía email y se le permite confirmar la acción para cerrar las sesiones anteriores.

- **Administración de Baneos:**  
  Se ha implementado un mecanismo para banear usuarios. Cuando un administrador decide banear a un usuario, se crea un registro en la tabla de baneos (junto con información como el motivo, quién lo baneó, la fecha y la IP) y se actualiza la tabla de usuarios para reflejar el estado de baneo. Los usuarios baneados no podrán iniciar sesión y, si lo intentan, se les informará del baneo junto con el motivo y se les sugerirá contactar con soporte.

- **Carga Modular de Recursos (CSS y JS):**  
  Se ha diseñado un sistema para cargar de forma modular los ficheros CSS y JS, diferenciando entre:
  - Recursos comunes a toda la aplicación.
  - Recursos para el área sin sesión (landing, login, registro, etc.) y para el área con sesión iniciada (el juego).
  - Archivos comunes a un controlador concreto y archivos propios de un método o función específica.  
  Esto optimiza el rendimiento al cargar únicamente los recursos necesarios para cada vista.

- **Internacionalización y Gestión de Mensajes:**  
  Todos los textos mostrados al usuario se centralizan en archivos de idioma (por ejemplo, `Auth.php` y `Messages.php` en `app/Language/es/`), permitiendo una fácil adaptación a otros idiomas y un mantenimiento centralizado de los mensajes.

### Estructura y Organización

El proyecto se organiza en varios directorios clave:
- **app/Controllers:** Contiene los controladores de autenticación (LoginController, RegisterController, VerifyController), administración (UserManagement) y el Dashboard de usuario.
- **app/Database/Migrations:** Incluye migraciones para crear y modificar las tablas de la base de datos (como bans, active_sessions, players y modificaciones en users).
- **app/Language/es:** Ficheros de idioma para centralizar los mensajes y textos.
- **app/Views:** Vistas para las diferentes áreas, como autenticación (login, register, confirm_override) y el dashboard.
- **assets:** Directorio con subcarpetas para CSS y JS, que contiene recursos globales para áreas sin sesión y con sesión, así como archivos específicos por controlador y método.

### Conclusión

El objetivo principal del código es proporcionar una base sólida y escalable para la gestión de usuarios en "The Archmage". Esto se logra mediante:
- Un sistema de autenticación y registro seguro y validado.
- Un control de sesiones que mejora la seguridad y la experiencia del usuario.
- Una administración de baneos que permite gestionar el acceso de forma precisa.
- La carga modular de recursos para optimizar el rendimiento.
- La centralización de mensajes para facilitar la internacionalización.
