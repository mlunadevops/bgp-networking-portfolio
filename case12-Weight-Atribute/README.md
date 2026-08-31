# Documento Procesado

BITÁCORA TÉCNICA Y GUÍA DE INGENIERÍA BGP
Manipulación de Tráfico mediante el Atributo Weight
CCNP Miguelangel Luna.
1. Introducción y Marco Teórico
El atributo de peso (Weight) es un parámetro propietario de Cisco utilizado en el protocolo BGP
(Border Gateway Protocol) para la selección de la mejor ruta. Sus características principales
son:
 Ámbito local: Tiene significado exclusivamente dentro del router local donde se configura y no
se propaga a través de actualizaciones BGP a vecinos.
 Rango de valores: Un número entero entre 0 y 65,535.
 Valores por defecto: Las rutas originadas localmente por el router reciben un peso de 32,768
por defecto; las rutas aprendidas de vecinos externos o internos reciben un peso de 0 por
defecto.
 Precedencia: Se evalúa en el primer lugar absoluto dentro del algoritmo de decisión de BGP
(por encima de Local Preference, AS_PATH, MED, etc.). Las rutas con un valor de Weight
mayor tienen preferencia absoluta.
2. Topología y Establecimiento de Vecinos BGP
La topología consta de cuatro routers en distintos Sistemas Autónomos (AS 100, AS 200, AS
300 y AS 400). Antes de anunciar prefijos, se establecieron las adyacencias de vecinos BGP en
cada dispositivo.
ROUTER A (AS 100)
router bgp 100
no synchronization
bgp log-neighbor-changes
neighbor 1.1.1.2 remote-as 300
neighbor 3.3.3.2 remote-as 400
no auto-summary
! ROUTER C (AS 300)
router bgp 300
no synchronization
bgp log-neighbor-changes
neighbor 1.1.1.1 remote-as 100
neighbor 2.2.2.2 remote-as 200
no auto-summary
!

! ROUTER B (AS 200)
router bgp 200
no synchronization
bgp log-neighbor-changes
neighbor 2.2.2.1 remote-as 300
neighbor 4.4.4.2 remote-as 400
no auto-summary
!
! ROUTER D (AS 400)
router bgp 400
no synchronization
bgp log-neighbor-changes
neighbor 3.3.3.1 remote-as 100
neighbor 4.4.4.1 remote-as 200
no auto-summary
network 175.10.0.0
!
3. Validaciones Iniciales con Evidencias
Tras la publicación de la red 175.10.0.0/16 en el Router D (AS 400), se verificaron las tablas de
enrutamiento y forwarding en cada nodo de la topología. A continuación se presentan las
evidencias gráficas correspondientes:
3.1 Validación en Router A (RTA)
RTA Muestra la tabla de enrutamiento IP y BGP en RTA, verificando el aprendizaje de la red
175.10.0.0/16 vía el next-hop 3.3.3.2 con Weight 0 por defecto.

3.2 Validación en Router C (RTC - Estado Inicial)
RTC Muestra la tabla de forwarding BGP en RTC con dos caminos disponibles (next-hops 2.2.2.2 y
1.1.1.1) antes de aplicar el atributo Weight.
3.3 Validación en Router B (RTB)
Muestra la propagación exitosa de la ruta hacia la red 175.10.0.0/16 en el Router B.
4. Ingeniería de Tráfico con Atributo Weight en Router C (RTC)
Para alterar la selección de ruta en el Router C (RTC), se configuraron pesos específicos por
vecino dentro de la instancia de BGP:

router bgp 300
neighbor 1.1.1.1 weight 200
neighbor 2.2.2.2 weight 100
4.1 Validación Post-Configuración de Weight
Muestra el cambio en la tabla de forwarding y enrutamiento de RTC tras aplicar los pesos (Weight
200 para el vecino 1.1.1.1 y Weight 100 para 2.2.2.2), validando que el router prioriza
categóricamente la ruta con mayor peso.
