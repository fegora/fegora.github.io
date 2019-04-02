---
layout: default
title: Solicitar Token
parent: Autenticación
nav_order: 3
---

# POST /token
Para solicitar un token, debe hacer una solicitud `POST` con su usuario y contraseña al endpoint **https://api.fegora.com/token**

## Request

### Headers

| Header | Valor | Descipción |
|-----|-----|-----|
|Accept|application/json||
|Content-Type|application/x-www-form-urlencoded||

### Body

| Header | Valor | Descipción |
|-----|-----|-----|
|`grant_type`|`password`|El método de autenticación|
|`username` |*{USUARIO}*|El usuario asignado. Por lo regular será el número de **NIT**|
|`password`|*{CONTRASEÑA}*|La constraseña asignada|
|`client_id`|`apiApp`|El tipo de client_id. Siempre va este valor|

#### Ejemplo de llamada

```bash
curl -X POST 
  https://api.fegora.com/token 
  -H 'Accept: application/json' 
  -H 'Content-Type: application/x-www-form-urlencoded' 
  -H 'Postman-Token: e5eb0e8e-c232-422d-8109-d3b1bf8121a0' 
  -H 'cache-control: no-cache' 
  -d 'grant_type=password&client_id=apiApp&username=25365421&password={contrasenia}'
```

## Response

### HTTP 200 OK
Si recibe este código `HTTP` de respuesta, se ha autenticado satisfactoriamente. El cuerpo de la respuesta (body) contiene la información siguiente en formato JSON: 

#### Descripción de respuesta

|Nombre|Descripción|Ejemplo
|-----|-----|-----|
|`access_token`|Contine el **token** que deberá utilizar en todas las llamdas al API, que indican que está correctamente autenticado|`7XLOCJyiMAs0ZxX_sUzeoDtcHmdxo3a5cTbkdz5...`|
|`token_type`|Tipo de token asignado. En genral será `bearer`|`bearer`|
|`expires_in`|Indica el tiempo de expiración del token en segundos. Por lo regular este valor será más grande que 86400 (24h).|`86400`|
|`refresh_token`|Contine el **token** que deberá utilizar para solicitar un nuevo token cuando el `access_token` haya expirado|`0dbf0f4d26af41d5b163d3d90f8a1376`|

#### Ejemplo de respuesta
```bash
{
    "access_token": "3iaQd3k-vdrSH5-wxy8sVzDEi4YEhLunN...",
    "token_type": "bearer",
    "expires_in": 1799,
    "refresh_token": "20811ac42ee3422da6ef0861c3409e8a",
    "as:client_id": "apiApp",
    "userName": "43430775",
    ".issued": "Wed, 06 Mar 2019 14:42:36 GMT",
    ".expires": "Wed, 06 Mar 2019 15:12:36 GMT"
}
```

### HTTP 400 Bad Request
Si recibe este código `HTTP` de respuesta, **NO** se ha autenticado satisfactoriamente. El cuerpo de la respuesta (body) contiene la información siguiente en formato JSON: 

#### Descripción de respuesta

|Nombre|Descripción|Ejemplo
|-----|-----|-----|
|`error`|Contine el nombre del error|`"invalid_clientId"`|
|`error_description`|Describe el error con más detalle|`"Client 'xyz' is not registered in the system."`|

#### Ejemplo de respuesta
```json
{
    "error": "invalid_clientId",
    "error_description": "Client 'apiAp' is not registered in the system."
}
```
