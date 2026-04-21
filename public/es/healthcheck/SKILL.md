---
id: AUDITORIA_SEGURIDAD
name: Auditoría de Seguridad
description: "Auditoría y endurecimiento de seguridad del host. Úsalo cuando se soliciten auditorías de seguridad, endurecimiento de firewall/SSH/actualizaciones, revisión de exposición de red, o comprobaciones de estado del servidor (portátil, workstation, VPS, Raspberry Pi)."
icon: 🔒
category: security
created_at: "2026-04-18"
updated_at: "2026-04-18"
---

# Auditoría de Seguridad del Host

## Resumen

Evalúa y endurece el host, alineándolo a la tolerancia de riesgo del usuario sin romper el acceso. Trata el endurecimiento del SO como un conjunto de pasos separado y explícito.

## Reglas principales

- Requiere aprobación explícita antes de cualquier acción que cambie estado.
- No modifiques la configuración de acceso remoto sin confirmar cómo se conecta el usuario.
- Prefiere cambios reversibles y escalonados con plan de rollback.
- Si el rol/identidad es desconocido, solo proporciona recomendaciones.
- Formato: cada conjunto de opciones debe estar numerado para que el usuario pueda responder con un solo dígito.
- Se recomienda tener copias de seguridad del sistema; intenta verificar su estado.

## Flujo de trabajo (sigue el orden)

### 0) Comprobación del modelo (no bloqueante)

Antes de empezar, verifica el modelo actual. Si está por debajo del estado del arte (Opus 4.5, GPT 5.4+), recomienda cambiar. No bloquees la ejecución.

### 1) Establecer contexto (solo lectura)

Intenta inferir los puntos 1-5 del entorno antes de preguntar. Prefiere preguntas simples y no técnicas si necesitas confirmación.

Determina (en orden):

1. Sistema operativo y versión (Linux/macOS/Windows), contenedor vs host.
2. Nivel de privilegio (root/admin vs usuario normal).
3. Método de acceso (consola local, SSH, RDP, tailnet).
4. Exposición de red (IP pública, proxy inverso, túnel).
5. Estado del firewall y puertos abiertos.
6. Sistema y estado de copias de seguridad (Time Machine, imágenes del sistema).
7. Contexto de despliegue (aplicación local, gateway remoto, contenedor/CI).
8. Estado del cifrado de disco (FileVault/LUKS/BitLocker).
9. Estado de las actualizaciones automáticas de seguridad del SO.
10. Modo de uso (estación de trabajo personal vs servidor sin cabeza vs otro).

Pide permiso una sola vez para ejecutar comprobaciones de solo lectura. Si se concede, ejecútalas por defecto y solo haz preguntas para los elementos que no puedas inferir.

Si debes preguntar, usa formulaciones no técnicas:

- "¿Usas Mac, Windows o Linux?"
- "¿Estás conectado directamente al equipo, o te conectas desde otro ordenador?"
- "¿Es este equipo accesible desde internet, o solo en tu red local?"
- "¿Tienes copias de seguridad activadas (Time Machine, etc.) y están al día?"
- "¿Está activado el cifrado de disco?"
- "¿Están activadas las actualizaciones automáticas de seguridad?"

### 2) Ejecutar auditorías de seguridad (solo lectura)

Ejecuta comprobaciones según el SO:

```bash
# Estado del SO
uname -a         # Linux
sw_vers          # macOS

# Puertos en escucha
ss -ltnp         # Linux
lsof -nP -iTCP -sTCP:LISTEN  # macOS

# Estado del firewall
ufw status       # Linux (Ubuntu/Debian)
/usr/libexec/ApplicationFirewall/socketfilterfw --getglobalstate  # macOS
```

Ofrece revisar también:
1. Permisos de ficheros sensibles (claves SSH, configuración)
2. Servicios en ejecución innecesarios
3. Usuarios con privilegios elevados

### 3) Determinar tolerancia al riesgo

Tras conocer el contexto del sistema, pregunta al usuario su perfil de riesgo (opciones numeradas):

1. **Balanceado/Workstation** (más común): firewall activo con valores razonables, acceso remoto restringido a LAN o tailnet.
2. **VPS Endurecido**: firewall de denegación por defecto, puertos mínimos, SSH solo con clave, sin login de root, actualizaciones automáticas.
3. **Comodidad para desarrollador**: más servicios locales permitidos, avisos explícitos de exposición, igualmente auditado.
4. **Personalizado**: restricciones definidas por el usuario.

### 4) Producir un plan de remediación

Proporciona un plan que incluya:

- Perfil objetivo
- Resumen de la postura actual
- Brechas vs objetivo
- Remediación paso a paso con comandos exactos
- Estrategia para preservar el acceso y rollback
- Riesgos y posibles escenarios de bloqueo
- Notas sobre mínimos privilegios
- Notas sobre higiene de credenciales

Muestra siempre el plan antes de realizar cualquier cambio.

### 5) Ofrecer opciones de ejecución

Ofrece una de estas opciones (numeradas):

1. Hazlo por mí (guiado, con aprobaciones paso a paso)
2. Solo mostrar el plan
3. Corregir solo los problemas críticos
4. Exportar comandos para más tarde

### 6) Ejecutar con confirmaciones

Para cada paso:

- Muestra el comando exacto
- Explica el impacto y el rollback
- Confirma que el acceso seguirá disponible
- Detente ante salida inesperada y pide orientación

### 7) Verificar y reportar

Vuelve a comprobar:

- Estado del firewall
- Puertos en escucha
- Que el acceso remoto sigue funcionando
- Re-ejecuta las auditorías de solo lectura
