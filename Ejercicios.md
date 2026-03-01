# Tema 5. Diseño y realización de pruebas

## Bloque 1. Rompe el programa

### Ejercicio 1.

```java
public static int calcularPrecioFinal(int precioBase, int descuento) { 
	return precioBase - (precioBase * descuento / 100); 
} 
```

| ID | Descripción | Entrada | Esperado | Oráculo |
| :--- | :--- | :--- | :--- | :--- |
| TC01 | Números enteros | a=100,b=50 | 50 | 50 |
| TC02 | Números decimales | a=3.33, b=14.7 | 3 | 2.8405 |
| TC03 | Precio negativo | a=-100,b=50 | -50 | -50 |
| TC04 | Decimales negativos | a=-5.2,b=33.3 | 3 | 3.4667 |
| TC05 | Descuento negativo | a=100,b=-25 | Error | 125 |
| TC06 | Precio y descuento negativo | a=-50,b=-50 | Error | -75 |
| TC07 | Precio nulo | a=null,b=50 | Error | Error |
| TC08 | Entero sobre INT_MAX | a=MAX+1,b=50 | Error | -2147483648 |


### Ejercicio 2.

¿Qué ocurre si descuento es negativo?
> Si `descuento` es negativo hace lo contrario y añade una cantidad al precio.

¿Y si es mayor que 100?
> Devuelve un precio negativo, se debe interpretar como que el vendedor debe ‘un valor’ a la persona que aplica el descuento.

¿El método debería permitir esos valores?
> No, deberían estar controlados con unos parámetros condicionales y devolver un mensaje de error.


### Ejercicio 3.

Asumiendo que:

- Es un establecimiento real y local (usa euros, redondeados a dos decimales).

- No acepta un descuento mayor a 100 y ningún input negativo.

```java
public static double calcularPrecioFinal(double precioBase, double descuento) { 
        if (precioBase < 0 || descuento < 0 || descuento > 100){ 
            throw new IllegalArgumentException ("Los parámetros no pueden ser 
            negativos ni estar manipulados"); 
        } 

        double precioConDescuento = precioBase - (precioBase * descuento / 100); 
        return Math.round(precioConDescuento * 100.0) / 100.0; 

} 
```

## Bloque 2. El cazador de bugs

### Ejercicio 4.

Observa el siguiente método:
```java
public static int maximo(int[] datos) {
    int max = 0;
    for (int i = 0; i < datos.length; i++) {
        if (datos[i] > max) {
            max = datos[i];
        }
    }
    return max;
}
```

Estas son 3 entradas que hacen que el método falle:

- **Entrada de un array nulo.**

    Resultado obtenido:		java: variable num might not have been initialized

    Resultado correcto: 		Error:"Input es un campo obligatorio”

    Tipo de fallo: 			NullPointerException 


- **Entrada de un array negativo**

    Resultado obtenido: 	0

    Resultado correcto: 		-1

    Tipo de fallo: 			Fallo lógico

- **Entrada de un número mayor a 2147483647 (int max_value + n)**

    Resultado obtenido: 	java: integer number too large

    Resultado correcto: 		Error: “El número no puede ser mayor a 						2147483647”

    Tipo de fallo: 			Límite indefinido


### Ejercicio 5.

Escribe una versión correcta del método.

```java
public static int maximo(int[] datos) { 
    if (datos == null || datos.length == 0) { 
        throw new IllegalArgumentException("Input es un campo obligatorio"); 
    } 

    long max = datos[0]; 

    for (int i = 0; i < datos.length; i++){ 
        if (datos[i] > Integer.MAX_VALUE) { 
            throw new IllegalArgumentException("El número no puede ser mayor a 
            2147483647"); 
    } 

    if (datos[i] > max){ 
        max = datos[i]; 
        } 
    } 
    return (int) max; 
} 
```

## Bloque 3. Diseña el oráculo

### Ejercicio 6.

Observa el siguiente método:
```java
public static int[] ordenar(int[] datos) {
    // algoritmo desconocido
}
```

### Ejercicio 6

Define 3 propiedades que siempre debe cumplir el resultado.

- Los elementos deben estar dispuestos de menor a mayor `resultado[i] <= resultado[i+1]`

- El array devuelto debe tener exactamente la misma cantidad de posiciones que el array de entrada. `resultado.length == datos.length`

- El array resultante debe contener exactamente los mismos elementos.

### Ejercicio 7

Explica por qué aquí es mejor usar oráculos por propiedades que valores concretos.

Porque el método no siempre devuelve un valor plano igual a otro caso. Depende de una serie de condiciones, mucho más manejables con condicionales.

## Bloque 4. Caminos y decisiones

A la vista del siguiente método:

```java
public static String clasificarEdad(int edad) {
    if (edad < 0) {
        return "ERROR";
    } else if (edad < 12) {
        return "NIÑO";
    } else if (edad < 18) {
        return "ADOLESCENTE";
    } else {
        return "ADULTO";
    }
}
```

### Ejercicio 8

Indica el número mínimo de tests necesarios.

> La cantidad mínima de tests se establecen por las posibles vías que comparten uno o más problemas. En este caso hay cuatro caminos posibles, `4`.

### Ejercicio 9

Propón esos tests.