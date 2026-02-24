# get_next_line

A C function that reads one line at a time from a file descriptor — a project from the **42 School** curriculum.

---

## Project Summary

`get_next_line` implements a function with the following prototype:

```c
char *get_next_line(int fd);
```

Each call returns the next line (including the terminating `\n` when present) from the given file descriptor. On end-of-file or error it returns `NULL`. The function works correctly for both regular files and standard input, and its read buffer size can be configured at compile time via the `BUFFER_SIZE` macro.

---

## What You Learn / Skills Acquired

- **Static variables** — preserving state across multiple function calls in C.
- **Dynamic memory management** — allocating and freeing heap memory without leaks.
- **File I/O in C** — using `open`, `read`, and `close` system calls.
- **String manipulation** — implementing helper utilities (`ft_strlen`, `ft_strdup`, `ft_substr`, `ft_strjoin`, `ft_strchr`) from scratch.
- **Buffer management** — handling partial reads and correctly reassembling lines.
- **Macro customisation** — overriding compile-time constants (`BUFFER_SIZE`) with `-D` flags.
- **Defensive programming** — dealing with edge cases (empty files, lines without `\n`, invalid file descriptors).

---

## Project Structure

```
get_next_line/
├── get_next_line.c        # Core logic: read_find() and get_next_line()
├── get_next_line_utils.c  # Helper functions (ft_strlen, ft_strdup, ft_substr, ft_strjoin, ft_strchr)
└── get_next_line.h        # Header — function prototypes and BUFFER_SIZE default (42)
```

---

## Build & Run Instructions

There is no standalone binary for this project — the source files are meant to be compiled together with your own program.

### 1. Basic compilation

```bash
cc -Wall -Wextra -Werror \
   get_next_line/get_next_line.c \
   get_next_line/get_next_line_utils.c \
   your_main.c \
   -o gnl_test
```

### 2. Custom buffer size

```bash
cc -Wall -Wextra -Werror -D BUFFER_SIZE=128 \
   get_next_line/get_next_line.c \
   get_next_line/get_next_line_utils.c \
   your_main.c \
   -o gnl_test
```

### 3. Minimal usage example (`your_main.c`)

```c
#include <fcntl.h>
#include <stdio.h>
#include <stdlib.h>
#include "get_next_line/get_next_line.h"

int main(void)
{
    int   fd = open("file.txt", O_RDONLY);
    char *line;

    if (fd == -1)
        return 1;
    while ((line = get_next_line(fd)) != NULL)
    {
        printf("%s", line);
        free(line);
    }
    close(fd);
    return 0;
}
```

---

## Usage Notes

- The caller is responsible for **freeing** each returned string.
- Calling `get_next_line` with the same `fd` across multiple calls is correct; it accumulates a static buffer internally.
- Changing `fd` between calls without draining the previous descriptor may leave residual buffered data.
- `BUFFER_SIZE` must be `> 0`; passing `fd < 0` returns `NULL` immediately.

---

## Author

**ruortiz-** — [42 School](https://www.42.fr/)
