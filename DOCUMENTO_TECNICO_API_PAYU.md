## 📋 Documento Técnico - Sistema API PAYU Banco Unión S.A.

# DOCUMENTO TÉCNICO
## Sistema de Conexión API PAYU
### Banco Unión S.A. - Vicepresidencia de Tecnología

**Versión:** 1.3  
**Fecha:** 2025  
**Clasificación:** Uso Interno  
**Estado:** Producción  

---

## 1. RESUMEN EJECUTIVO

### 1.1 Objetivo del Proyecto
Desarrollar una interfaz web para la conexión y consulta al API de PAYU que permita a los equipos del Banco Unión S.A. realizar consultas transaccionales en ambientes de QA y Producción de manera segura y eficiente.

### 1.2 Alcance
- Sistema web monolítico HTML/CSS/JavaScript
- Conexión dual a PAYU QA y Producción
- Cuatro tipos de consultas transaccionales
- Gestión de credenciales por ambiente
- Sistema de proxy configurable

### 1.3 Stakeholders
- **Vicepresidencia de Tecnología**: Desarrollo y mantenimiento
- **Operaciones Bancarias**: Usuarios finales
- **Departamento Legal**: Cumplimiento normativo
- **Seguridad Informática**: Validación de accesos

---

## 2. ARQUITECTURA TÉCNICA

### 2.1 Diagrama de Arquitectura

┌─────────────────────────────────────────────────────────────┐
│ NAVEGADOR DEL USUARIO │
│ ┌──────────────────────────────────────────────────────┐ │
│ │ APLICACIÓN WEB (HTML5) │ │
│ │ • HTML/CSS/JavaScript Vanilla │ │
│ │ • localStorage para persistencia │ │
│ │ • Fetch API para comunicaciones │ │
│ └──────────────────────────────────────────────────────┘ │
└───────────────────────────┬─────────────────────────────────┘
│
┌───────────────────────────┼─────────────────────────────────┐
│ PROXY (Opcional) │ │
│ ┌───────────────────────┼─────────────────────────────┐ │
│ │ • CORS Anywhere │ │ │
│ │ • Proxy Corporativo │ │ │
│ └───────────────────────┼─────────────────────────────┘ │
└───────────────────────────┼─────────────────────────────────┘
│
┌───────────────────────────┼─────────────────────────────────┐
│ PAYU API │ │
│ ┌───────────────────────▼─────────────────────────────┐ │
│ │ • QA: sandbox.api.payulatam.com │ │
│ │ • Prod: api.payulatam.com │ │
│ │ • Endpoint: /reports-api/4.0/service.cgi │ │
│ └─────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘

### 2.2 Stack Tecnológico
| Componente | Tecnología | Versión | Propósito |
|------------|------------|---------|-----------|
| Frontend | HTML5 | - | Estructura de la aplicación |
| Estilos | CSS3 | - | Diseño responsivo |
| Lógica | JavaScript ES6+ | - | Funcionalidad principal |
| Almacenamiento | localStorage | - | Persistencia de configuraciones |
| Comunicación | Fetch API | - | Peticiones HTTP |
| Iconografía | Font Awesome | 6.4.0 | Iconos UI |
| Fuentes | Segoe UI | - | Tipografía corporativa |

### 2.3 Flujo de Datos
1. **Inicialización**: Carga configuración desde localStorage
2. **Autenticación**: Validación de credenciales por ambiente
3. **Consulta**: Construcción de payload JSON según tipo
4. **Envío**: Request HTTP POST al endpoint correspondiente
5. **Procesamiento**: Parseo y visualización de respuesta
6. **Persistencia**: Guardado de logs y configuración

---

## 3. ESPECIFICACIONES TÉCNICAS

### 3.1 Requisitos del Sistema
#### 3.1.1 Requisitos Mínimos
- Navegador web moderno (Chrome 80+, Firefox 75+, Edge 80+)
- Conexión a internet
- JavaScript habilitado
- Resolución mínima: 320px

#### 3.1.2 Requisitos Recomendados
- Chrome 90+ o Firefox 85+
- CPU: 2+ núcleos
- RAM: 2GB+
- Conexión: 10 Mbps+

### 3.2 Especificaciones de Seguridad
- **Almacenamiento**: Credenciales en localStorage del navegador
- **Comunicaciones**: HTTPS obligatorio
- **CORS**: Manejo mediante proxy configurable
- **Exposición**: Sin transmisión a servidores externos

### 3.3 Límites y Capacidades
| Parámetro | Límite | Notas |
|-----------|--------|-------|
| Tamaño de respuesta | 5MB | Límite del navegador |
| Timeout de petición | 30 segundos | Configurable |
| Credenciales almacenadas | 4 sets | QA y Prod por separado |
| Historial de errores | Último error | No se almacena histórico |

---

## 4. DISEÑO DE LA APLICACIÓN

### 4.1 Estructura de Componentes
```javascript
// Estructura modular de la aplicación
const appStructure = {
  config: {
    credentials: {
      qa: { apiLogin, apiKey, merchantId, accountId },
      prod: { apiLogin, apiKey, merchantId, accountId }
    },
    environment: 'qa' | 'production',
    proxy: { enabled, url, timeout, logRequests }
  },
  
  ui: {
    tabs: ['Configuración', 'Prueba', 'Proxy', 'Credenciales'],
    queryTypes: ['ORDER_DETAIL', 'TRANSACTION', 'REFERENCE', 'PING'],
    notifications: { success, error, info }
  },
  
  services: {
    api: {
      urls: { qa, production },
      makeRequest(data, env),
      handleResponse(response),
      handleError(error)
    },
    storage: {
      loadConfig(),
      saveConfig(),
      clearAll()
    }
  }
};

### 4.2 Esquema de Colores Corporativos

Variable CSS	    Color HEX	 Uso
--primary-color	    #2c3e50	Encabezados, botones primarios
--secondary-color	#1a5276	Fondo header, elementos secundarios
--accent-color	    #3498db	Botones, links, elementos interactivos
--success-color	    #27ae60	Notificaciones exitosas, estado OK
--warning-color	    #f39c12	Advertencias, elementos destacados
--danger-color	    #e74c3c	Errores, botones destructivos

### 4.3 Responsive Design

Breakpoint	    Dispositivo	    Características
< 576px	        Mobile	        Columnas simples, menú compacto
576px - 768px	Tablet	        Diseño adaptativo, dos columnas
768px - 992px	Desktop pequeño Grid completo, elementos expandidos
> 992px	        Desktop	        Máximo ancho 1200px, espaciado amplio

### 5. API PAYU - ESPECIFICACIONES

### 5.1 Endpoints

Ambiente	URL	Propósito
QA/Sandbox	https://sandbox.api.payulatam.com/reports-api/4.0/service.cgi	Pruebas y desarrollo
Producción	https://api.payulatam.com/reports-api/4.0/service.cgi	Operaciones reales

### 5.2 Comandos Implementados

{
  "PING": {
    "description": "Prueba de conectividad básica",
    "parameters": "Ninguno",
    "response": { "code": "SUCCESS", "result": { "payload": "ping" } }
  },
  "ORDER_DETAIL": {
    "description": "Consulta por ID de orden",
    "parameters": { "details": { "orderId": "number" } },
    "response": "Detalles completos de la orden"
  },
  "TRANSACTION_RESPONSE_DETAIL": {
    "description": "Consulta por ID de transacción",
    "parameters": { "details": { "transactionId": "string" } },
    "response": "Estado y detalles de transacción"
  },
  "ORDER_DETAIL_BY_REFERENCE_CODE": {
    "description": "Consulta por código de referencia",
    "parameters": { "details": { "referenceCode": "string" } },
    "response": "Orden asociada al código"
  }
}

### 5.3 Estructura de Request

{
  "test": "boolean",           // true para QA, false para Prod
  "language": "string",        // "en" o "es"
  "command": "string",         // Comando a ejecutar
  "merchant": {
    "apiLogin": "string",      // Credencial de API
    "apiKey": "string",        // Llave de API
    "merchantId": "string",    // ID del comercio (opcional para PING)
    "accountId": "string"      // ID de cuenta (opcional)
  },
  "details": {}                // Parámetros específicos por comando
}

### 5.4 Estructura de Response

{
  "code": "string",            // "SUCCESS" o "ERROR"
  "error": "string|null",      // Mensaje de error si aplica
  "result": {                  // Datos de la respuesta
    "payload": {}              // Contenido específico
  }
}

### 5.5 Códigos de Error Comunes

Código	Mensaje	                        Causa Probable	                        Solución
ERROR	"Invalid credentials"	        Credenciales incorrectas	            Verificar credenciales por ambiente
ERROR	"Formato de petición inválido"	JSON mal formado	                    Validar estructura del request
ERROR	HTTP 401	                    No autorizado	                        Credenciales expiradas o inválidas
ERROR	HTTP 403	                    Prohibido	                            Problemas de CORS o proxy
ERROR	HTTP 500	                    Error interno PAYU	                    Contactar soporte PAYU

### 6. CONFIGURACIÓN Y DESPLIEGUE

### 6.1 Instalación Local

# 1. Crear directorio del proyecto
mkdir payu-api-banco-union
cd payu-api-banco-union

# 2. Crear archivos principales
touch index.html README.md LICENSE

# 3. Copiar código (index.html contiene todo el frontend)

# 4. Agregar logo corporativo
# Nombre del archivo: LOGO.png
# Tamaño recomendado: 60x60px mínimo

### 6.2 Configuración Inicial

1. Credenciales QA:

   - Obtener de: https://sandbox.dashboard.payulatam.com/

   - Configuración → API Keys

   - Copiar API Login y API Key

2. Credenciales Producción:

   - Obtener de: https://dashboard.payulatam.com/

   - Configuración → API Keys

   - Credenciales DIFERENTES a QA

3. Merchant ID y Account ID:

   - Consultar con el equipo de integraciones

   - Son diferentes por ambiente

### 6.3 Despliegue en Entornos

### 6.3.1 Desarrollo/QA

- Archivo local en navegador

- Sin servidor web requerido

- Testing con credenciales de sandbox

### 6.3.2 Producción

- Hosting en servidor interno

- Acceso restringido por VPN

- SSL/TLS obligatorio

### 6.4 Script de Backup

// backup-config.js - Script para exportar configuración
function exportConfiguration() {
  const config = JSON.parse(localStorage.getItem('payuApiConfig') || '{}');
  const blob = new Blob([JSON.stringify(config, null, 2)], {
    type: 'application/json'
  });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = `payu-config-${new Date().toISOString().split('T')[0]}.json`;
  a.click();
}

### 7. MANTENIMIENTO Y SOPORTE

### 7.1 Monitoreo

Métrica	                  Umbral	Acción
Tiempo de respuesta API	  > 5s	    Verificar conectividad
Tasa de errores	          > 10%	    Revisar credenciales
Uso de memoria	          > 100MB	Limpiar localStorage

### 7.2 Procedimientos de Mantenimiento

### 7.2.1 Mensual

- Verificar vigencia de credenciales

- Limpiar localStorage de usuarios

- Actualizar documentación

### 7.2.2 Trimestral

- Revisar compatibilidad con navegadores

- Validar endpoints de PAYU

- Auditoría de seguridad

### 7.3 Troubleshooting

Problema: "Invalid credentials" en Producción

# Diagnóstico:
1. Verificar que se usan credenciales de Producción ✓
2. Confirmar en dashboard.payulatam.com ✓
3. Validar formato (sin espacios, caracteres especiales) ✓
4. Verificar restricciones de IP en credenciales ✓

# Solución:
1. Generar nuevas credenciales en dashboard de Producción
2. Actualizar en configuración de la aplicación
3. Probar conexión

Problema: Error de CORS

# Soluciones:
1. Habilitar proxy en configuración avanzada
2. Usar extensión CORS para desarrollo
3. Configurar proxy corporativo
4. Contactar a infraestructura para whitelist

### 7.4 Escalación de Problemas

Usuario → 
  ↓
Soporte N1 (Equipo desarrollo) → 
  ↓
Soporte N2 (Infraestructura) → 
  ↓
Soporte N3 (PAYU) → 
  ↓
Gerencia TI

### 8. SEGURIDAD Y COMPLIANCE

### 8.1 Consideraciones de Seguridad

1. Almacenamiento Local:

- Credenciales en localStorage (vulnerable a XSS)

- Recomendación: Limpiar después de uso

- No usar en computadoras públicas

2. Comunicaciones:

- Todas las peticiones vía HTTPS

- Validación de certificados SSL

- No almacenamiento de logs sensibles

3. Accesos:

- Restringir por IP corporativa

- Autenticación de red adicional

- Logs de acceso (opcional)

### 8.2 Cumplimiento Normativo

- LFPDPPP: Tratamiento de datos personales

- SOX: Controles internos financieros

- PCI DSS: Pagos con tarjeta (indirecto)

- Políticas Internas Banco: Seguridad informática

### 8.3 Recomendaciones de Hardening

// Mejoras de seguridad sugeridas
const securityImprovements = {
  encryption: 'Implementar cifrado AES para credenciales',
  sessionTimeout: 'Agregar expiración de sesión',
  auditLogs: 'Registro de consultas realizadas',
  inputValidation: 'Sanitización estricta de inputs',
  csrfProtection: 'Tokens para acciones sensibles'
};

### 9. PRUEBAS Y CALIDAD

### 9.1 Matriz de Pruebas


Tipo	    Escenario	                    Resultado Esperado	                Ambiente
Unitarias	Carga de configuración	        Configuración cargada correctamente	Local
Integración	Conexión PAYU QA	            Respuesta SUCCESS con PING	        QA
Integración	Conexión PAYU Prod	            Respuesta SUCCESS con PING	        Prod
Funcionales	Consulta por Order ID	        Datos completos de orden	        QA
Carga	    Múltiples consultas simultáneas	Respuesta en < 5 segundos	        QA
Seguridad	Inyección en parámetros	        Error controlado	                QA

### 9.2 Criterios de Aceptación

1. ✅ Conexión exitosa a ambos ambientes PAYU

2. ✅ Consultas retornan datos esperados

3. ✅ Manejo apropiado de errores

4. ✅ Interfaz responsiva en dispositivos objetivo

5. ✅ Persistencia correcta de configuración

6. ✅ Copiado al portapapeles funcional

7. ✅ Notificaciones claras al usuario

### 9.3 Métricas de Calidad

- Cobertura de código: 85%+ (funcionalidades críticas)

- Tiempo de carga: < 3 segundos

- Score Lighthouse: > 90 en performance

- Compatibilidad: Chrome, Firefox, Edge actuales

### 10. DOCUMENTACIÓN ADICIONAL

### 10.1 Referencias

Documentación Oficial PAYU - https://developers.payulatam.com/latam/es/docs/integrations/api-integration/queries-api.html

API Endpoints - Pruebas: https://sandbox.api.payulatam.com/reports-api/4.0/service.cgi
                Producción: https://api.payulatam.com/reports-api/4.0/service.cgi

### 10.2 Glosario

Término	    Definición
API Login	Identificador único para autenticación PAYU
API Key	    Llave secreta para firmar peticiones
Merchant ID	Identificador del comercio en PAYU
Account ID	Identificador de cuenta específica
PING	    Comando de prueba de conectividad
CORS	    Cross-Origin Resource Sharing

### 10.3 Contactos

Área	        Contacto	                        Responsabilidad
Desarrollo	    [equipo.desarrollo@bancounion.com]	Mantenimiento código
Operaciones	    [operaciones.pagos@bancounion.com]	Uso operativo
Infraestructura	[infra.ti@bancounion.com]	        Despliegue y red
Soporte PAYU	[soporte@payulatam.com]	            Problemas con API

### 11. ANEXOS

### 11.1 Códigos de Estado HTTP

200: OK - Petición exitosa
400: Bad Request - JSON mal formado
401: Unauthorized - Credenciales inválidas
403: Forbidden - Acceso denegado
500: Internal Server Error - Error PAYU
504: Gateway Timeout - Timeout configurado

### 11.2 Estructura de Logs

{
  "timestamp": "2025-01-15T10:30:00Z",
  "environment": "production",
  "command": "ORDER_DETAIL",
  "parameters": { "orderId": 123456 },
  "status": "success|error",
  "responseTime": 1250,
  "error": null
}

### 11.3 Plan de Rollback

# Procedimiento de Rollback v1.3 → v1.2

1. **Pre-condiciones:**
   - Backup completo de configuración actual
   - Usuarios notificados de mantenimiento

2. **Pasos:**
   - Deshabilitar acceso a aplicación
   - Restaurar versión anterior (index.html)
   - Restaurar configuración desde backup
   - Validar funcionalidad básica
   - Reabrir acceso

3. **Post-condiciones:**
   - Sistema funcionando en versión estable
   - Documentar causas del rollback
   - Planificar correcciones

### 12. FIRMAS Y APROBACIONES

Elaborado por:

John Jairo Vargas González
Ingeniero de Soluciones TI (Desarrollador - Vicepresidencia de Tecnología)
Fecha: 28/12/2025


Revisado por:

Alejandra Trujillo Cabrera
Lider Soluciones TI - RTE (Líder Técnico - Vicepresidencia de Tecnología)
Fecha: 28/12/2025


Aprobado por:

Diana Cristina Betancur Peña
Gerente Gestión de Servicios de TI & Modernización (Gerente - Vicepresidencia de Tecnología)
Fecha: 28/12/2025


### Distribución:

- Vicepresidencia de Tecnología

- Departamento de Operaciones

- Seguridad Informática

- Archivo Técnico

- Control de Documentos

Última Revisión: 2025
Próxima Revisión: 2026
Control de Cambios: [Sistema de Control Documental]