# Endpoint A

```text
La funcionalidad de este endpoint es:

```



# Solicitud & Respuesta

## Solicitud

Formato XML

```xml

<SHIP_ADDRESS>
    <SHIP_TYPE>HOME DELIVERY</SHIP_TYPE>
    <CUSTOMER_ID>854732</CUSTOMER_ID>
    <ADDRESS_ID>753945486</ADDRESS_ID>
    <CARRIER_CODE>FEDEX</CARRIER_CODE>
</SHIP_ADDRESS>
```

---

## Respuesta

Formato JSON

```json
{
  "SHIP_INFO": {
    "COST": 1517.16,
    "ASSIGNED_CARRIER": "FEDEX",
    "PICKUP_DATE": "2025-05-02",
    "DELIVERY_ZIPCODE": "54700",
    "EST_DELIVERY": "2025-05-15 20:00"
  }
}
```


---

---

---







# Campos de la Solicitud & Respuesta

## Solicitud

### SHIP_TYPE [Varchar 30]

Existen 3 opciones: \n
HOME DELIVERY
CARRIER PICKUP OFFICE
PICKUP

### CUSTOMER_ID [Integer 16]

El ID del cliente

### ADDRESS_ID [Integer 64]

Si es PICKUP es el ID del centro de distribución
Sino El ID de dirección del cliente

### CARRIER_CODE [Varchar 16]

Para PICKUP el XML NO debe de enviar el dato.
Para los otros casos se puede enviar [UPS, DHL, FEDEX] o ir vacío para su cálculo automático

---

## Respuesta

### COST [Float 10]
El costo de envío

### ASSIGNED_CARRIER [Varchar 16]
Si es PICKUP NO aparecerá este dato en el JSON.
Sino será el carrier asignado para el pedido [UPS, DHL, FEDEX]

### PICKUP_DATE [Date 10]
Es la fecha en la que el Carrier pasará por el pedido en UTC. 
	

### DELIVERY_ZIPCODE [Varchar 12]
Es el código postal donde se entregará el paquete
	
	
### EST_DELIVERY [Date 10]
Es la fecha estimada de envío. En formato UTC. Convertir este dato al presentarlo al cliente
	
	


---

---

---






# Diagrama de flujo - Condiciones de cada campo

````plantuml
@startuml
 
title Solicitud & Respuesta - Condiciones de cada campo
 
start
 
:SOLICITUD;
 
switch (SHIP_TYPE)
  case ( HOME DELIVERY )
    :ADDRESS_ID = \n ID dirección cliente;
    :CARRIER_CODE = \n UPS, FEDEX, DHL o vacio;
  case (    CARRIER PICKUP ) 
    :ADDRESS_ID = \n ID dirección cliente;
    :CARRIER_CODE = \n UPS, FEDEX, DHL o vacio;
  case ( PICKUP  )
    :ADDRESS_ID = \n ID Centro Distribución;
    :</CARRIER_CODE> \n [Dato vacio];
endswitch
 
:RESPUESTA;
 
switch (SHIP_TYPE)
  case ( HOME DELIVERY )
    :ASSIGNED_CARRIER = \n UPS, FEDEX, DHL;
    :DELIVERY_ZIPCODE = \n Código postal del domicilio;
  case (    CARRIER PICKUP ) 
    :ASSIGNED_CARRIER = \n UPS, FEDEX, DHL;
    :DELIVERY_ZIPCODE = \n Código postal de la \n oficina de correo;
  case ( PICKUP  )
    :ASSIGNED_CARRIER = \n NO aparecerá en el JSON;
    :DELIVERY_ZIPCODE = \n Código postal del \n Centro de Distribución;
endswitch
 
stop
 
@enduml
````


---

---

---


# Diagrama de Secuencia UML

````plantuml
@startuml

title "Get Shipping costs"

entity Magento
entity Middleware
entity SAP

Magento -> Middleware : Get Ship Estimations
Middleware -> Middleware: Transform Data
Middleware -> SAP : Get Ship Estimations
SAP -> Middleware: Return ship costs
Middleware -> Middleware: Transform Data
Middleware -> Magento: Return ship costs
Magento -> Magento: Display data to customer


@enduml
````
