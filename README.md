# APIs Labs - OpenAPI Specifications

Este repositorio contiene las especificaciones OpenAPI (Swagger) para las APIs del laboratorio DevOps.

## 📁 Contenido

- **petstore-api.yaml**: API de gestión de mascotas (CRUD completo)
- **orders-api.yaml**: API de gestión de órdenes de compra

## 🎯 Propósito

Las especificaciones OpenAPI en este repositorio sirven para:

1. **Documentación**: Definir contratos de API claros y estándar
2. **Importación a APIM**: Azure API Management puede importar estas specs automáticamente
3. **Generación de código**: Herramientas como Swagger Codegen pueden generar clientes
4. **Testing**: Usar en herramientas como Postman o Newman
5. **Versionado**: Control de versiones de los contratos de API

## 🔗 Integración con Azure API Management

### URLs Raw de GitHub

Para importar estas APIs en Azure API Management, usa las URLs raw:

```
https://raw.githubusercontent.com/ImTronick2025/apis-labs-api/main/petstore-api.yaml
https://raw.githubusercontent.com/ImTronick2025/apis-labs-api/main/orders-api.yaml
```

### Importación Manual

1. Ve a Azure Portal → API Management
2. Selecciona "APIs" en el menú
3. Click en "+ Add API" → "OpenAPI"
4. Pega la URL raw de GitHub
5. Configura:
   - Display name: "Petstore API" o "Orders API"
   - API URL suffix: "petstore" o "orders"
6. Click en "Create"

### Importación Automática con Terraform

En el repositorio `apis-labs-infra`, agrega:

```hcl
resource "azurerm_api_management_api" "petstore" {
  name                = "petstore-api"
  resource_group_name = azurerm_resource_group.main.name
  api_management_name = azurerm_api_management.main.name
  revision            = "1"
  display_name        = "Petstore API"
  path                = "petstore"
  protocols           = ["https"]
  
  import {
    content_format = "openapi-link"
    content_value  = "https://raw.githubusercontent.com/ImTronick2025/apis-labs-api/main/petstore-api.yaml"
  }
}

resource "azurerm_api_management_api" "orders" {
  name                = "orders-api"
  resource_group_name = azurerm_resource_group.main.name
  api_management_name = azurerm_api_management.main.name
  revision            = "1"
  display_name        = "Orders API"
  path                = "orders"
  protocols           = ["https"]
  
  import {
    content_format = "openapi-link"
    content_value  = "https://raw.githubusercontent.com/ImTronick2025/apis-labs-api/main/orders-api.yaml"
  }
}
```

### Importación con Azure CLI

```bash
# Petstore API
az apim api import \
  --resource-group apis-labs-rg \
  --service-name <your-apim-name> \
  --path petstore \
  --api-id petstore-api \
  --display-name "Petstore API" \
  --specification-url https://raw.githubusercontent.com/ImTronick2025/apis-labs-api/main/petstore-api.yaml \
  --specification-format OpenApiJson

# Orders API
az apim api import \
  --resource-group apis-labs-rg \
  --service-name <your-apim-name> \
  --path orders \
  --api-id orders-api \
  --display-name "Orders API" \
  --specification-url https://raw.githubusercontent.com/ImTronick2025/apis-labs-api/main/orders-api.yaml \
  --specification-format OpenApiJson
```

## 📝 Modificar las Especificaciones

### Validación Local

Instala herramientas de validación:

```bash
# Con npm
npm install -g @apidevtools/swagger-cli

# Validar
swagger-cli validate petstore-api.yaml
swagger-cli validate orders-api.yaml
```

### Editor Online

Usa el [Swagger Editor](https://editor.swagger.io/) para editar y validar en tiempo real.

### Versionado

Para nuevas versiones de la API:

1. Actualiza el campo `version` en el archivo YAML
2. Considera crear un archivo separado (ej: `petstore-api-v2.yaml`)
3. Haz commit y push
4. Actualiza la importación en APIM

## 🧪 Testing

### Con Postman

1. Importa el archivo YAML en Postman
2. Configura las variables de entorno:
   - `apim-gateway`: URL de tu APIM
   - `subscription-key`: Tu clave de suscripción

### Con Newman (CLI)

```bash
newman run petstore-api.yaml \
  --env-var apim-gateway=your-apim.azure-api.net \
  --env-var subscription-key=your-key
```

## 🔐 Seguridad

Todas las APIs usan API Key authentication a través de:
- Header: `Ocp-Apim-Subscription-Key`
- Gestionada por Azure API Management

## 📚 Recursos

- [OpenAPI Specification](https://swagger.io/specification/)
- [Azure API Management - Import APIs](https://docs.microsoft.com/azure/api-management/import-api-from-oas)
- [Swagger Editor](https://editor.swagger.io/)
- [Swagger Codegen](https://github.com/swagger-api/swagger-codegen)

## 🔄 Flujo de Trabajo

1. Edita las especificaciones OpenAPI localmente
2. Valida con `swagger-cli validate`
3. Commit y push a GitHub
4. Las URLs raw se actualizan automáticamente
5. APIM puede re-importar para actualizar (manual o automatizado)
