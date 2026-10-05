# Known problems of FPC

[**English (en)**](<https://www.freepascal.org/probs.html>) **Español (es)**

# Problemas conocidos de FPC

  * Cuándo el compilador está instalado en una ruta que contiene **nombres largos** :



    El enlazador GNU (el que utiliza FPC) para DOS y windows no soporta nombres largos, por tanto, no instales el compilador en un directorio con una ruta que incluya nombres largos y no compiles fuentes que estén ubicadas en rutas semejantes. No obstante lo anterior, la librería en tiempo de ejecución si soporta nombres largos y por tanto los programas compilados podrán hacer uso de ellos. ¡¡Este problema se produce únicamente en la versión DOS y windows!!

  * En modo Delphi, los conjuntos no coinciden en tamaño con los de Delphi. Las enumeraciones tienen siempre 4 octetos (0..32 elementos) o 32 octetos ((33..256 elementos), y no su tamaño real.


  * La longitud de la documentación en texto plano es > 80 caracteres.


  * Las siguientes funcionalidades de Delphi no se han implementado a día de hoy: 
    * Interfaces _Display_ (dispinterface) y el modificador de indice dispip.
    * Paquetes y _sharemem_ , y la funcionalidad para DLL relacionada
    * Delegación con la palabra clave _implements_
    * La convención de llamada para registros pone los argumentos en orden incorrecto en la pila (Los parámetros a la izquierda son introducidos primero)


  * La herramienta utilizada para crear la documentación en texto desde fuentes TeXT no es capaz de de crear archivos con longitud de línea mayor de 80 caracteres.


  * Incompatibilidades conocidas con Delphi: 
    * Al usar un enlazador externo y por cuestiones de portabilidad entre plataformas, no es posible declarar una función en un interfaz de una unidad, y volver as declararla como externa en la implementación. Una función externa debe declararse completamente en la interfaz, y sólo en la interfaz..


  * Errores conocidos **específicos de _Darwin/Mac OS X_ y _Classic Mac OS_**
    * El paso de parámetros en coma flotante a rutinas C mediante elipsis (cómo en printf) no funciona hoy día para AIX ABI (que es utilizado por todo los SO mencionados).
    * El paso por valor de registros a rutinas C no funciona para registros cuyo tamaño es diferente de 1, 2 y 4 octetos.
    * La creación de librerías compartidas no está soportada.
    * El tipo moneda (_currency_) tiene algunos problemas con valores cercanos a los límites superior e inferior (debido a que el tipo _double_ se utiliza para algunas conversiones, y su precisión es demasiado pequeño para estos casos).



## Fuente

[Problemas conocidos (Original y actualizado)](<http://www.freepascal.org/probs.html>)

---

_Source: [https://wiki.freepascal.org/Known_problem_of_FPC](https://web.archive.org/web/20240619105442/https://wiki.freepascal.org/Known_problem_of_FPC)_
