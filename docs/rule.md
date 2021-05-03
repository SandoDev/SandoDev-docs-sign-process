# Regla del proceso de firma

La **regla** es un concepto ideado por el equipo de tecnología de **Zinobe** que tiene como finalidad controlar el flujo de funcionamiento de un servicio.

El **servicio de firma** funciona como un orquestador de microservicios para firmar documentos digitalmente. En la regla para este servicio se podrá configurar los **componentes** (microservicios) que se utilizarán para un flujo de firma especifíco.

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

#### signers
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

#### canceled
#### succes
Serán los componentes que se ejecuten cuando se ha completado todo el proceso de firma exitosamente

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

## Parámetros de un componente

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
### params
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
### mandatory
### auto
### public
Si este valor es true se mostrará a nivel general en el historial de pasos ejecutados

    - Obligatorio
    - Tipo: boolean

**Ejemplo de uso:**
```json
    "public": true
```