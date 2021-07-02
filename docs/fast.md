- [Inicio rápido](#inicio-rápido)
  - [Crear el proceso](#crear-el-proceso)
  - [Firmar un documento](#firmar-un-documento)
    - [Paso 1](#paso-1)
    - [Paso 2](#paso-2)
    - [Paso 3](#paso-3)
  - [Firmar el siguiente documento](#firmar-el-siguiente-documento)
  - [Finalmente](#finalmente)
# Inicio rápido

ESSSSSSTOOOO ESSSSTAAA EN CONSSSTRUCCIÓN........

AJUSTAR CURL Y RESPONSE...

## Crear el proceso
Cree un proceso de firma con el siguiente curl, reemplazando el host por el adecuado.

1. Asegure que exista una aplicación ya lista para firmar. `application_uid`
2. Procure que exista una regla para el producto. `product_uuid`
3. Verifique que el contacto tiene todos sus formularios llenos. `contact_uuid`
```curl
curl --location -g --request POST '{{host}}/api/v1/documents' \
--header 'Content-Type: application/json' \
--data-raw '{
    "application_uid": "685896118",
    "product_uuid": "5205c9b6-14c2-11eb-94ae-0fcd21ac0359",
    "contact_uuid": "783b1fd1-e0a4-4dd5-878a-54385b5f0b61"
}'
```

**Response**
```json
{
    "process_id": "c6f0b0d08c9fb44e6c342e721194379618fdc6da",
    "created_at": "2021-04-23T16:30:13.772000",
    "expires": "2021-05-22 16:30:13.772000",
    "documents": [
        {
            "name": "c212397f-94ce-4512-9654-015c70d44146",
            "description": "Contrato para crédito",
            "type": "contrato",
            "signed_at": null,
            "step": "",
            "history_steps": [],
            "document": null
        },
        {
            "name": "c31e8c26-7764-4cc3-9b48-4c2310c7e884",
            "description": "Pagaŕe para crédito",
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
    "process_id": "c6f0b0d08c9fb44e6c342e721194379618fdc6da",
    "document_template": "c212397f-94ce-4512-9654-015c70d44146",
    "contact_uuid": "783b1fd1-e0a4-4dd5-878a-54385b5f0b61"
}'
```

**Response**

### Paso 2

Vuelva a ejecutar el curl anterior

**Response**

### Paso 3

Valide los tokens que le llegaron al email y al sms del contacto, con el siguiente curl.

```curl
curl --location --request POST '{{host}}/api/v1/token/validate' \
--header 'Content-Type: application/json' \
--data-raw '{
    "process_id": "c6f0b0d08c9fb44e6c342e721194379618fdc6da",
    "document_template": "c212397f-94ce-4512-9654-015c70d44146",
    "contact_uuid": "783b1fd1-e0a4-4dd5-878a-54385b5f0b61",
    "channels": [
        {
            "channel": "sms",
            "value_token": "258700"
        },
        {
            "channel": "email",
            "value_token": "411314"
        }
    ]
}'
```

**Response**

## Firmar el siguiente documento

Repetir todos los pasos que se hicieron para el documento anterior.

Asegurese de cambiar el `document_template` con el uuid del documento que no ha firmado.

## Finalmente

Ejecute el primer curl y verifique que en el response las llaves `signed_at` de cada documento tienen fecha y hora

```curl
curl --location -g --request POST '{{host}}/api/v1/documents' \
--header 'Content-Type: application/json' \
--data-raw '{
    "application_uid": "685896118",
    "product_uuid": "5205c9b6-14c2-11eb-94ae-0fcd21ac0359",
    "contact_uuid": "783b1fd1-e0a4-4dd5-878a-54385b5f0b61"
}'
```

**Response**
```json
{
    "process_id": "c6f0b0d08c9fb44e6c342e721194379618fdc6da",
    "created_at": "2021-04-23T16:30:13.772000",
    "expires": "2021-05-22 16:30:13.772000",
    "documents": [
        {
            "name": "c212397f-94ce-4512-9654-015c70d44146",
            "description": "Contrato para crédito",
            "type": "contrato",
            "signed_at": null,
            "step": "",
            "history_steps": [],
            "document": null
        },
        {
            "name": "c31e8c26-7764-4cc3-9b48-4c2310c7e884",
            "description": "Pagaŕe para crédito",
            "type": "pagare",
            "signed_at": null,
            "step": "",
            "history_steps": [],
            "document": null
        }
    ]
}
```

**Ahora sonríe, has terminado 😁**