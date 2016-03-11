# Floating point exception

1. Todos los codigos requieren agregar la definicion TRAPFPE, puesto
que todos tiran errores en ciertas condiciones (por ejemplo, division
por 0 en test_fpe1 y test_fpe2, numeros negativos en test_fpe3,
numeros muy grandes en los tres casos). Para linkear adecuadamente,
hay que:

   - Agregar la definicion -DTRAPFPE al compilar los tres casos.

   - Cambiar el `#include fpe_x87_sse.h` por `#include
fpe_x87_sse/fpe_x87_sse.h` en los tres casos.

   - Agregar el flag `-lm` cuando se linkea test_fpe3 (para que linkee
con la libreria matematica).

   - Agregar el objeto fpe_x87_sse/fpe_x87_sse.o cuando se linkean los
tres casos.

2. Los mensajes de salida se vuelven legibles. Cuando hay algun error,
avisa que tipo de error hubo, con un texto legible.

---

# Segmentation Fault

1. El error que sale al correr big.x es un *segmentation fault*. Luego
de ejecutar `ulimit -s unlimited` deja de aparece.

2. Ejecutando `ulimit -s unlimited` se quita la cota máxima al stack
accesible por el usuario (por defecto, en esta computadora, es de
8192kb=8Mb). En el caso de small.x, el stack necesario ronda los
8bytes*500**2=2Mb, mientras que en el caso de big.x, es
8bytes*2500**2=50Mb.

3. No es una solución en el sentido de debugging porque no soluciona
el problema en general. Este es un código que va a seguir teniendo un
stackoverflow cada vez que se corra. Una posible solución sería alocar
con malloc la variable (*temp*) que no se encuentra alocada, pidiendo
la cantidad de espacio que necesita.