# 📌 Punto 2:

## 📝 Actualización de cabeceras 📂

📌 Se incluyen los archivos .h, eliminación de `newval` y se inicializa `valor_encontrado` en `0`.

```diff
< #  include "fb3-2.h"
---
> #  include "fb3-3.h"

<   double *oldval, *newval;	/* saved arg values */
<   double v;
---
>   double *oldval;	/* saved arg values */
>   double v, valor_encontrado = 0;
```
## 🚀 Optimización en la asignación de memoria

📌 Se elimina la reserva de memoria para newval y se mejora la validación de espacio, se remueve la asignación de memoria para newval y se agrega una validación más clara para oldval.

```diff
<   newval = (double *)malloc(nargs * sizeof(double));
<   if(!oldval || !newval) {
<     yyerror("Out of space in %s", fn->name); return 0.0;
---
>   if(!oldval) {
>     yyerror("Out of space in %s", fn->name);
>     return 0.0;
```
## 🏗️ Manejo de valores antiguos y nuevos en variables dummy

📌 Se optimiza la manera en que se guardan los valores antiguos y nuevos; se guarda el valor antiguo de cada variable dummy, se eliminan referencias a newval, usando valor_encontrado y se introduce la variable todos_argumentos_evaluados para verificar si todos los argumentos fueron procesados correctamente.

```diff
>   /* se guardan los valores antiguos de las variables dummy */
>   sl = fn->syms;
>   for(i = 0; i < nargs; i++) {
>     struct symbol *s = sl->sym;
> 
>     oldval[i] = s->value;
>     sl = sl->next;
>   }
> 
>   sl = fn->syms;
>   /* se evaluan y se guardan los valores nuevos en las variables dummy */
>   int todos_argumentos_evaluados = 1; // true

>     struct symbol *s = sl->sym;
>
```
## 🔄 Ajuste en la lógica de evaluación de argumentos

📌 Se reemplazan asignaciones de newval por valor_encontrado, y se mejora la detección de errores, se evita la asignación innecesaria a newval, se utiliza valor_encontrado directamente para almacenar el resultado de la evaluación y se introduce una verificación de error (todos_argumentos_evaluados).

```diff
<       free(oldval); free(newval);
<       return 0;
---
>       todos_argumentos_evaluados = 0; // false;
>       break;

<       newval[i] = eval(args->l);
---
>       valor_encontrado = eval(args->l);

<       newval[i] = eval(args);
---
>       valor_encontrado = eval(args);

<   }
```
## ✅ Asignación de valores y verificación final

📌 Se ajusta la asignación de valores y se implementa una condición para validar si la función debe ejecutarse, se usa valor_encontrado en lugar de newval[i] y se introduce una validación que permite ejecutar la función solo si todos los argumentos se evaluaron correctamente.

```diff
<              
<   /* save old values of dummies, assign new ones */
<   sl = fn->syms;
<   for(i = 0; i < nargs; i++) {
<     struct symbol *s = sl->sym;

<     oldval[i] = s->value;
<     s->value = newval[i];
---
>     s->value = valor_encontrado;

<   free(newval);
< 

<   v = eval(fn->func);
---
>   if (todos_argumentos_evaluados != 0) // distinto a false
>     v = eval(fn->func);
>   else
>     v = 0;

```
