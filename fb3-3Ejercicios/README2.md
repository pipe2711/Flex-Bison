# Punto2:
---
```diff
< #  include "fb3-2.h"
---
> #  include "fb3-3.h"

<   double *oldval, *newval;	/* saved arg values */
<   double v;
---
>   double *oldval;	/* saved arg values */
>   double v, valor_encontrado = 0;

<   newval = (double *)malloc(nargs * sizeof(double));
<   if(!oldval || !newval) {
<     yyerror("Out of space in %s", fn->name); return 0.0;
---
>   if(!oldval) {
>     yyerror("Out of space in %s", fn->name);
>     return 0.0;

<   
<   /* evaluate the arguments */
---
> 
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