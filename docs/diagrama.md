# Explicación del Diagrama de Infraestructura

El diagrama representa la infraestructura de red del proyecto, mostrando cómo se conectan los diferentes equipos y redes virtuales:

![Diagrama de la infraestructura](../imagen/Diagram.png)




- **Router (R-N03):** Es el dispositivo central que conecta y enruta el tráfico entre las distintas redes (NAT, DMZ e INTRANET).
- **Red NAT:** Permite la salida a internet de los equipos internos a través del router.
- **Red DMZ:** Zona desmilitarizada donde se ubican los servidores accesibles desde el exterior, como el servidor web (W-N03), el servidor de base de datos (B-N03) y el servidor FTP (F-N03).
- **Red INTRANET:** Red interna donde se encuentran los clientes, como el PC con Windows (PC-1.03) y el PC con Linux (PC-2.03).
- **Conexiones:** El diagrama muestra cómo cada equipo está conectado a su respectiva red y cómo todas las redes se interconectan a través del router.

