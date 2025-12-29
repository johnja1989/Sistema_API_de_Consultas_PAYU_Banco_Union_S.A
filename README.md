## Sistema de Conexión API PAYU - Banco Unión S.A.

## 📋 Descripción

Sistema web para la conexión y consulta al API de PAYU desarrollado para el Banco Unión S.A., específicamente para la Vicepresidencia de Tecnología. Esta herramienta permite realizar consultas a los servicios de PAYU tanto en ambiente de pruebas (QA/Sandbox) como en producción.

## ✨ Características

- Configuración separada por ambiente: Credenciales independientes para QA y Producción

- Múltiples tipos de consulta:

  - Consulta por Order ID

  - Consulta por Transaction ID

  - Consulta por Reference ID

  - Prueba de conexión (PING)

- Gestión de credenciales: Almacenamiento seguro en localStorage

- Configuración de proxy: Soporte para conexiones con proxy

- Respuestas formateadas: Visualización de JSON con sintaxis resaltada

- Copiado al portapapeles: Funcionalidad para copiar respuestas

- Interfaz responsiva: Compatible con dispositivos móviles y desktop

## 🏗️ Estructura del Proyecto

payu-api-connection/
├── index.html          # Archivo principal (HTML/CSS/JavaScript)
├── README.md           # Documentación
└── LOGO.png           # Logo del Banco Unión S.A.

## 🔧 Configuración Inicial

Requisitos

- Navegador web moderno (Chrome 80+, Firefox 75+, Edge 80+)

- Acceso a internet

- Credenciales de PAYU (separadas por ambiente)

Pasos de Instalación

1. Clonar/Descargar el proyecto

git clone [URL-del-repositorio]

2. Abrir el archivo index.html

   - Doble click en el archivo, o

   - Servir desde un servidor web local

3. Configurar credenciales

   - Abrir la aplicación en el navegador

   - Ir a la pestaña "Configuración"

   - Ingresar las credenciales correspondientes

## 📝 Configuración de Credenciales

## Ambiente QA/Sandbox

1. API Login (QA): Credencial obtenida de https://sandbox.dashboard.payulatam.com/

2. API Key (QA): Llave de API del ambiente de pruebas

3. Merchant ID (QA): ID del comercio en sandbox

4. Account ID (QA): ID de cuenta en sandbox (opcional)

## Ambiente de Producción

1. API Login (Producción): Credencial obtenida de https://dashboard.payulatam.com/

2. API Key (Producción): Llave de API del ambiente de producción

3. Merchant ID (Producción): ID del comercio en producción

4. Account ID (Producción): ID de cuenta en producción (opcional)

## ⚠️ IMPORTANTE: Las credenciales de QA y Producción son DIFERENTES. PayU genera credenciales únicas para cada ambiente.

## 🚀 Uso del Sistema

## 1. Prueba de Conexión

1. Ir a la pestaña "Prueba de Conexión"

2. Configurar las credenciales correspondientes

3. Hacer click en "Probar Conexión Actual"

4. Verificar la respuesta en el panel derecho

## 2. Ejecutar Consultas

1. Seleccionar el tipo de consulta:

   - Por Order ID: Consulta por identificador de orden

   - Por Transaction ID: Consulta por identificador de transacción

   - Por Reference ID: Consulta por código de referencia

   - Ping: Prueba de conectividad básica

2. Ingresar el parámetro requerido (excepto para Ping)

3. Hacer click en "Ejecutar Consulta"

4. Revisar la respuesta en el panel de resultados

## 3. Configuración Avanzada

- Proxy: Configurar proxy para evitar problemas de CORS

- Timeout: Ajustar tiempo de espera de las peticiones

- Logging: Habilitar registro de peticiones para debug

## 🔍 Solución de Problemas

## Error: "Invalid credentials"

## Causa: Credenciales incorrectas para el ambiente seleccionado

## Solución:

1. Verificar que estás usando las credenciales correctas para cada ambiente

2. Obtener nuevas credenciales desde el dashboard correspondiente:

   - QA: https://sandbox.dashboard.payulatam.com/

   - Producción: https://dashboard.payulatam.com/

## Error: "Formato de petición inválido"

## Causa: Estructura JSON incorrecta en la petición

## Solución:

1. Usar el "Probador de Estructuras" en la pestaña de prueba

2. Probar diferentes estructuras hasta encontrar la correcta

## Error: Problemas de CORS

## Causa: Restricciones de seguridad del navegador

## Solución:

1. Habilitar configuración de proxy

2. Usar un proxy público o interno del banco

## 🗂️ Almacenamiento de Datos

- El sistema utiliza localStorage del navegador para guardar:

- Credenciales de API (encriptadas en el navegador)

- Configuración de proxy

- Preferencias de usuario

- Historial de errores

## Nota: Los datos se guardan solo en el navegador local y no se transmiten a servidores externos.

## 📱 Compatibilidad

## Navegadores Soportados

- ✅ Google Chrome 80+

- ✅ Mozilla Firefox 75+

- ✅ Microsoft Edge 80+

- ✅ Safari 13+

- ✅ Opera 67+

## Responsive Design

- Desktop (≥ 1200px)

- Tablet (≥ 768px)

- Mobile (≥ 320px)

## 🔒 Seguridad

- Las API Keys se almacenan en localStorage del navegador

- No se transmiten datos sensibles a servidores externos

- Se recomienda limpiar el localStorage después de su uso

- No usar en computadoras públicas o compartidas

## 📊 Estructura de la Petición API

## Ejemplo para PING

{
  "test": false,
  "language": "en",
  "command": "PING",
  "merchant": {
    "apiLogin": "API_LOGIN",
    "apiKey": "API_KEY"
  }
}

## Ejemplo para ORDER_DETAIL

{
  "test": false,
  "language": "en",
  "command": "ORDER_DETAIL",
  "merchant": {
    "apiLogin": "API_LOGIN",
    "apiKey": "API_KEY",
    "merchantId": "MERCHANT_ID"
  },
  "details": {
    "orderId": 123456789
  }
}

## 🔗 URLs del API

- QA/Sandbox: https://sandbox.api.payulatam.com/reports-api/4.0/service.cgi

- Producción: https://api.payulatam.com/reports-api/4.0/service.cgi

## 📞 Soporte

## Para problemas técnicos o preguntas:

1. Soporte Banco Unión: Contactar a la Vicepresidencia de Tecnología

2. Documentación PayU: https://developers.payulatam.com/latam/es/docs/

3. Dashboards PayU:

   - QA: https://sandbox.dashboard.payulatam.com/

   - Producción: https://dashboard.payulatam.com/

## 📄 Licencia

© 2025 Banco Unión S.A. - Todos los derechos reservados.

Este sistema es para uso interno del Banco Unión S.A. y no debe ser distribuido fuera de la organización sin autorización expresa.

## 🔄 Historial de Versiones

## v1.3 (Actual)

- Credenciales separadas por ambiente

- Mejores mensajes de error

- Solución para "Invalid credentials"

## v1.2

- Probador de estructuras múltiples

- Mejor manejo de formatos JSON

## v1.1

- Corrección de formato de petición

- Sistema de proxy mejorado

## v1.0

- Versión inicial

- Consultas básicas a API PAYU

- Configuración dual QA/Producción

## Desarrollado por: Vicepresidencia de Tecnología - Banco Unión S.A.

## Última actualización: 2025