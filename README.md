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
