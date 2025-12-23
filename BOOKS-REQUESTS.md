# Books API - Example Requests (APIM)

Variables sugeridas:
- APIM gateway: https://<apim-name>.azure-api.net
- Subscription key: <subscription-key>

## Health check

```
curl https://<apim-name>.azure-api.net/api/health \
  -H "Ocp-Apim-Subscription-Key: <subscription-key>"
```

## List books

```
curl https://<apim-name>.azure-api.net/api/books \
  -H "Ocp-Apim-Subscription-Key: <subscription-key>"
```

## Create book

```
curl -X POST https://<apim-name>.azure-api.net/api/books \
  -H "Content-Type: application/json" \
  -H "Ocp-Apim-Subscription-Key: <subscription-key>" \
  -d '{
    "isbn": "978-1-234567-89-0",
    "title": "Clean Architecture",
    "author": { "id": "author-001", "name": "Robert C. Martin" },
    "categories": ["Technology"],
    "publicationYear": 2017,
    "language": "en",
    "pages": 432
  }'
```

## Get book by id

```
curl https://<apim-name>.azure-api.net/api/books/<book-id> \
  -H "Ocp-Apim-Subscription-Key: <subscription-key>"
```

## Update book

```
curl -X PUT https://<apim-name>.azure-api.net/api/books/<book-id> \
  -H "Content-Type: application/json" \
  -H "Ocp-Apim-Subscription-Key: <subscription-key>" \
  -d '{
    "title": "Clean Architecture (Updated)",
    "pages": 436
  }'
```

## Delete book

```
curl -X DELETE https://<apim-name>.azure-api.net/api/books/<book-id> \
  -H "Ocp-Apim-Subscription-Key: <subscription-key>"
```
