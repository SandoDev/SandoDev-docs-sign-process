# Ejecutar pasos

El proceso de firma funciona ejecutando pasos por cada firmante y cada documento a firmar, los pasos que se ejecutarán corresponden a los componentes configurados en la regla en la llave [documents](../rule/#documents).

Por ejemplo: si hay un firmante principal y un codeudor, y hay dos documentos (pagare, contrato), se deberá ejecutar este proceso 4 veces, dos para el firmante principal, es decir una por cada documento, y otras dos para el codeudor.


Ejecute los pasos con el siguiente endpoint:

    POST: {HOST}/api/v1/next_steps
    HOST: example.com
    Content-Type: application/json

## Cuerpo de la solicitud
```json
{
    "process_id": "c6f0b0d08c9fb44e6c342e640194379618fdc6da",
    "document_template": "c212397f-94ce-4512-9654-015c70d44146",
    "contact_uuid": "bf3860d1-bf53-43ac-ad92-169d3d9326a9"
}
```

### Campos de la solicitud

#### process_id
Corresponde al id que retorna la [creación](../post/#respuesta-correcta) del proceso de firma cuando las respuesta es 200

    - Obligatorio
    - Tipo: str

**Ejemplo de uso:**
```json
    "process_id": "c6f0b0d08c9fb44e6c342e721194379618fdc6da"
```

#### document_template
uuid identificador del template asociado a ese documento.

Corresponde al `name` que retorna la [creación](../post/#respuesta-correcta) del proceso de firma cuando las respuesta es 200

    - Obligatorio
    - Tipo: str(UUID)

**Ejemplo de uso:**
```json
    "document_template": "c212397f-94ce-4512-9654-015c70d44146"
```

#### contact_uuid
Identificador del contacto

    - Obligatorio
    - Tipo: str(UUID)

**Ejemplo de uso:**
```json
    "contact_uuid": "bf3860d1-bf53-43ac-ad92-169d3d9326a9"
```

## Ejemplo de respuesta
La respuesta correcta debe tener **código de estado 200** cada vez que sea ejecutado

Debe ejecutar este endpoint tantas veces por cada documento hasta que la respuesta sea
```json
{
    "success": true,
    "message": "Process complete",
    "data": {
        "step": "sign"
    }
}
```
Dependiendo de como haya configurado la regla y los passos para los documentos, así mismo tendrá que ejecutar este endpoint una cantidad de veces determiniada.