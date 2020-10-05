# Ejercicio Nº 0 - Contador de Palabras

## Nicolás Riedel (102130)

### Taller de Programación I (75.42) - Cátedra Veiga - UBA



## Pasos

### Paso 0: Entorno de Trabajo

1. Capturas de pantalla de la ejecución del aplicativo (con y sin **_Valgrind_**)

   ​		Sin **_Valgrind_**

   ![hola_mundo_crudo](Capturas/hola_mundo_crudo.png)

   ​		 Con **_Valgrind_**

   ![hola_mundo_crudo](Capturas/hola_mundo_valgrind.png)

   

2. ¿Para qué sirve **_Valgrind_**? ¿Cuáles son sus opciones más comunes?

   **_Valgrind_** es una herramienta que nos permite visualizar como maneja la memoria nuestro programa.  

   Sus usos mas comunes son:

    * --leak-check=full 

      ​	Encuentra leaks (o "perdidas" de memoria).

    * --track-origins=yes

      ​	Para encontrar variables sin inicializar .(Lo que puede llevar a errores de memoria)

    * --show-reachable=yes

      ​	Muestra todos los bloques de memoria referenciados,  nos alerta si se perdió la referencia a alguno.

3. ¿Qué representa sizeof()? ¿Cuál sería el valor de salida de sizeof(char) y sizeof(int)?

   sizeof() es una función estándar de C que recibe una variable/tipo de dato y devuelve el tamaño en bytes que ocupa en memoria. La salida de sizeof(char) y sizeof(int)  representa el tamaño de un char o un int en la arquitectura que estemos trabajando. En la mayoría de los entornos actuales un char tiene un tamaño de 1 byte y un entero de 4.

4. ¿El sizeof() de una struct de C es igual a la suma del sizeof() de cada uno sus elementos? Justifique mediante un ejemplo

​	No necesariamente. El compilador por defecto guarda las variables en posiciones múltiplos de 4, entonces si guardamos un struct compuesto por un entero 	seguido de un char, este en memoria tendrá un sector de _padding_ para que la próxima variable quede alineada en memoria. 

​	Código de ejemplo :

```c
#include <stdio.h>
struct S {
	int a;
    char b;
};

int main(){
	printf("El tam de un entero es: %zu \n",sizeof(int));
	printf("El tam de un char es: %zu \n",sizeof(char));
	printf("El tam del struct conformado por ambos es: %zu \n",sizeof(struct S) );
	return 0;
}
```



​	y la salida del programa es: 

​	![size_struct](Capturas/size_struct.png)

​	Con esto podemos observar que se agregan 3 bytes de padding para que el struct quede alineado en memoria. 

5. Investigar la existencia de los archivos estándar: STDIN, STDOUT, STDERR. Explicar brevemente su uso y cómo redirigirlos en caso de ser necesario (caracteres > y <) y como conectar la salida estándar de un proceso a la entrada estándar de otro con un pipe (carácter | ).                                                             

   Son los streams de texto estándar para todo sistema basado en UNIX.

    * STDIN : Es usado para entrada de datos.
   	* STDOUT : Es usado para salida de datos.
   	* STDERR : Es usado para mostrar mensajes de error.

   Estos pueden ser redirigidos con los caracteres '>' y '<' .

   Por ejemplo si queremos redirigir la salida de nuestro primer ejemplo a un archivo de texto basta con ejecutar el programa como:

    `./tp 1 > salida.txt`

   y el "Hola mundo" sera impreso en el archivo de texto en lugar de STDOUT.

   Ahora si en lugar de redirigir STDOUT queremos redirigir el STDERR tenemos que ejecutar.

    `./tp 2> error.txt`

   y si queremos hacerlo con ambos streams debemos

    `./tp 1> salida.txt 2>error.txt`

   (siendo 1 el indicador de STOUT y 2 el de STDERR)

   Análogamente si queremos hacer algo similar con la entrada tendremos que ejecutar : 

    `./tp 1 < entrada.txt`

   En lo que respecta a conectar la salida estándar de un proceso con la entrada estándar de otro, esto se realiza mediante un pipe. 

   Digamos que queremos capturar la salida de nuestro primer ejemplo ("Hola mundo") con la entrada estándar de otro. 

   `./tp | ./lector  `

   En este caso "lector" recibirá por STDIN la salida de "tp". 



### Paso 1:  SERCOM - Errores de generación y normas de programación

1. 