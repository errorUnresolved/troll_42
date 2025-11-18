*This project has been created as part of the 42 curriculum by jrebuffa.*

# libft

## Description

libft is a small C library implemented for learning purposes as part of the 42 curriculum. Its goal is to reimplement a set of standard C library functions (string, memory, character checks and simple utilities) to gain a deeper understanding of low-level C behavior, API contracts, edge cases, and safe coding practices. The library builds as a static archive (`libft.a`) that can be linked into other projects.

## Instructions

### Build

```
make
```

This compiles all source files and creates the archive `libft.a`.

### Cleaning

* Remove object files:

```
make clean
```

* Remove object files and the static library:

```
make fclean
```

* Rebuild everything from scratch:

```
make re
```

### Using the Library

To compile a program with libft:

```
cc -I. -L. -lft your_program.c -o your_program
```

Or link the archive explicitly:

```
cc -I. your_program.c libft.a -o your_program
```

## Compilation Flow (Visual Overview)

```
+---------------------+
|     Source Files    |
|    (ft_*.c files)   |
+----------+----------+
           |
           v
+---------------------+
|   Compilation Step  |
|   gcc -Wall -Wextra |
|     -Werror -c      |
+----------+----------+
           |
           v
+---------------------+
|     Object Files    |
|        (*.o)        |
+----------+----------+
           |
           v
+---------------------+
|   Archiving Step    |
|  ar rcs libft.a     |
+----------+----------+
           |
           v
+---------------------+
|  Your Program Link  |
| cc main.c -L. -lft  |
+---------------------+
```

## Detailed Description of the Library

Below is a complete explanation of each function included in the library. Descriptions are concise but accurate, reflecting expected behavior according to standard C conventions.

### **Function Definitions**

#### **Character checks and conversions**

* **ft_isalpha** — returns 1 if the character is alphabetical, else 0.
* **ft_isdigit** — returns 1 if the character is a numeric digit ('0'–'9').
* **ft_isalnum** — returns 1 if the character is alphanumeric.
* **ft_isascii** — returns 1 if the character is a valid ASCII value (0–127).
* **ft_isprint** — returns 1 if the character is printable (including space).
* **ft_toupper** — converts a lowercase letter to uppercase if applicable.
* **ft_tolower** — converts an uppercase letter to lowercase if applicable.

#### **Memory operations**

* **ft_memset** — fills a memory block with a specific byte value.
* **ft_bzero** — sets a memory block to zero.
* **ft_memcpy** — copies memory from one location to another (no overlap allowed).
* **ft_memmove** — safely copies memory even if regions overlap.
* **ft_memchr** — scans memory for the first occurrence of a byte.
* **ft_memcmp** — compares two memory blocks byte-by-byte.
* **ft_calloc** — allocates zero-initialized memory (nmemb × size).

#### **String operations**

* **ft_strlen** — returns the length of a C string.
* **ft_strlcpy** — safely copies a string into a buffer of given size.
* **ft_strlcat** — safely concatenates strings with size-bound checking.
* **ft_strchr** — finds the first occurrence of a character in a string.
* **ft_strrchr** — finds the last occurrence of a character in a string.
* **ft_strncmp** — compares two strings up to a given number of characters.
* **ft_strnstr** — locates a substring within a bounded search area.
* **ft_strdup** — allocates and returns a duplicate of a string.

#### **Conversions and parsing**

* **ft_atoi** — converts a numeric string to an integer, handling signs and whitespace.
* **ft_itoa** — converts an integer to a newly allocated string.

#### **Higher-level utilities**

* **ft_substr** — extracts a substring from a given string.
* **ft_strjoin** — concatenates two strings into a new allocated result.
* **ft_strtrim** — removes specified characters from both ends of a string.
* **ft_split** — splits a string by a delimiter into an array of strings.
* **ft_strmapi** — applies a function to each character of a string, returning a new one.
* **ft_striteri** — applies a function to each character of a string in place.
* **ft_putchar_fd** — writes a single character to a file descriptor.
* **ft_putstr_fd** — writes a string to a file descriptor.
* **ft_putendl_fd** — writes a string followed by a newline to a file descriptor.
* **ft_putnbr_fd** — writes an integer to a file descriptor.

#### **Linked list functions**

* **ft_lstnew** — creates a new list node with given content.
* **ft_lstadd_front** — inserts a node at the beginning of the list.
* **ft_lstadd_back** — inserts a node at the end of the list.
* **ft_lstdelone** — frees the content of a node using a given function, then frees the node.
* **ft_lstclear** — deletes all nodes in a list using a provided deletion function.
* **ft_lstiter** — applies a function to each element's content.
* **ft_lstmap** — creates a new list by applying a function to each element of an existing list.

---

The library is organized into several functional categories. All prototypes are declared in `libft.h`.

### 1. Character Checks & Conversions

Functions:

* `ft_isalpha`, `ft_isdigit`, `ft_isalnum`, `ft_isascii`, `ft_isprint`
* `ft_toupper`, `ft_tolower`

**Purpose:** Mimic `<ctype.h>` behavior. These functions test character classes or convert case, handling values representable as `unsigned char` or EOF.

### 2. Memory Operations

Functions:

* `ft_memset`, `ft_bzero`, `ft_memcpy`, `ft_memmove`, `ft_memchr`, `ft_memcmp`
* `ft_calloc`

**Purpose:** Perform raw memory manipulation. `ft_memmove` specifically supports overlapping regions. `ft_calloc` allocates and zeroes memory.

### 3. String Operations

Functions:

* `ft_strlen`, `ft_strlcpy`, `ft_strlcat`, `ft_strchr`, `ft_strrchr`
* `ft_strncmp`, `ft_strnstr`, `ft_strdup`

**Purpose:** Provide essential string manipulation routines following C standard conventions. Safe copy/concat follow OpenBSD `strl*` semantics.

### 4. Conversions & Parsing

Functions:

* `ft_atoi`, `ft_itoa`

**Purpose:** Convert between integers and strings, mirroring common libc behaviors (handling whitespace, signs, overflow patterns where applicable).

### 5. Higher-level Utilities

Functions:

* `ft_substr`, `ft_strjoin`, `ft_strtrim`, `ft_split`
* `ft_strmapi`, `ft_striteri`
* `ft_putchar_fd`, `ft_putstr_fd`, `ft_putendl_fd`, `ft_putnbr_fd`

**Purpose:** Extended helpers for string creation, manipulation with callbacks, and simple file descriptor output.

### Implementation Notes

* Functions return NULL on allocation failure.
* String functions using sizes follow safe `strl*` behaviors to prevent buffer overflows.
* Thread safety depends on caller responsibility when sharing mutable memory.

## Files of Interest

### Directory Structure Visualization

Here is the directory tree:

```

project/
├── Makefile
├── libft.a
└── README.md

src/
├── char_checks/
│   ├── ft_isalpha.c
│   ├── ft_isdigit.c
│   ├── ft_isalnum.c
│   ├── ft_isascii.c
│   ├── ft_isprint.c
│   ├── ft_toupper.c
│   └── ft_tolower.c
│
├── memory/
│   ├── ft_memset.c
│   ├── ft_bzero.c
│   ├── ft_memcpy.c
│   ├── ft_memmove.c
│   ├── ft_memchr.c
│   ├── ft_memcmp.c
│   └── ft_calloc.c
│
├── strings/
│   ├── ft_strlen.c
│   ├── ft_strlcpy.c
│   ├── ft_strlcat.c
│   ├── ft_strchr.c
│   ├── ft_strrchr.c
│   ├── ft_strncmp.c
│   ├── ft_strnstr.c
│   └── ft_strdup.c
│
├── conversion/
│   ├── ft_atoi.c
│   └── ft_itoa.c
│
├── utils/
│   ├── ft_substr.c
│   ├── ft_strjoin.c
│   ├── ft_strtrim.c
│   ├── ft_split.c
│   ├── ft_strmapi.c
│   ├── ft_striteri.c
│   ├── ft_putchar_fd.c
│   ├── ft_putstr_fd.c
│   ├── ft_putendl_fd.c
│   └── ft_putnbr_fd.c
│
└── linked_list/
    ├── ft_lstnew.c
    ├── ft_lstadd_front.c
    ├── ft_lstadd_back.c
    ├── ft_lstdelone.c
    ├── ft_lstclear.c
    ├── ft_lstiter.c
    └── ft_lstmap.c

```
- **Makefile** — defines build targets (`all`, `clean`, `fclean`, `re`).
- **libft.h** — public header containing prototypes.
- **Source files (`*.c`)** — implementation of each function.

## Resources
- Man pages (e.g., `man 3 memcpy`, `man 3 strchr`, `man 3 strlcpy`).
- OpenBSD documentation on `strlcpy` / `strlcat`.
- The official 42 Libft subject.
- Articles on safe memory and string handling.
- 42 Gitbook Guide
- Libft Tester (Tripouille and WarMachine)

## AI Usage Disclosure
An AI assistant was used to help draft and structure this README.
All code and implementation decisions were made by the repository owner.
In some cases, an AI assistant was also used to help debug issues or generate simple test mains for verifying functions.

## License / Attribution
This repository contains original work created by jrebuffa as shown below, for the 42 curriculum. Reuse or redistribution should respect the author's intent and 42’s rules.

```
::::..::.:.:::::..:.......:.:.....:.......:::..::..:.:..:::::::..::::::::::::::::::::::::::::::::::::
::::.:..:.::......:.............................:.....::.::::::::::::::::::::::::::::::::::::::::::::
...::.::.:....:........:.....:...:.....:......:..:....:..::::::::.:::::::::::::::::::::::::::::::::::
::::.:...:................:....................:.......:..::::.:..::::.::::::::::::::::::::::::::::::
:..:.....:...:.........................................:.:.::.::..:::::::::::::::::::::::::::::::::::
...........:.................................................:.:..::.::::::::::::::::::::::::::::::::
......:.................................................:....:....:::::::::::::::::::::::::::::::::::
..................................................-:+-#%%%%%#%%##%%:.:.:::.::::::::::::::::::::::::::
...........................................:.:%@@@%#%@@@@@@@@@@@@@@@@@*--::::::::::::::::::::::::::::
...........................................+%%@@%@@@@@@%@@@%%%%%%%%%%@@@@%=::::::::::::::::::::::::::
...........................................#@@%@%##**+==---------------==+%@%-=-.-:::::::::::::::::::
........................................:%@%*+--------::::::::::::--------===%%%%##::::::::::::::::::
......................................=%@%-------::::::..:.:::.:::::------=====#@@@%*-:::::::::::::::
.....................................%%%--------:::::::::::.:::::::-----=======++%@#*=*::::::::::::::
..........:::---....................%#+-------::::::::::::::::::::::----=====+++++*%*-.::::::::::::::
..........::--===.................:%%=---------::::::::::::::::::::::::---===++++++#%::::::::::::::::
.........::-===++*.................%*==-------::::::::::::::::::::::::-----===+++++*#::::::::::::::::
.......:::-=+++***.................#=----------:---==---:::::.:::::=-++*#%%%%##*++++*::::::::::::::::
........:-==+**##*................++-------=*#%#%#%%##*+====--=====+#*##%#######%*+++::::::::::::::::
....:::-==++***#*.................#*-----+*#***#######*****+*****######%######*****++::::::::::::::::
...-:---=+#****-..................-#----=++++**#%%%%@@@%%##*+-=*#%%%%@@@%***#%###**++::::::::::::::::
.::::-=++*****.....................#----=++#%%%%%@%%%%%##*+--::=*%%%%@@%%@%#%%%%%#**+#*+:-:::::::::::
::::-==+****=..................-##==*---==--=**##%%%#++=--=--::-=++***#%%#####%*+++++%##:::::::-:::::
::::-==+***...................:=+-:=*=-------:--:-:----=-----:::-++++===----====+++++*#*+::::::-:::::
::::-+++**...................::==:--*+---::::::::::-----------::-=+++=====----==+++++#++----:---::--:
:::......:::::--...............-=:-..+=-----:--:---====------:.:-=++=+*+=-======+++*+-*=-----:-:-:-::
:....::::::-=======-...........:-::..+==========+*#*+++=---=--=-=++++*****++++++++***++==--------:-:-
:::::-===++*++++++++====.........:---+#=====++*#%#===++**###++****%%%%#+**#%#*********++==--------:--
::.::::::-=+***##***+++=====......::::=+++==+#@#*++++***#%%%%%#%@@@@@@##%**#%@*****#%**++=-----------
::....:::--==++*###***+++++====....::::#+++++*%%####**#*#*#%@%%%%%%%%#####%%%%+**###****++==---------
:.:::--==========+%#*******++++==.::::::*++++++##**%@*+*+*++####%**#%@@%%##%*++*####****++==---------
-=+===+++*****+++=====%##******++::.::::-++**++**+====+%%%%*@@@@@%@%%%******+**%###*****++==---------
+*****#***#####*++++=+====+#*****.::::::--#*+*++*+=======-:=:=**#####*******#*##%*******++==---------
--=+*#############***+++++++==-:...:::::-===***++++=+=+++*+++**##%#%#***###*%#%%#********++==--------
:::-=++*######%#####******++++++..:::--===++=####+==+=+++**##*######**+***%%%%#***********+===-------
:---==++++*****#%@@%####********::::--==++++=++%*#++===--===*+++++==++*##@%%@#************++==-------
-==+****++++++=======+%@%##****=::::--==++++:=+**%%#*#**++++++*++***#%%%@@@@##************++==-------
**************+*++++++++===+.:::::::---....:+=+++#@%%%%%##*###%####%%@@%%%######%+********++===------
-=++*##*########**********+++:::...........:*===+**#%#%@@@@@@@@%@@@@%#@%#%########@%=++=-=...:.:-----
:-===++#%@@@%##*************+...............-====+++#*#%@%%*#####@@@%%%#############%@+-:--=:-:--....
=++++=====--------===+#***-..................=-=-==++++**##%###%%%#%%###############***%:...:==:.:...
+**##*****++============-......................=---=+++++***#####################***++==.....:.......
#############*###*****++..........................=-===++++***###############*****++====.:...........
################*###**+.................:...:..........=++***##############**++-::..................:
#*****+.-:......:..............................-.:.:.:..........:......:..................:......:.:-
..............................................:........:-:::::::.:-:::-:::..:..:.::........:...::.:..
.................................................::.:...:..:::::.:::.::..:::::.:..:..:..::-:.........
.........::...........................................--:::::::.::::-:::-:-::-::----.:...............
.........:.............................................:.:-.::-:::-::=-:::...........................
........:..:................................................:::.::::..:..:...........................
......::.::..:...........:.:::.::.:.::..............................................................:
......:..:..:..........::::-::--::-:::.........................................................:.....
....::..::.:..:..:.::::--:-=--=----::..................................................:...:...:...:.
...:...:..::.::.:::::---=-====---=:::.:....................................................:...-.....
..::.:-::-:::-::---=========-=-:-:::..............................................:....:..:...:..:...
:--::-:-=---=--=======+====---:-:::..:..:.:.......................................:...:...:...:......
```
