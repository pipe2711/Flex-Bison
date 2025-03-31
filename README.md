# 🚀 Flex-Bison

# 📌 Comandos esenciales

## 📁 Utilizando MakeFile:

📌 El uso más común de Makefiles es administrar las dependencias de los archivos fuente de los programas durante la fase de compilación y enlazado (build) , es decir, compilar sólo los archivos que necesitan ser compilados.

📌 En la terminal ejecutamos el siguiente comando:

```
make fb3-1

```

📌 Si queremos eliminar los archivos podemos utilizar el comando:

```
make clean
```
⚠️ *Se necesita tener instalado make si no lo tienes instalado ejecuta el siguiente comando*

🍏 Para MacOS:

```
brew install make
```

🌪️ Para distribuciones basadas en Debian:

```
sudo apt install make
```

### 🎯 Ejecutar el programa:
```bash
./fb3-1
```

## Sin usar MakeFile:

### 🎯 Generar los archivos con **Bison**:
```bash
bison -d fb3-1.y
```

### 🎯 Generar los archivos con **Flex**:
```bash
flex -o fb3-1.lex.c fb3-1.l
```

### 🎯 Compilar los archivos generados:
```bash
cc -o fb3-1 fb3-1.tab.c fb3-1.lex.c fb3-1funcs.c -ll
```

### 🎯 Ejecutar el programa:
```bash
./fb3-1
```

⚠️ *Si necesitas limpiar los archivos generados antes de recompilar, usa:*
```bash
rm -f fb3-1.tab.c fb3-1.tab.h fb3-1.lex.c fb3-1
```

---
## ✅ Resultado esperado:
```bash
6 + 6
= 12
```

---

## 🔄 Cambios y mejoras

### 📝 **Actualización de cabeceras** 📂
📌 Se incluyen los archivos `.h` (archivos cabecera) y se actualizan los tokens.

```diff

< #  include "fb3-2.h"
---
> #  include "fb3-3.h"

< %token IF THEN ELSE WHILE DO LET
---
> %token IF THEN ELSE WHILE DO LET DONE FI END

```

### 🎯 **Mejoras en la gramática** 📜
📌 Se ha modificado la gramática para mejorar la sintaxis, eliminando la necesidad de ';' dobles y agregando palabras clave de finalización (`FI`, `DONE`).

📌 Se añade una regla a la gramatica para evita ambigüedades, la cual consiste en agregar un ';' al final de cada expresion (exp).
 
```diff

< stmt: IF exp THEN list           { $$ = newflow('I', $2, $4, NULL); }
<    | IF exp THEN list ELSE list  { $$ = newflow('I', $2, $4, $6); }
<    | WHILE exp DO list           { $$ = newflow('W', $2, $4, NULL); }
<    | exp
---
> stmt: IF exp THEN list FI          { $$ = newflow('I', $2, $4, NULL); }
>    | IF exp THEN list ELSE list FI { $$ = newflow('I', $2, $4, $6); }
>    | WHILE exp DO list DONE        { $$ = newflow('W', $2, $4, NULL); }
>    | IF exp THEN list FI ';'          { $$ = newflow('I', $2, $4, NULL); }
>    | IF exp THEN list ELSE list FI ';' { $$ = newflow('I', $2, $4, $6); }
>    | WHILE exp DO list DONE ';'        { $$ = newflow('W', $2, $4, NULL); }
> 
>    | exp ';' /* Para que la gramática no sea ambigua
>    O si no "exp" y "exp list" se reducen a lo mismo (list)
>    */

```

### 🛠 **Corrección en la estructura de la gramática** 🏗
📌 Se eliminó el `;` del `stmt` y se ajustó el número de parámetros.
```diff
<    | stmt ';' list { if ($3 == NULL)
---
>    | stmt list { if ($2 == NULL)

< 			$$ = newast('L', $1, $3);
---
> 			$$ = newast('L', $1, $2);

```

### 🔚 **Añadida la palabra clave `END` para funciones** 🎯
📌 Ahora, las funciones finalizan con `END` para mayor claridad.

```diff

<   | calclist LET NAME '(' symlist ')' '=' list EOL {
---
>   | calclist LET NAME '(' symlist ')' '=' list END EOL {

---
>   | calclist LET NAME '(' symlist ')' '=' list END ';' EOL {
>                        dodef($3, $5, $8);
>                        printf("Defined %s\n> ", $3->name); }

```

