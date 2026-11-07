# 🛡️ Laboratorio 1: Ventana Proactiva — Caso Football Leaks (Manchester City)

**Programa:** AI Cyber Defense Academy — Campuslands x ETHOS IA  
**Módulo:** 2 — Cómo piensa un adversario & Laboratorios de Defensa  
**Laboratorio:** 1 — Ventana Proactiva: Reconstrucción y Señales Tempranas  
**Caso de Estudio:** La intrusión de Football Leaks y los 115 cargos financieros del Manchester City  

---

## 🎯 1. Resumen Ejecutivo (El Porqué del Análisis)

El caso de *Football Leaks* no solo sacudió la industria deportiva mundial al originar los **115 cargos financieros** que la Premier League imputa al Manchester City; desde la perspectiva de la ciberdefensa, representa un **caso de manual sobre el colapso de la ventana proactiva**.

Un atacante no comprometió de golpe 70 millones de documentos confidenciales. Existió una ventana de meses donde el adversario estuvo dentro de los sistemas realizando reconocimiento, pruebas de credenciales y extracción masiva. Este laboratorio reconstruye qué ocurrió desde el Día 0 y demuestra cómo los controles tempranos habrían roto la cadena antes de la catástrofe reputacional y legal.

---

## 🔍 2. Hechos, Hipótesis y Suposiciones

* **HECHOS (Confirmados en procesos judiciales y auditorías):**
  - El atacante obtuvo acceso no autorizado a buzones de correo y servidores de bufetes de abogados, fondos de inversión deportiva y clubes europeos.
  - El método principal de entrada inicial fue el engaño mediante correos dirigidos (*spear-phishing*) y recolección de credenciales corporativas.
  - Los buzones vulnerados carecían de autenticación multifactor (MFA) obligatoria o utilizaban contraseñas reutilizadas.
  - La extracción abarcó más de 3.4 terabytes de datos confidenciales (contratos de jugadores, acuerdos de patrocinio simulados y correspondencia de alta dirección).
  - La información fue posteriormente filtrada a consorcios periodísticos internacionales y utilizada en intentos de extorsión económica, desencadenando investigaciones regulatorias de la UEFA y la Premier League.
* **HIPÓTESIS:**
  - El adversario aprovechó credenciales comprometidas en filtraciones externas previas para intentar accesos directos por fuerza bruta sobre servicios de webmail expuestos a internet.
  - La persistencia dentro de los servidores de correo se mantuvo durante meses debido a la ausencia total de alertas por volumen inusual de descarga de datos.
* **SUPOSICIONES:**
  - No se asume que el atacante contaba con armamento cibernético avanzado o vulnerabilidades de día cero; las brechas se debieron primordialmente a negligencia básica en higiene digital de identidades y accesos.

---

## ⏱️ 3. La Ventana Proactiva: Línea de Tiempo del Incidente

La **Ventana Proactiva** es el margen de tiempo entre las primeras acciones del adversario y el momento en que se produce el impacto destructivo o la filtración pública.

```
 [ DÍA 0 ] Reconocimiento y Phishing
    │      Señales: Correos de suplantación, solicitudes de reseteo de claves.
    │      Control Omitido: Filtro de correo estricto + Autenticación Multifactor (MFA).
    ▼
 [ DÍA 1 - 15 ] Acceso Inicial y Conexión al Webmail
    │      Señales: Inicios de sesión desde ubicaciones geográficas anómalas e IPs desconocidas.
    │      Control Omitido: Bloqueo por Acceso Condicional e inspección de geolocalización.
    ▼
 [ DÍA 15 - 60 ] Movimiento Lateral e Interceptación
    │      Señales: Creación de reglas de reenvío automático de correos en buzones directivos.
    │      Control Omitido: Auditoría de reglas de buzón y principio de mínimo privilegio.
    ▼
 [ DÍA 60 - 180 ] Exfiltración Masiva de Terabytes
    │      Señales: Descargas masivas de adjuntos en horarios no laborales hacia servidores externos.
    │      Control Omitido: Prevención de Pérdida de Datos (DLP) y alertas de umbral de tráfico.
    ▼
 [ IMPACTO ] Publicación en Prensa, Extorsión y 115 Cargos Legales
           (La ventana de defensa se cerró).
```

---

## 🛡️ 4. Matriz de Defensa Proactiva: Qué Controles Pudieron Verlo Antes

| Fase del Ataque | Señal Temprana Observable | Control Preventivo que Debió Existir |
| :--- | :--- | :--- |
| **Ingeniería Social** | Intentos reiterados de phishing dirigidos a correos de directores y abogados. | DMARC/DKIM/SPF estricto para evitar suplantación de dominio y entrenamiento defensivo. |
| **Autenticación** | Peticiones de acceso usando únicamente usuario y contraseña básica. | **MFA Resistente a Phishing:** Ningún acceso a correo permitido sin doble factor. |
| **Persistencia en Correo** | Configuración oculta de buzones que reenviaban copias a cuentas externas. | Alertas automáticas en el SIEM ante cualquier regla de reenvío externo en buzones corporativos. |
| **Exfiltración** | Transferencias continuas de cientos de gigabytes comprimidos hacia el exterior. | Políticas de DLP (*Data Loss Prevention*) que bloqueen la descarga masiva de adjuntos y auditen volúmenes anormales. |

---

## 🤖 5. Conexión Crítica con la Seguridad de la Inteligencia Artificial

> *"Una fuente de datos no debe convertirse automáticamente en una fuente de instrucciones."*

En el caso histórico de Football Leaks (2015-2018), el atacante tardó **años** en recolectar, revisar manualmente y clasificar los 70 millones de documentos para entender qué acuerdos violaban el Fair Play Financiero.

**¿Qué ocurre si este ataque se repite hoy en una organización con Inteligencia Artificial corporativa?**
1. **Extracción automatizada a velocidad de máquina:** Al vulnerar la identidad de un directivo, el atacante no descarga 3 terabytes a ciegas; consulta a un agente de IA corporativo (ej. Copilot o RAG interno):
   - *"Resume los acuerdos donde los fondos de inversión triangulen salarios."*
   - *"Extrae todas las tablas de patrocinio que no coincidan con las cuentas bancarias del club."*
2. **Impacto en minutos, no en años:** La IA sintetiza la evidencia incriminatoria en segundos, reduciendo la ventana proactiva de semanas a casi cero.
3. **Lección del defensor:** Proteger la identidad delegada y los permisos que concedemos a nuestros agentes de IA no es un lujo técnico; es la barrera que evita la ruina reputacional de una organización.

---

## ⚖️ 6. Marco Legal e Institucional

* **Acceso Ilícito vs. Interés Público:** Aunque la información reveló irregularidades financieras corporativas, los tribunales determinaron que el acceso no autorizado, la violación de correspondencia y los intentos de extorsión económica son delitos penales tipificados internacionalmente.
* **Responsabilidad Corporativa:** Las organizaciones tienen la obligación legal y contractual de custodiar la información confidencial de sus empleados, socios y clientes. La negligencia en la protección de correos directivos acarrea multas severas y responsabilidades civiles y penales.
