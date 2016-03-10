1. Todos los codigos requieren agregar la definicion TRAPFPE, puesto que todos tiran errores en ciertas condiciones (por ejemplo, division por 0 en test_fpe1 y test_fpe2, numeros negativos en test_fpe3, numeros muy grandes en los tres casos). Para linkear adecuadamente, hay que:
- Agregar la definicion -DTRAPFPE al compilar los tres casos.
- Cambiar el `#include fpe_x87_sse.h` por `#include fpe_x87_sse/fpe_x87_sse.h` en los tres casos.
- Agregar el flag `-lm` cuando se linkea test_fpe3 (para que linkee con la libreria matematica).
- Agregar el objeto fpe_x87_sse/fpe_x87_sse.o cuando se linkean los tres casos.

2. Los mensajes de salida se vuelven legibles. Cuando hay algun error, avisa que tipo de error hubo, con un texto legible.
