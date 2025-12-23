# APIs Labs - OpenAPI Specifications

This repository contains OpenAPI specs for the APIs Labs project.

## Contents

- petstore-api.yaml: Petstore API (CRUD)
- orders-api.yaml: Orders API
- books-api.yaml: Books catalog API (CRUD)

## Raw GitHub URLs

- https://raw.githubusercontent.com/ImTronick2025/apis-labs-api/main/petstore-api.yaml
- https://raw.githubusercontent.com/ImTronick2025/apis-labs-api/main/orders-api.yaml
- https://raw.githubusercontent.com/ImTronick2025/apis-labs-api/main/books-api.yaml

## Import into APIM (Books API)

### Azure CLI

```bash
az apim api import \
  --resource-group <apim-rg> \
  --service-name <apim-name> \
  --path api \
  --api-id books-api \
  --display-name "Books API" \
  --specification-path books-api.yaml \
  --specification-format OpenApiJson \
  --service-url https://<function-app>.azurewebsites.net/api
```

### Terraform (example)

```hcl
resource "azurerm_api_management_api" "books" {
  name                = "books-api"
  resource_group_name = azurerm_resource_group.main.name
  api_management_name = azurerm_api_management.main.name
  revision            = "1"
  display_name        = "Books API"
  path                = "api"
  protocols           = ["https"]

  import {
    content_format = "openapi-link"
    content_value  = "https://raw.githubusercontent.com/ImTronick2025/apis-labs-api/main/books-api.yaml"
  }
}
```

## Validation

```bash
swagger-cli validate petstore-api.yaml
swagger-cli validate orders-api.yaml
swagger-cli validate books-api.yaml
```

## GitHub Actions: Deploy to APIM

This repo includes a workflow that imports books-api.yaml into APIM and configures the backend to Azure Functions.

Required secrets:
- AZURE_CREDENTIALS
- AZURE_APIM_NAME
- AZURE_APIM_RESOURCE_GROUP
- AZURE_FUNCTIONAPP_NAME
- AZURE_FUNCTIONAPP_RESOURCE_GROUP

## Examples and Postman

- BOOKS-REQUESTS.md: request examples for APIM.
- postman/Books-APIM.postman_collection.json: Postman collection.
