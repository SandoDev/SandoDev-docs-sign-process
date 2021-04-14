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