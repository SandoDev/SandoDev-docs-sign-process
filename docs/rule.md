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
### process
#### generals
#### signers
#### documents
#### canceled
#### succes

## Parámetros de un componente

### name
### type
### params
### priority
### direct_action
### mandatory
### auto