# Reglas de negocio para el boton agregar al carrito

Al dar clic en el botón "Agregar al carrito" se deberán ejecutar las siguientes validaciones, en orden de izquierda a derecha:

```plantuml
@startuml

title Reglas de negocio para el boton agregar al carrito


start

while (¿El valor ingresado en el input es correcto?)  is (No)

switch (Validar el dato)
  case ( Input = 0 )
    :Mostrar error \n "Ingresa una cantidad\n mayor a 0.";
  case ( Input < Min_Qty ) 
    :Mostrar error \n "Favor de introducir\n un valor mayor que o\n igual a 10.";
  case ( Input % Step != 0  )
    :Mostrar error \n "Puede comprar este\n producto solo en\n cantidades de 5 al mismo\n tiempo.";
  case ( Input > Max_Qty )
    :Mostrar error \n "El máximo que\n puede comprar es 50.";
endswitch

endwhile (Yes)

:Agregar al carrito;

stop

@enduml
```