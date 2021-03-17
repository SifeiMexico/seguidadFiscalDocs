# ![Sifei](https://www.sifei.com.mx/web/image/res.company/1/logo?unique=38c7250)



# Web Service Seguridad Fiscal Sifei.
Este repositorio incluye ejemplos de solicitudes y respuesta de los endpoint(metodos) del WS de Seguridad Fiscal.

 
[Sifei](https://www.sifei.com.mx/) 

# Listado de WS 

Actualmente se cuentan con los siguientes metodos
-  Método Lista69B
-  Método Lista69
-  Método LCO
-  Método LRFC

# Descripción Técnica
El presente WS está basado en una arquitectura tipo REST,  tanto la respuesta como la petición deben estar en formato JSON. Cada  método  tiene  su  propio  path  relativo a  la  URL  principal  de  consumo  y  en  general  comparten  una estructura  de  respuesta  basado en  JSON  genérica,  donde  únicamente  varía  el  atributo  data  según  el endpointconsumido.


# Requerimientos Mínimos

-  	Token valido para el consumo de API


# Estándar

El cuerpo de  respuesta genérico para todos los endpoint del  WS es un JSON con los siguientes atributos:

| Código       | Tipo       |  Definición |
| ----------- | ----------- |------------- |
|status   | string | Indica el estado de la solicitud, sus valores posibles son: success, fail, error. <br /> Donde:<br /> •	success indica que el consumo fue exitoso <br /> •	fail indica algún error en el consumo(token invalido, falta de algún atributo o petición mal formada, para este caso ver el atributo code para mayor información, endpoint inexistente). <br />•	Error. Error genérico de aplicación.|
| data       | any       |  Contiene el resultado de la operación, si se trata de consultas, este campo contendrá el resultado de la búsqueda.   |
| code       | String       |  Si bien a través del campo status es posible saber si una solicitud se ejecutó la operación deseada, o bien si fue aceptada y validada, en el campo code se devuelve un código específico para cada tipo de operación, en general este campo contiene el código de error cuando status es diferente a success.   |
| message        | String       |  Cuando exista un error en la petición, junto con **code**, este campo contendrá el error verbal, es decir, una descripción del error.   |

Donde la estructura del campo **data** cambiara según el método consumido, todos los demás campos siempre serán del tipo definido en la tabla superior y mantienen su propósito y significado a través de todos los métodos.

# Métodos del API

## Método Validar Lista69B
Este método espera un RFC y lo buscara en la lista de 69B, los parámetros de la petición y respuesta son mostrados a continuación:

### Request

El método y URL que forman la petición para la descarga se describe en la siguiente tabla

~~~HTTP
POST/validadorCFDI/api/v1/Rfc69B
HTTP/1.1
Host:seguridadfiscal.sifei.com.mx
User-Agent:curl/7.58.0Accept:*/*
Content-Type:application/json,text
Authorization:TOKEN

{"rfc":"XXXXXXXXXXXX",}

~~~

### Response (Respuesta)
A continuación se listan ejempos de respuestas.

#### Respuesta encontrado
~~~JSON

{
    "status": "success",
    "data": {
        "rfc": "XXXXXXXXXXXX",
        "nombre_razon_social": "XX.",
        "situacion_contribuyente": "Sentenciafavorable",
        "presuntos": {
            "encontrado": true,
            "numero_and_fecha_oficio_global": "500-05-2018-xxxdefecha01dejuniode2018",
            "fecha_publicacion_sat": "2018-06-0112:22:12",
            "fecha_publicacion_dof": "2018-06-2512:22:12"
        },
        "definitivos": {
            "encontrado": true,
            "numero_and_fecha_oficio_global": "500-05-2018-xxdefecha27deseptiembrede2018",
            "fecha_publicacion_sat": "2018-09-2812:22:12",
            "fecha_publicacion_dof": "2018-10-2312:22:12"
        },
        "desvirtuados": {
            "encontrado": false,
            "numero_and_fecha_oficio_global": null,
            "fecha_publicacion_sat": null,
            "fecha_publicacion_dof": null
        },
        "sentencia_favorable": {
            "encontrado": true,
            "numero_and_fecha_oficio_global": "500-05-2019-xxdefecha5demarzode2019",
            "fecha_publicacion_sat": "0019-03-0512:22:12",
            "fecha_publicacion_dof": "0019-04-1612:22:12"
        }
    },
    "code": null,
    "message": null
}
~~~

#### Respuesta Erronea

~~~JSON
{
    "status": "fail",
    "data": null,
    "code": "4002",
    "message": "rfc vacio"
}

~~~


#### Response falta rfc

~~~JSON
{
    "status": "fail",
    "data": null,
    "code": "4001",
    "message": "falta campo rfc"
}

~~~

#### Response sin token

~~~JSON
{
    "status": "fail",
    "data": null,
    "code": "4001",
    "message": "Falta Token "
}
~~~

## 	Método LCO por RFC

Este método (endpoint) permite buscar un RFC en la lista de contribuyentes obligados. 


### Request 

~~~HTTP
GET /validadorCFDI/api/v1/LCO/rfc/XXXXXXXXXXXXX 
HTTP/1.1
Accept: */*
Accept-Encoding: gzip, deflate
Authorization: valordeltoken
Connection: keep-alive
Host: dev:2828


~~~


#### Responses Encontrado
~~~HTTP
HTTP/1.1 200 OK
Connection: Keep-Alive
Content-Length: 410
Content-Type: application/json
Pragma: no-cache
  
{
    "code": null,
    "data": {
        "certificados": [
            {
                "estado_certificado": "A",
                "fecha_final_cert": "2020-12-31 19:46:03",
                "fecha_inicial_cert": "2016-12-31 19:46:03",
                "numero_serie": "000010000004040000000",
                "rfc": "XXXXXXXXXXXX",
                "validez_obligaciones": 2
            }
        ],
        "encontrado": true,
        "fecha_lista": "2020-02-08",
        "mensaje": "RFC: XXXXXXXXXXXX encontrado en una lista",
        "rfc": "XXXXXXXXXXXX",
        "total_ocurrencias": 1
    },
    "message": null,
    "status": "success"
}


~~~


## 	Método 69 
Este método permite consultar si un RFC aparece en alguna lista del 69.

### Ejemplo no encontrado
#### Request  

~~~HTTP
GET /validadorCFDI/api/v1/Rfc69/XXXX HTTP/1.1
Accept: */*
Accept-Encoding: gzip, deflate
Authorization: valordeltoken
Connection: keep-alive
~~~
#### Response  

~~~HTTP
HTTP/1.1 200 OK
Connection: Keep-Alive
Content-Length: 569
Content-Type: application/json
    
{
  "status": "success",
  "data": {
    "rfc": "XXXX",
    "encontrado": false,
    "mensaje": "RFC no encontrado",
    "total_ocurrencias": 0,
    "listas_encontradas": [],
    "supuestos": [],
    "exigibles": null,
    "firmes": null,
    "no_localizados": null,
    "sentencias": null,
    "cancelados": null,
    "condonados_multas": null,
    "condonados_concurso_mercantil": null,
    "condonados_recargos": null,
    "condonados_por_decreto_22_01_2007_y_26_03_del_2015": null,
    "retorno_inversiones": null,
    "condonados_01_enero_2007_a_04_mayo_2015": null,
    "cancelados_01_enero_2007_a_04_mayo_2015": null,
    "eliminados_no_localizados": null
  },
  "code": null,
  "message": null
}




~~~
### RFC encontrado en más de una lista.


#### Request 
~~~ HTTP
GET /validadorCFDI/api/v1/Rfc69/XXXXXXXXXXXXXX HTTP/1.1
Accept: */*
Accept-Encoding: gzip, deflate
Authorization: valordeltoken
Connection: keep-alive
Host: dev:2828

~~~
#### Reponse

~~~HTTP
HTTP/1.1 200 OK
Connection: Keep-Alive
Content-Length: 1617
Content-Type: application/json    
{
  "status": "success",
  "data": {
    "rfc": "XXXXXXXXXXXXXX",
    "encontrado": true,
    "mensaje": "RFC: XXXXXXXXXXXXXX encontrado en una lista",
    "total_ocurrencias": 3,
    "listas_encontradas": [
      "firmes",
      "no_localizados",
      "cancelados"
    ],
    "supuestos": [
      "FIRMES",
      "NO LOCALIZADOS",
      "CANCELADOS"
    ],
    "exigibles": null,
    "firmes": {
      "encontrado": true,
      "total_ocurrencias_en_lista": 1,
      "fecha_lista": null,
      "ocurrencias": [
        {
          "rfc": "XXXXXXXXXXXXXX",
          "razon_social": "RAZON ",
          "tipo_persona": "F",
          "supuesto": "FIRMES",
          "fecha_primera_pub": "2016-03-16",
          "entidad_federativa": "GUERRERO",
          "monto": null,
          "motivo": null,
          "fecha_public_con_monto": null,
          "ano": null
        }
      ]
    },
    "no_localizados": {
      "encontrado": true,
      "total_ocurrencias_en_lista": 1,
      "fecha_lista": null,
      "ocurrencias": [
        {
          "rfc": "XXXXXXXXXXXXXX",
          "razon_social": "RAZON ",
          "tipo_persona": "F",
          "supuesto": "NO LOCALIZADOS",
          "fecha_primera_pub": "2014-02-01",
          "entidad_federativa": "GUERRERO",
          "monto": null,
          "motivo": null,
          "fecha_public_con_monto": null,
          "ano": null
        }
      ]
    },
    "sentencias": null,
    "cancelados": {
      "encontrado": true,
      "total_ocurrencias_en_lista": 1,
      "fecha_lista": null,
      "ocurrencias": [
        {
          "rfc": "XXXXXXXXXXXXXX",
          "razon_social": "RAZON ",
          "tipo_persona": "F",
          "supuesto": "CANCELADOS",
          "fecha_primera_pub": "2017-11-01",
          "entidad_federativa": "GUERRERO”
          "monto": "6,000",
          "motivo": "",
          "fecha_public_con_monto": "01\/11\/2017",
          "ano": null
        }
      ]
    },
    "condonados_multas": null,
    "condonados_concurso_mercantil": null,
    "condonados_recargos": null,
    "condonados_por_decreto_22_01_2007_y_26_03_del_2015": null,
    "retorno_inversiones": null,
    "condonados_01_enero_2007_a_04_mayo_2015": null,
    "cancelados_01_enero_2007_a_04_mayo_2015": null,
    "eliminados_no_localizados": null
  },
  "code": null,
  "message": null
}

~~~
## LRFC (lista de RFC inscritos no cancelados)

Este método espera un RFC y lo busca en la lista de RFC inscritos no cancelados, tiene 2 modalidades:
-	Modalidad uno a uno (individual)
-	Por lotes (masiva), este modo se activa al detectar comas como separador en el parámetro.


### Uno a uno RFC(en la URL)


#### Request 
~~~ HTTP

GET /validadorCFDI/api/v1/lrfc/ZZY080917LX6 
HTTP/1.1
Authorization: Token
Host: seguridadfiscal.sifei.com.mx

~~~
#### Response

~~~HTTP
HTTP/1.1 200 OK
Transfer-Encoding: chunked
Content-Type: application/json

{
  "status": "success",
  "data": {
    "rfc": "ZZY080917LX6",
    "encontrado": true,
    "tipo_lista": "L_RFC",
    "mensaje": “RFC encontrado”,
    "sncf": "no",
    "subcontratacion": "no"
  },
  "code": null,
  "message": null
}

~~~
### Múltiples RFC 

Notar  las comas entre los RFC, el primer RFC posee caracteres especiales por lo que es codificado, el resto de RFC son encontrado y no encontrado respectivamente
#### Request 
~~~ HTTP
GET /validadorCFDI/api/v1/lrfc/%26%26U9411181C7,ZZY080917LX6,EZY080917LX6 
HTTP/1.1
Authorization: Token
Host: seguridadfiscal.sifei.com.mx


~~~
#### Response

~~~HTTP
HTTP/1.1 200 OK
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Transfer-Encoding: chunked
Content-Type: application/json

{
  "status": "success",
  "data": [
    {
      "rfc": "&&U9411181C7",
      "encontrado": true,
      "tipo_lista": "L_RFC",
      "mensaje": “RFC encontrado”,
      "sncf": "no",
      "subcontratacion": "no"
    },
    {
      "rfc": "ZZY080917LX6",
      "encontrado": true,
      "tipo_lista": "L_RFC",
      "mensaje": “RFC encontrado”,
      "sncf": "no",
      "subcontratacion": "no"
    },
    {
      "rfc": "EZY080917LX6",
      "encontrado": false,
      "tipo_lista": "L_RFC",
      "mensaje": "RFC no encontrado",
      "sncf": null,
      "subcontratacion": null
    }
  ],
  "code": null,
  "message": null
}

~~~

# CENTRO DE SOPORTE TÉCNICO SIFEI
Acceso a recursos de Soporte Técnico de los productos y servicios de SIFEI, Preguntas Frecuentes, Manuales de Usuario, Manuales Técnicos, Notas Técnicas, entre otros 

# ATENCIÓN A INCIDENTES

La atención a incidentes se realizará mediante una herramienta de gestión de incidentes y la comunicación se realizará mediante correo electrónico.  

Correo Electrónico	helpdesk@sifei.com.mx


# HORARIO DE ATENCIÓN

El horario de atención a clientes y de Soporte Técnico para para preguntas, dudas o problemas de la aplicación es:
Lunes a viernes	De 09:00 a 19:00 hrs.

# PÁGINAS OFICIALES DE SIFEI

Sitio web	http://www.sifei.com.mx/
Facebook	http://www.facebook.com/SIFEIMexico
Twitter	http://twitter.com/SIFEIMexico
YouTube	http://www.youtube.com/SIFEIMexico
LinkedIn	http://www.linkedin.com/company/SIFEIMexico 

