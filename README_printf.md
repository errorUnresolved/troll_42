*This project has been created as part of the 42 curriculum by jrebuffa.*

# ft_printf

## Description

ft_printf is a custom implementation of the standard C library function `printf()`, created as part of the 42 curriculum. The goal is to recreate the core functionality of printf, handling various format specifiers while gaining a deep understanding of variadic functions, string formatting, type conversions, and low-level output operations. The project builds as a static library (`libftprintf.a`) that can be linked into other projects.

## Project Requirements

The project must:
- **NOT** implement the buffer management of the original printf()
- Handle the following conversions: `c` `s` `p` `d` `i` `u` `x` `X` `%`
- Be compared against the original printf() for correctness
- Return the number of characters printed (same as printf)
- Be compiled with `-Wall -Wextra -Werror`

## Supported Format Specifiers

| Specifier | Description                         | Example                                   |
|-----------|-------------------------------------|-------------------------------------------|
| `%c`      | Single character                    | `ft_printf("%c", 'A')` → `A`              |
| `%s`      | String of characters                | `ft_printf("%s", "Hello")` → `Hello`      |
| `%p`      | Pointer address in hexadecimal      | `ft_printf("%p", ptr)` → `0x7fff5fbff7a0` |
| `%d`      | Signed decimal integer              | `ft_printf("%d", -42)` → `-42`            |
| `%i`      | Signed decimal integer (same as %d) | `ft_printf("%i", 42)` → `42`              |
| `%u`      | Unsigned decimal integer            | `ft_printf("%u", 42)` → `42`              |
| `%x`      | Hexadecimal (lowercase)             | `ft_printf("%x", 255)` → `ff`             |
| `%X`      | Hexadecimal (uppercase)             | `ft_printf("%X", 255)` → `FF`             |
| `%%`      | Literal percent sign                | `ft_printf("%%")` → `%`                   |

## Instructions

### Build

```bash
make
```

This compiles all source files and creates the archive `libftprintf.a`.

### Cleaning

* Remove object files:

```bash
make clean
```

* Remove object files and the static library:

```bash
make fclean
```

* Rebuild everything from scratch:

```bash
make re
```

### Using the Library

To compile a program with ft_printf:

```bash
cc your_program.c libftprintf.a -o your_program
```

Or with explicit flags:

```bash
cc -Wall -Wextra -Werror your_program.c libftprintf.a -o your_program
```

### Example Usage

```c
#include "ft_printf.h"

int main(void)
{
    int count;
    
    count = ft_printf("Hello, %s!\n", "World");
    ft_printf("Characters printed: %d\n", count);
    
    ft_printf("Number: %d, Hex: %x, Pointer: %p\n", 42, 255, &count);
    
    return (0);
}
```

## Architecture Overview

### File Structure

```
ft_printf/
├── Makefile
├── ft_printf.h              (header with all prototypes)
├── ft_printf.c              (main function + dispatcher)
├── ft_printf_utils.c        (basic output functions)
├── ft_printf_numbers.c      (decimal/unsigned printing)
├── ft_printf_hex.c          (hexadecimal + pointer printing)
├── ft_printf_len.c          (length calculation helpers)
└── README.md
```

### Function Organization

**Total: 13 functions across 5 files**

#### ft_printf.c (2 functions)
- `ft_printf()` - Main entry point, variadic function handler
- `ft_type()` - Format specifier dispatcher

#### ft_printf_utils.c (2 functions)
- `ft_putchar_fd()` - Write single character to file descriptor
- `ft_putstr()` - Write string with counter tracking

#### ft_printf_numbers.c (3 functions)
- `ft_printid()` - Print signed integers (%d, %i)
- `ft_printu()` - Print unsigned integers (%u)
- `ft_putnbr_fd()` - Recursive number printer with fd support

#### ft_printf_hex.c (4 functions)
- `ft_printhex()` - Print hexadecimal (%x, %X)
- `ft_puthex()` - Static helper for hex conversion
- `ft_printp()` - Print pointer addresses (%p)
- `ft_putptr()` - Recursive pointer-to-hex converter

#### ft_printf_len.c (2 functions)
- `ft_numlen()` - Calculate decimal number length
- `ft_ptrlen()` - Calculate hexadecimal length

## Function Call Flow Map

### How ft_printf Works

When you call `ft_printf()`, the format string is parsed character by character. Regular characters are printed directly, while format specifiers (starting with `%`) trigger the dispatcher to call specialized handlers.

### Flow Diagram

```
┌─────────────────────────────────────────┐
│     User calls ft_printf(format, ...)   │
└──────────────────┬──────────────────────┘
                   │
                   ▼
         ┌─────────────────────┐
         │   Parse format[]    │
         │   character by char │
         └──────────┬──────────┘
                    │
        ┌───────────┴───────────┐
        │                       │
        ▼                       ▼
  ┌──────────┐           ┌────────────┐
  │ Regular  │           │ Found '%'  │
  │   char   │           │  specifier │
  └────┬─────┘           └──────┬─────┘
       │                        │
       ▼                        ▼
  ft_putchar_fd()        ft_type() dispatcher
       │                        │
       │         ┌──────────────┼──────────────┐
       │         │              │              │
       │         ▼              ▼              ▼
       │    ┌────────┐    ┌─────────┐   ┌──────────┐
       │    │  %c/%% │    │  %s     │   │  %d/%i   │
       │    └────┬───┘    └────┬────┘   └────┬─────┘
       │         │             │             │
       │         ▼             ▼             ▼
       │   ft_putchar_fd  ft_putstr    ft_printid
       │                                     │
       │         ┌───────────────────────────┼────────────┐
       │         │              │            │            │
       │         ▼              ▼            ▼            ▼
       │    ┌────────┐    ┌─────────┐  ┌────────┐  ┌────────┐
       │    │  %u    │    │  %x/%X  │  │   %p   │  │ other  │
       │    └────┬───┘    └────┬────┘  └────┬───┘  └────┬───┘
       │         │             │            │           │
       │         ▼             ▼            ▼           ▼
       │    ft_printu    ft_printhex   ft_printp   ft_putchar_fd
       │         │             │            │           │
       └─────────┴─────────────┴────────────┴───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │  Return total count  │
                    └──────────────────────┘
```

### Detailed Format Specifier Routing

#### `%c` - Single Character
```
ft_printf("Char: %c", 'A')
    ↓
ft_type() sees 'c'
    ↓
ft_putchar_fd('A', 1)
    ↓
write(1, "A", 1)
    ↓
counter++
```

#### `%s` - String
```
ft_printf("String: %s", "Hello")
    ↓
ft_type() sees 's'
    ↓
ft_putstr("Hello", &counter)
    ↓
Loop: ft_putchar_fd('H', 1), ft_putchar_fd('e', 1), ...
    ↓
counter += 5
```
**Special case:** NULL pointer prints `(null)`

#### `%d` or `%i` - Signed Integer
```
ft_printf("Number: %d", -42)
    ↓
ft_type() sees 'd'
    ↓
ft_printid(-42, &counter)
    ├─> ft_putnbr_fd(-42, 1)     [prints: -42]
    │       ↓
    │   write(1, "-", 1)
    │   Recursively print digits
    │
    └─> ft_numlen(-42)           [returns: 3]
        ↓
    counter += 3
```

#### `%u` - Unsigned Integer
```
ft_printf("Unsigned: %u", 12345)
    ↓
ft_type() sees 'u'
    ↓
ft_printu(12345, &counter)
    ↓
Recursive division by 10
    ↓
ft_putchar_fd for each digit
    ↓
counter += 5
```

#### `%x` / `%X` - Hexadecimal
```
ft_printf("Hex: %x", 255)
    ↓
ft_type() sees 'x'
    ↓
ft_printhex(255, 'x', &counter)
    ├─> ft_puthex(255, 'x')      [prints: ff]
    │       ↓
    │   Recursive base-16 conversion
    │   Uses 'a'-'f' for lowercase
    │   Uses 'A'-'F' for uppercase
    │
    └─> ft_ptrlen(255)           [returns: 2]
        ↓
    counter += 2
```

#### `%p` - Pointer Address
```
int x = 42;
ft_printf("Pointer: %p", &x)
    ↓
ft_type() sees 'p'
    ↓
ft_printp((uintptr_t)&x, &counter)
    ↓
Check if NULL → print "(nil)"
    OR
    ├─> write(1, "0x", 2)        [prints: 0x]
    │   counter += 2
    │
    └─> ft_putptr(address, &counter)
            ↓
        Recursive base-16 conversion
        Increments counter directly
```

#### `%%` - Literal Percent
```
ft_printf("100%%")
    ↓
ft_type() sees '%'
    ↓
ft_putchar_fd('%', 1)
    ↓
counter++
```

## Implementation Details

### Key Design Decisions

#### 1. Counter Tracking
Every printing function either:
- Returns a value added to the counter, OR
- Modifies the counter directly via pointer

This ensures accurate character counting without checking `write()` return values.

#### 2. Type Safety with uintptr_t
For pointer operations, we use `uintptr_t` (from `<stdint.h>`):
- Guaranteed to be large enough for any pointer
- Allows arithmetic operations (division, modulo)
- Portable across 32-bit and 64-bit systems

#### 3. Recursive Printing
Functions like `ft_putnbr_fd()`, `ft_printu()`, and `ft_putptr()` use recursion to:
- Handle digits/characters in correct order
- Simplify the logic
- Avoid building intermediate strings

#### 4. NULL Handling
- `%s` with NULL pointer → prints `(null)`
- `%p` with NULL pointer → prints `(nil)`

### Edge Cases Handled

| Case                        | Input                          | Output        |
|-----------------------------|--------------------------------|---------------|
| NULL string                 | `ft_printf("%s", NULL)`        | `(null)`      |
| NULL pointer                | `ft_printf("%p", NULL)`        | `(nil)`       |
| Zero                        | `ft_printf("%d", 0)`           | `0`           |
| INT_MIN                     | `ft_printf("%d", -2147483648)` | `-2147483648` |
| Negative as unsigned        | `ft_printf("%u", -1)`          | `4294967295`  |
| Empty string                | `ft_printf("%s", "")`          | (nothing)     |
| Just %%                     | `ft_printf("%%")`              | `%`           |
| No conversions              | `ft_printf("Hello")`           | `Hello`       |

### Memory Management

- **No dynamic allocation** in core printing functions
- All helper functions use stack memory or direct output
- Safe for embedded/constrained environments

### Performance Considerations

- Single-pass parsing (O(n) where n = format string length)
- Minimal function call overhead
- Direct `write()` syscalls (no buffering like standard printf)
- Recursive functions have bounded depth (max digits in number)

## Testing

A comprehensive tester is included that compares ft_printf output with standard printf:

```bash
cc -Wall -Wextra -Werror main_tester.c libftprintf.a -o tester
./tester
```

The tester validates:
- All format specifiers
- Edge cases (NULL, INT_MIN/MAX, empty strings)
- Mixed format strings
- Return value accuracy
- Character-by-character output matching

## Compilation Flow

```
┌─────────────────────┐
│   Source Files      │
│   (ft_*.c)          │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Compilation Step   │
│  cc -Wall -Wextra   │
│  -Werror -c         │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│   Object Files      │
│   (*.o)             │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Archiving Step     │
│  ar rcs             │
│  libftprintf.a      │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Link with Program  │
│  cc main.c          │
│  libftprintf.a      │
└─────────────────────┘
```

## Resources

- `man 3 printf` - Standard printf documentation
- `man stdarg` - Variadic function macros
- `man 3 write` - Low-level write syscall
- C99 Standard - Format specifier specifications
- 42 ft_printf subject PDF
- ft_printf testers (Tripouille, etc.)

## Limitations

As per project requirements:
- **No buffer management** (output is not buffered like standard printf)
- **No field width or precision** (e.g., `%10d`, `%.5s` not supported)
- **No flags** (e.g., `-`, `+`, `0`, ` `, `#` not supported)
- **No length modifiers** (e.g., `l`, `ll`, `h` not supported)

## AI Usage Disclosure

An AI assistant was used to:
- Help draft and structure this README
- Debug compilation issues
- Generate the comprehensive tester
- Optimize code organization

All core implementation and design decisions were made by the repository owner.

## License / Attribution

This repository contains original work created by jrebuffa for the 42 curriculum. Reuse or redistribution should respect the author's intent and 42's rules.

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

---

*Created with dedication for the 42 curriculum - jrebuffa, 2025*
