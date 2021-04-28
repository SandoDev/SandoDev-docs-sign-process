# Crear proceso

Para inicializar un proceso de firma consuma el endpoint:

    POST: {HOST}/api/v1/documents
    HOST: example.com
    Content-Type: application/json

## Cuerpo de la solicitud
```json
{
    "product_uuid": "a42dbee0-7743-4cb4-b782-6ca836f7889c",
    "application_uid": "2485654285",
    "contact_uuid": "bf3860d1-bf53-43ac-ad92-169d3d9326a9",
    "overwrite": {},
    "renew": []
}
```

### Campos de la solicitud

#### product_uuid
Identificador del producto, debe existir una [regla](rule.md) existente para este producto

    - Obligatorio
    - Tipo: str(UUID)

**Ejemplo de uso:**
```json
    "product_uuid": "a42dbee0-7743-4cb4-b782-6ca836f7889c"
```

#### application_uid
Identificador de la solicitud de crédito

    - Obligatorio
    - Tipo: str(num)

**Ejemplo de uso:**
```json
    "application_uid": "2485654285"
```

#### contact_uuid
Identificador del contacto

    - Obligatorio
    - Tipo: str(UUID)

**Ejemplo de uso:**
```json
    "contact_uuid": "bf3860d1-bf53-43ac-ad92-169d3d9326a9"
```

#### overwrite
Sobreescribe variables para algunos elementos

    - Obligatorio
    - Tipo: objeto

**Ejemplo de uso:**
```json
    "overwrite": {
        "legal_representative": {
            "name": "nombre-replace",
            "doc_nro": "113218690"
        },
        "product": {
            "amount": 500000
        },
        "customer": {
            "first_name": "Pepe"
        }
    }
```

<div class="note">
    <strong>Nota: </strong>
    Estas 3 llaves dentro de overwrite son los elementos actuales en funcionamiento y se deben escribir exactamente igual para que el programa las tome.
</div>

#### renew
Renueva el proceso de firma para algún documento

    - Opcional
    - Tipo: lista de str

**Ejemplo de uso:**
```json
    "renew": ["contrato","pagare"]
```

#### cancel_old_processes
Cancela todos los procesos de firma antiguos para ese contacto con ese producto, dejando activo únicamente el nuevo proceso que se esta creando

    - Opcional
    - Tipo: bool

**Ejemplo de uso:**
```json
    "cancel_old_processes": true
```

## Respuesta correcta

    status: ok 
    code: 200

### Ejemplo de respuesta

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
        }
    ]
}
```