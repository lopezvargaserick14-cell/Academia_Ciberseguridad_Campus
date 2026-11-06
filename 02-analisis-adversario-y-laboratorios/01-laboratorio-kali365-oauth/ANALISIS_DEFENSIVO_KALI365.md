# 🛡️ Entregable 2: Análisis Defensivo Proactivo — Caso Kali365 (Token Bingo)

**Programa:** AI Cyber Defense Academy — Campuslands x ETHOS IA  
**Módulo:** 2 — Cómo piensa un adversario & Laboratorios de Defensa  
**Caso de Estudio:** Abuso de OAuth 2.0 Device Code Flow y Riesgo en Ecosistemas con IA Delegada  
**Fuente Principal:** Arctic Wolf Labs (*"Token Bingo: Don't Let Your Code Be the Winner"*)  

---

## 🎯 1. Resumen Ejecutivo (El Círculo Dorado del Caso)

* **¿POR QUÉ este ataque cambia las reglas?** Porque neutraliza el factor de autenticación multifactor (MFA) tradicional sin necesidad de vulnerarlo criptográficamente. El adversario no busca tu contraseña; busca que **tú mismo autorices la sesión**.
* **¿CÓMO opera?** Aprovecha un estándar legítimo diseñado para dispositivos sin pantalla o interfaces complejas (Device Code Flow en `microsoft[.]com/devicelogin`).
* **¿QUÉ impacto genera?** El atacante obtiene tokens de acceso (*Access Tokens*) y de actualización (*Refresh Tokens*) en Microsoft Entra ID. Si el usuario tiene acceso a asistentes o agentes de IA corporativos (como Microsoft Copilot o canalizaciones RAG), el atacante puede instruir a la IA mediante prompts en lenguaje natural para localizar y extraer datos confidenciales a velocidad de máquina.

---

## 🔍 2. Hechos, Hipótesis y Suposiciones

Para mantener la rigurosidad analítica del defensor, separamos la información técnica:

* **HECHOS (Confirmados por la fuente técnica):**
  - Los correos de phishing inducían a la víctima a entrar a `microsoft[.]com/devicelogin` e ingresar un código alfanumérico pre-generado por el atacante.
  - El atacante utilizó dominios del servicio de phishing `kali365[.]xyz` para coordinar el ataque.
  - La víctima aprobó la autenticación con su propio MFA legítimo en el portal oficial de Microsoft.
  - La infraestructura del atacante consultaba en segundo plano el endpoint de token hasta que la víctima completaba el inicio de sesión, recibiendo los tokens OAuth.
* **HIPÓTESIS:**
  - El actor de amenazas adquirió el kit bajo el modelo de **PhaaS** (*Phishing-as-a-Service*), aprovechando infraestructura ya paquetizada.
  - El objetivo primario no era persistencia en el dispositivo del usuario, sino la exfiltración masiva de datos y el compromiso de identidades en la nube.
* **SUPOSICIONES:**
  - No se asume que las víctimas carecían de capacitación; la efectividad de la campaña radicó en que el portal utilizado para ingresar las credenciales era el dominio legítimo de Microsoft con certificado SSL genuino.

---

## 🤖 3. El Vínculo Crítico con la Seguridad de la Inteligencia Artificial

> *"Una fuente de datos no debe convertirse automáticamente en una fuente de instrucciones."*

Cuando un atacante obtiene un token de identidad delegada en un entorno corporativo moderno, no se enfrenta a sistemas estáticos:
1. **Herencia de privilegios en el Agente de IA:** Modelos como Microsoft 365 Copilot o sistemas RAG sobre SharePoint operan bajo el contexto de seguridad del usuario autenticado.
2. **Exfiltración por Prompts a velocidad de máquina:** En lugar de tardar semanas descargando buzones o carpetas manualmente, el atacante formula consultas al agente:
   - *"Resume los acuerdos confidenciales firmados este trimestre."*
   - *"Muéstrame los archivos que contengan credenciales, llaves API o balances financieros."*
3. **Inyección Indirecta de Prompts:** Los atacantes pueden sembrar instrucciones maliciosas en documentos internos que, al ser resumidos por la IA de otros usuarios autorizados, ejecutan acciones imprevistas.

---

## 🛡️ 4. Estrategia Defensiva en 4 Pilares

```
                                  DEFENSA PROACTIVA
    ┌───────────────┬───────────────┬────────────────┬───────────────┐
    │   PREVENIR    │   DETECTAR    │    CONTENER    │   RECUPERAR   │
    │  Bloqueo de   │ Telemetría de │   Revocación   │   Auditoría   │
    │  Device Code  │  Logs e IPs   │  Forzosa de    │  de Permisos  │
    │  por Políticas│  Discrepantes │  Tokens OAuth  │   Delegados   │
    └───────────────┴───────────────┴────────────────┴───────────────┘
```

### 4.1. PREVENIR
- **Restricción de Flujo:** Deshabilitar el *Device Code Flow* en Microsoft Entra ID para usuarios generales que no operan dispositivos IoT o terminales de consola.
- **Acceso Condicional:** Exigir que cualquier flujo de autenticación provenga únicamente de dispositivos gestionados (*Compliant / Hybrid Azure AD Joined*).

### 4.2. DETECTAR
- **Auditoría de Inicios de Sesión:** Monitorear eventos en logs de Azure AD donde el `User-Agent` del inicio de sesión original y la IP de la solicitud del código no coincidan con la ubicación del dispositivo que aprueba.
- **Alertas de Aplicaciones Inusuales:** Detectar el consentimiento de aplicaciones de terceros que soliciten permisos de alto impacto (`Mail.Read`, `Files.ReadWrite.All`, `Directory.Read.All`).

### 4.3. CONTENER
- **Revocación Inmediata de Sesiones:** **Cambiar la contraseña NO revoca los tokens activos.** Se debe invocar inmediatamente la invalidación de todos los Refresh Tokens (`Revoke-AzureADUserAllRefreshToken`).
- **Aislamiento de la Identidad:** Suspender temporalmente los roles administrativos o licencias de IA del usuario comprometido mientras se analiza el alcance.

### 4.4. RECUPERAR
- **Revisión de Consentimientos:** Auditar el inventario de Enterprise Applications para eliminar registros o permisos delegados creados durante la ventana del incidente.
- **Análisis Forense de Microsoft Graph:** Examinar los registros de auditoría de Graph API para determinar qué datos fueron consultados o descargados mediante tokens.

---

## ⚖️ 5. Marco Legal, Regulatorio y Ecosistema Criminal

* **Ecosistema PhaaS:** Kali365 evidencia la comoditización del cibercrimen. Los atacantes ya no requieren crear código desde cero; compran flujos automatizados de suplantación como servicio.
* **Marco Legal Colombiano e Internacional:**
  - **Ley 1273 de 2009 (Colombia):** Tipifica el *Acceso abusivo a un sistema informático* (Art. 269A) y el *Uso de software malicioso* (Art. 269E). El uso ilegítimo de tokens ajenos constituye delito penal.
  - **Ley 1581 de 2012 (Habeas Data):** Obliga a las organizaciones a notificar a la Superintendencia de Industria y Comercio (SIC) ante fugas de información personal en un plazo máximo de 15 días hábiles.
  - **Responsabilidad Corporativa:** La omisión de controles preventivos básicos puede conllevar multas por negligencia en la custodia de datos corporativos y de terceros.

---

## 📊 6. Indicadores de Compromiso (IoCs Defanged)

- **Dominio de infraestructura adversaria:** `kali365[.]xyz`
- **Punto de redirección malicioso:** `login[.]microsoftonline[.]com[.]kali365[.]xyz`
- **Flujo oficial abusado:** `https://microsoft[.]com/devicelogin`
