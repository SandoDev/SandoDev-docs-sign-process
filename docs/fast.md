- [Inicio rápido](#inicio-rápido)
  - [Crear el proceso](#crear-el-proceso)
  - [Firmar un documento](#firmar-un-documento)
    - [Paso 1](#paso-1)
    - [Paso 2](#paso-2)
    - [Paso 3](#paso-3)
    - [Paso 4](#paso-4)
  - [Firmar el siguiente documento](#firmar-el-siguiente-documento)
  - [Finalmente](#finalmente)
# Inicio rápido

Guía de inicio rápido de uso del servicio de Sign-Process

## Crear el proceso
Cree un proceso de firma con el siguiente curl, reemplazando el host por el adecuado.

1. Asegure que exista una aplicación ya lista para firmar. `application_uid`
2. Procure que exista una regla para el producto. `product_uuid`
3. Verifique que el contacto tiene todos sus formularios llenos. `contact_uuid`
```curl
curl --location --request POST '{{host}}/api/v1/documents' \
--header 'Content-Type: application/json' \
--data-raw '{
    "application_uid": "685896118",
    "product_uuid": "5205c9b6-14c2-11eb-94ae-0fcd21ac0359",
    "contact_uuid": "12fa5b7f-4a1e-4884-b040-7b5e181c6b05"
}'
```

**Response [200]**
```json
{
    "process_id": "bc6c3435a91f88d74b762a5ddb1b7eb5892d08c8",
    "created_at": "2021-07-22T15:40:01.219000",
    "expires": "2021-07-22 15:40:01.219000",
    "documents": [
        {
            "name": "f8ba746a-b692-4f6e-933c-e58837ebc75d",
            "description": "Contrato para crédito rotativo aliatu",
            "type": "contrato",
            "signed_at": null,
            "step": "",
            "history_steps": [],
            "document": null
        },
        {
            "name": "a7bfeddb-af2b-4b11-af4a-01dca30d2895",
            "description": "Pagare + carta de instrucción + FNG",
            "type": "pagare",
            "signed_at": null,
            "step": "",
            "history_steps": [],
            "document": null
        }
    ]
}
```

## Firmar un documento

### Paso 1
Ejecute una vez este curl, para firmar el primer documento.

1. Del response anterior tomar el `process_id`
2. Del response anterior tomar el `name` de la llave `documents` y ponerlo en el `document_template`
2. Del response anterior tomar el `contact_uuid`

```curl
curl --location --request POST '{{host}}/api/v1/next_steps' \
--header 'Content-Type: application/json' \
--data-raw '{
    "process_id": "bc6c3435a91f88d74b762a5ddb1b7eb5892d08c8",
    "document_template": "f8ba746a-b692-4f6e-933c-e58837ebc75d",
    "contact_uuid": "12fa5b7f-4a1e-4884-b040-7b5e181c6b05"
}'
```

**Response [200]**
```json
{
    "success": true,
    "document": "https://zinobe-test.s3.amazonaws.com/templates/08d186f2-7070-4f9f-82ac-91571a3abf15.pdf?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIARDM62DIACYWN3YCV%2F20210722%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210722T204453Z&X-Amz-Expires=3600&X-Amz-SignedHeaders=host&X-Amz-Signature=f2df8a4ef8ec6f6e668ccc1c641fce04f1fb8e4918b855ca86beabffc9eeca7c",
    "message": "Document preview",
    "data": {
        "step": "view_document"
    }
}
```

### Paso 2

Vuelva a ejecutar el curl anterior

```curl
curl --location --request POST '{{host}}/api/v1/next_steps' \
--header 'Content-Type: application/json' \
--data-raw '{
    "process_id": "bc6c3435a91f88d74b762a5ddb1b7eb5892d08c8",
    "document_template": "f8ba746a-b692-4f6e-933c-e58837ebc75d",
    "contact_uuid": "12fa5b7f-4a1e-4884-b040-7b5e181c6b05"
}'
```

**Response [200]**
```json
{
    "success": true,
    "message": "Rquiere la validacion de los tokens",
    "data": {
        "endpoint": "token/validate",
        "method": "POST",
        "body": {
            "channels": [
                {
                    "channel": "sms",
                    "value_token": ""
                },
                {
                    "channel": "email",
                    "value_token": ""
                }
            ]
        },
        "step": "validation"
    }
}
```

### Paso 3

Valide los tokens que le llegaron al email y al sms del contacto, con el siguiente curl.

```curl
curl --location --request POST '{{host}}/api/v1/token/validate' \
--header 'Content-Type: application/json' \
--data-raw '{
    "process_id": "bc6c3435a91f88d74b762a5ddb1b7eb5892d08c8",
    "document_template": "f8ba746a-b692-4f6e-933c-e58837ebc75d",
    "contact_uuid": "12fa5b7f-4a1e-4884-b040-7b5e181c6b05",
    "channels": [
        {
            "channel": "sms",
            "value_token": "112448"
        },
        {
            "channel": "email",
            "value_token": "593092"
        }
    ]
}'
```

**Response [200]**
```json
{
    "success": true,
    "uuid": "6e6dde78-8228-42cc-90f0-a6fcf5010b99",
    "message": "Tokens validated",
    "data": {
        "step": "validation"
    }
}
```

### Paso 4

Volver a ejecutar el endpoint de next_steps

```curl
curl --location --request POST '{{host}}/api/v1/next_steps' \
--header 'Content-Type: application/json' \
--data-raw '{
    "process_id": "bc6c3435a91f88d74b762a5ddb1b7eb5892d08c8",
    "document_template": "f8ba746a-b692-4f6e-933c-e58837ebc75d",
    "contact_uuid": "12fa5b7f-4a1e-4884-b040-7b5e181c6b05"
}'
```

**Response [200]**
```json
{
    "success": true,
    "document": "https://zinobe-test.s3.amazonaws.com/files/pdf/2021/07/22/20/45959168-f69d-4ffa-b7f1-f51720b37d06.pdf?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIARDM62DIACYWN3YCV%2F20210722%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210722T205201Z&X-Amz-Expires=3600&X-Amz-SignedHeaders=host&X-Amz-Signature=b28ffec20eb21758769557123be38b11257ad98354c84eb2cf67954e8b3978fb",
    "uuid": "45959168-f69d-4ffa-b7f1-f51720b37d06",
    "message": "The service has been effectively consumed",
    "data": {
        "step": "sign"
    }
}
```

## Firmar el siguiente documento

Repetir los 3 primeros pasos que se hicieron para el documento anterior.

Asegurese de cambiar el `document_template` con el uuid del documento que no ha firmado.

## Finalmente

Ejecute el primer curl y verifique que en el response las llaves `signed_at` de cada documento tienen fecha y hora

```curl
curl --location --request POST '{{host}}/api/v1/documents' \
--header 'Content-Type: application/json' \
--data-raw '{
    "application_uid": "685896118",
    "product_uuid": "5205c9b6-14c2-11eb-94ae-0fcd21ac0359",
    "contact_uuid": "12fa5b7f-4a1e-4884-b040-7b5e181c6b05"
}'
```

**Response [200]**
```json
{
    "process_id": "bc6c3435a91f88d74b762a5ddb1b7eb5892d08c8",
    "created_at": "2021-07-22T15:40:01.219000",
    "expires": "2021-07-22 15:40:01.219000",
    "documents": [
        {
            "name": "f8ba746a-b692-4f6e-933c-e58837ebc75d",
            "description": "Contrato para crédito rotativo aliatu",
            "type": "contrato",
            "signed_at": "2021-07-22T15:52:05.316000",
            "step": "sign",
            "history_steps": [
                "view_document",
                "validation_generate",
                "validation",
                "sign"
            ],
            "document": "https://zinobe-test.s3.amazonaws.com/files/pdf/2021/07/22/20/45959168-f69d-4ffa-b7f1-f51720b37d06.pdf?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIARDM62DIACYWN3YCV%2F20210722%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210722T205552Z&X-Amz-Expires=3600&X-Amz-SignedHeaders=host&X-Amz-Signature=e3f91a1f3d59aa087c5e433325e0789e352aa649da33f57c3a1b89ddd4395aad"
        },
        {
            "name": "a7bfeddb-af2b-4b11-af4a-01dca30d2895",
            "description": "Pagare + carta de instrucción + FNG",
            "type": "pagare",
            "signed_at": "2021-07-22T15:54:16.245000",
            "step": "validation",
            "history_steps": [
                "view_document",
                "validation_generate",
                "validation"
            ],
            "document": null
        }
    ]
}
```

**Ahora sonríe, has terminado 😁**