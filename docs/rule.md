- [Regla del proceso de firma](#regla-del-proceso-de-firma)
  - [Elementos de la regla](#elementos-de-la-regla)
    - [uuid](#uuid)
    - [slug](#slug)
    - [product](#product)
    - [expired](#expired)
    - [process](#process)
      - [generals](#generals)
      - [documents](#documents)
      - [success](#success)
      - [canceled](#canceled)
      - [signers](#signers)
  - [Parámetros de un componente](#parámetros-de-un-componente)
    - [name](#name)
    - [type](#type)
    - [params](#params)
    - [priority](#priority)
    - [direct_action](#direct_action)
    - [mandatory](#mandatory)
    - [auto](#auto)
    - [unique](#unique)
    - [return](#return)
    - [public](#public)
# Regla del proceso de firma

La **regla** es un concepto ideado por el equipo de tecnología de **Zinobe** que tiene como finalidad controlar el flujo de funcionamiento de un servicio.

El **servicio de firma** funciona como un orquestador de microservicios para firmar documentos digitalmente. En la regla para este servicio se podrá configurar los **componentes** (microservicios o funcionalidades) que se utilizarán para un flujo de firma especifíco.

## Elementos de la regla

### uuid
Identificador del producto, debe existir en el api de productos.

    - Obligatorio
    - Tipo: str(UUID)

**Ejemplo de uso:**
```json
    "uuid": "a42dbee0-7743-4cb4-b782-6ca836f7889c"
```

### slug
Slug del producto

    - Obligatorio
    - Tipo: str

**Ejemplo de uso:**
```json
    "slug": "credito-ordinario"
```

### product
Nombre del producto

    - Obligatorio
    - Tipo: str

**Ejemplo de uso:**
```json
    "product": "Credito Ordinario"
```

### expired
Indica los días de expiración del proceso de firma una vez iniciado

    - Obligatorio
    - Tipo: int

**Ejemplo de uso:**
```json
    "expired": 180
```
### process
Contiene el flujo completo para el proceso de firma, esta compuesto por 5 llaves: generals, documents, success, canceled, signers
#### generals
Serán los primeros componentes que se ejecuten cuando se crea el proceso de firma

    - Obligatorio
    - Tipo: list(componentes)

**Ejemplo de uso:**
```json
        "generals": [
            {
                "name": "create_process",
                "type": "process",
                "params": null,
                "priority": 6,
                "direct_action": {
                    "active": false
                },
                "mandatory": true,
                "auto": true,
                "init_process": true
            }
        ],
```

#### documents
Serán los documentos a firmar, cada documento puede ser configurado con diferentes componentes en la llave `steps`, y se ejecutarán utilizando el endpoint de [`next_steps`](next.md) 

    - Obligatorio
    - Tipo: list(documents)

```json
            "documents": [
                {
                    "name": "e92fd52e-5481-4b0c-994b-efa51bf0fc28",
                    "description": "Contrato para crédito",
                    "type": "contrato",
                    "steps": [
                        {
                            "name": "template",
                            "params": null,
                            "priority": 1,
                            "direct_action": {
                                "active": false
                            },
                            "auto": true,
                            "unique": true,
                            "return": false,
                            "public": false
                        },
                        {
                            "name": "view_document",
                            "params": {
                                "location_document": "template",
                                "generate": "s3"
                            },
                            "priority": 2,
                            "direct_action": {
                                "active": false
                            },
                            "mandatory": false,
                            "auto": false,
                            "unique": false,
                            "return": true,
                            "public": true
                        },
                        {
                            "name": "myfield",
                            "params": null,
                            "priority": 3,
                            "direct_action": {
                                "active": false
                            },
                            "mandatory": false,
                            "auto": false,
                            "unique": false,
                            "return": true,
                            "public": true
                        }
                    ]
                }
            ],
```

#### success
Serán los componentes que se ejecuten cuando un firmante ha completado de firmar todos los documentos que tenía asignados

    - Obligatorio
    - Tipo: list(componentes)

**Ejemplo de uso:**
```json
        "success": [
            {
                "name": "notify_sns",
                "params": null,
                "priority": 1,
                "direct_action": {
                    "active": false
                },
                "auto": true,
                "unique": true,
                "return": false,
                "public": false
            }
        ]
```

#### canceled
Son los componentes que se ejecutan cuando se cancela el proceso de firma

    - Obligatorio
    - Tipo: list(componentes)

**Ejemplo de uso:**
```json
    "canceled": [
        {
            "name": "decval_cancel",
            "depend": false,
            "direct_action": {
                "active": false
            },
            "params": null,
            "priority": 1,
            "unique": false,
            "return": false,
            "public": false
        },
        {
            "name": "notify_sns",
            "depend": false,
            "direct_action": {
                "active": false
            },
            "params": {
                "cancelled": true
            },
            "priority": 2,
            "unique": false,
            "return": false,
            "public": false
        }
    ]
```

#### signers
Son los componentes que se ejecutan para cada firmante

    - Obligatorio
    - Tipo: list(componentes)

**Ejemplo de uso:**
```json
    "signers": [
        {
            "name": "validation_generate",
            "params": {
                "channels": [
                    {
                        "channel": "sms",
                        "value": "",
                        "template": "Aliatu le informa que su token de validacion es "
                    },
                    {
                        "channel": "email",
                        "value": "",
                        "template": "token-de-verificacion"
                    }
                ]
            },
            "priority": 4,
            "direct_action": {
                "active": false
            },
            "mandatory": false,
            "auto": true,
            "unique": false,
            "return": false,
            "public": true
        }
    ]
```

## Parámetros de un componente
Un componente es un objeto con diferentes atributos que puede ser reflejado en la ejecución de un microservicio o funcionalidad.

El siguiene componente indica que será ejecutado el servicio de **contacts** el cuál trae la información del los contactos de Zinobe.
```json
    {
        "name": "contacts",
        "params": null,
        "priority": 3,
        "direct_action": {
            "active": false
        },
        "mandatory": true,
        "auto": true
    }
```
Se listan a continuación los atributos que puede tener un componente.

### name
Nombre del componente

    - Obligatorio
    - Tipo: str
    - Anotación: en minúscula, separado por guión bajo, sin carácteres especiales

**Ejemplo de uso:**
```json
    "name": "product"
```

### type
----?

    - Obligatorio
    - Tipo: str

**Ejemplo de uso:**
```json
    "type": "process"
```

### params
Parámetros adicionales para la ejecución del componente

    - Obligatorio
    - Tipo: object
    - Tipo: Si el componente necesita un parámetro especial puede ser enviado en este apartado

**Ejemplo de uso:**
```json
    "params": {
        "location_document": "template",
        "generate": "s3"
    }
```

### priority
Numero que indica el orden de ejecución del componente

    - Obligatorio
    - Tipo: num
    - Anotación: cada componente debe tener un número diferente

**Ejemplo de uso:**
```json
    "priority": 1
```
Será el primer componente en ejecutar
```json
    "priority": 2
```
Será el segundo componente en ejecutar

### direct_action
Indica si un componente requiere alguna acción directa por parte del cliente

    - Obligatorio
    - Tipo: object

**Ejemplo de uso:**
```json
    "direct_action": {
        "active": true,
        "error": "Rquiere la validacion de los tokens",
        "meta": {
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
            }
        }
    }
```

### mandatory
Si este valor es true el response del componente estará disponible (mediante la llave params de la clase principal) para la ejecución de componentes posteriores

    - Obligatorio
    - Tipo: boolean

**Ejemplo de uso:**
```json
    "mandatory": false
```

### auto
Si este valor es true el componente será ejecutado automáticamente despues de la ejecución del anterior

    - Obligatorio
    - Tipo: boolean

**Ejemplo de uso:**
```json
    "auto": false
```

### unique
Si este valor es true el componente se ejecutará una única vez en todo el proceso de firma para todos los firmantes

    - Obligatorio
    - Tipo: boolean

**Ejemplo de uso:**
```json
    "unique": false
```

### return
Si este valor es true el componente deberá retornar una respuesta al cliente inmediatamente termine su ejecución

    - Obligatorio
    - Tipo: boolean
    - Anotación: Valide que después del actual componente no haya ninguno que requiera ejecución automática

**Ejemplo de uso:**
```json
    "return": true
```

### public
Si este valor es true se mostrará a nivel general en el historial de pasos ejecutados

    - Obligatorio
    - Tipo: boolean

**Ejemplo de uso:**
```json
    "public": true
```