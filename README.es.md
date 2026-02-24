# get_next_line

Una función en C que lee una línea a la vez desde un descriptor de archivo — proyecto del currículo de la **Escuela 42**.

---

## Resumen del proyecto

`get_next_line` implementa una función con el siguiente prototipo:

```c
char *get_next_line(int fd);
```

Cada llamada devuelve la siguiente línea (incluido el carácter `\n` si está presente) del descriptor de archivo indicado. Al llegar al final del archivo o en caso de error, devuelve `NULL`. La función funciona correctamente tanto con archivos regulares como con la entrada estándar, y el tamaño del búfer de lectura puede configurarse en tiempo de compilación mediante la macro `BUFFER_SIZE`.

---

## Qué se aprende / Habilidades adquiridas

- **Variables estáticas** — preservar el estado entre múltiples llamadas a una función en C.
- **Gestión dinámica de memoria** — asignar y liberar memoria en el heap sin fugas.
- **E/S de archivos en C** — uso de las llamadas al sistema `open`, `read` y `close`.
- **Manipulación de cadenas** — implementar utilidades auxiliares (`ft_strlen`, `ft_strdup`, `ft_substr`, `ft_strjoin`, `ft_strchr`) desde cero.
- **Gestión de búferes** — manejo de lecturas parciales y reconstrucción correcta de líneas.
- **Personalización con macros** — sobreescribir constantes en tiempo de compilación (`BUFFER_SIZE`) con la opción `-D`.
- **Programación defensiva** — tratar casos límite (archivos vacíos, líneas sin `\n`, descriptores de archivo inválidos).

---

## Estructura del proyecto

```
get_next_line/
├── get_next_line.c        # Lógica principal: read_find() y get_next_line()
├── get_next_line_utils.c  # Funciones auxiliares (ft_strlen, ft_strdup, ft_substr, ft_strjoin, ft_strchr)
└── get_next_line.h        # Cabecera — prototipos de funciones y valor por defecto de BUFFER_SIZE (42)
```

---

## Instrucciones de compilación y ejecución

Este proyecto no genera un binario independiente — los archivos fuente deben compilarse junto con tu propio programa.

### 1. Compilación básica

```bash
cc -Wall -Wextra -Werror \
   get_next_line/get_next_line.c \
   get_next_line/get_next_line_utils.c \
   tu_main.c \
   -o gnl_test
```

### 2. Tamaño de búfer personalizado

```bash
cc -Wall -Wextra -Werror -D BUFFER_SIZE=128 \
   get_next_line/get_next_line.c \
   get_next_line/get_next_line_utils.c \
   tu_main.c \
   -o gnl_test
```

### 3. Ejemplo mínimo de uso (`tu_main.c`)

```c
#include <fcntl.h>
#include <stdio.h>
#include <stdlib.h>
#include "get_next_line/get_next_line.h"

int main(void)
{
    int   fd = open("archivo.txt", O_RDONLY);
    char *linea;

    if (fd == -1)
        return 1;
    while ((linea = get_next_line(fd)) != NULL)
    {
        printf("%s", linea);
        free(linea);
    }
    close(fd);
    return 0;
}
```

---

## Notas de uso

- El llamador es responsable de **liberar** cada cadena devuelta.
- Llamar a `get_next_line` con el mismo `fd` en múltiples llamadas es correcto; la función acumula un búfer estático internamente.
- Cambiar `fd` entre llamadas sin haber vaciado el descriptor anterior puede dejar datos residuales en el búfer.
- `BUFFER_SIZE` debe ser `> 0`; pasar `fd < 0` devuelve `NULL` de inmediato.

---

## Autor

**ruortiz-** — [Escuela 42](https://www.42.fr/)
