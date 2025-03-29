# Flex-Bison

### Realizamos los siguientes comandos para BISON:

```
bison -d fb3-1.y
```
### Para los archivos FLEX:

```
flex -o fb3-1.lex.c fb3-1.l
```

### Luego compilamos los archivos generados:

```
cc -o fb3-1 fb3-1.tab.c fb3-1.lex.c fb3-1funcs.c -ll
```

### Por ultimo el comando para el ejecutable:

```
./fb3-1
```

### Si todo sale bien deberiamos ya obtener lo siguiente:

```
6 + 6
= 12
```
