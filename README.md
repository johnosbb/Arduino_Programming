# Arduino_Programming
All things Arduino Programming


## Useful Functions

### The map() function

Purpose: Maps a number from one range to another.
Defined In: wiring.c (part of the Arduino Core library).

```c
long map(long value, long fromLow, long fromHigh, long toLow, long toHigh);
Return Type: long (integer type).
```

```c
int analogValue = analogRead(A0); // 0–1023
int mappedValue = map(analogValue, 0, 1023, 0, 100); // Maps to 0–100

Serial.println(mappedValue); // Prints normalized value

```


### The constrain() Function

Purpose: Restricts a number to stay within a specific range.
Defined In: wiring.c (part of the Arduino Core library).


__How it works:__

- If value is less than min, it returns min.
- If value is greater than max, it returns max.
- If value is within the range, it returns the value itself.

```c
long constrain(long value, long min, long max);
```

```c
int sensorValue = analogRead(A0); // 0–1023
int mappedValue = map(sensorValue, 0, 1023, 0, 100); 
mappedValue = constrain(mappedValue, 0, 100); // Ensures value is between 0–100

Serial.println(mappedValue);
```


## Maximising Memory

Minimizing program memory (flash) and SRAM (RAM) usage is important in embedded systems, especially when working with resource-constrained devices like Arduino. Here are several techniques you can use to reduce memory consumption:

### Use const and constexpr for Fixed Data
Program Memory (Flash): When you use const or constexpr for variables, the data is stored in program memory (flash), not SRAM.


```cpp

constexpr int ledPin = 13;  // stored in flash memory
```

### Store Constant Data in Program Memory

Use the PROGMEM keyword to store constant data (like strings, arrays, or lookup tables) in program memory instead of SRAM.

```cpp
const char message[] PROGMEM = "Hello, World!";
```
To read data from PROGMEM, you need to use functions like pgm_read_byte or pgm_read_word.

### Avoid Using String Objects

The Arduino String class can consume a lot of SRAM because it dynamically allocates memory. Instead, use C-style strings (char arrays).

```
// Avoid using String
String str = "Hello, World!";
// Use a char array instead
const char* str = "Hello, World!";
```

### Use Fixed-Size Buffers

For arrays and buffers, avoid dynamic memory allocation. Use fixed-size arrays whenever possible.

``` 
const int bufferSize = 10;
int buffer[bufferSize];  // A fixed-size buffer
```

### Use Data Types Efficiently

Choose smaller data types to save memory. For example, use uint8_t (1 byte) instead of int (2 bytes or more).

```
uint8_t pinValue = 0;  // Instead of using int pinValue;
```

### Use inline Functions Instead of Macros

Inline functions use less memory than macros and provide type safety. If you don't need a macro, use a function with inline keyword.

```
inline int add(int a, int b) {
  return a + b;
}
```

### Minimize Use of Global Variables

Minimize the number of global variables, especially if they are large. Prefer local variables whenever possible, as they are more memory-efficient.

```
// Avoid this:
int globalVariable[100];  // Uses 200 bytes in SRAM

// Use local variables inside functions:
void someFunction() {
  int localVariable[10];  // More efficient
}
```

### Optimize Loop Efficiency

Avoid large, memory-intensive calculations inside loops. Precompute values where possible and use simple math operations.

```
// Instead of recalculating every time inside the loop
for (int i = 0; i < 1000; i++) {
    someFunction(i * 2);  // Avoid unnecessary recalculations
}
```

### Use #define or constexpr for Constants

Using #define or constexpr for constants can reduce memory usage, as these values don't take up space in SRAM.

```
#define MAX_TEMP 30
constexpr int maxTemp = 30;  // Both use minimal memory
```

### Use Efficient Libraries

Use lightweight or optimized libraries that are memory efficient. Some libraries, like Adafruit_SSD1306, have more memory-efficient versions. Always check for optimizations or alternatives.
Example: Instead of using a large String object, use a simple char array or proceed with raw data manipulation.

### Use Memory Pooling

Use memory pooling techniques for managing memory in a more efficient way. Allocate a large block of memory and manage it manually, avoiding fragmentation.

```
char memoryPool[1024];  // Pre-allocate a large memory block
```

### Optimize Use of Functions and Libraries

If you don’t need all the features of a library, check if there’s a lightweight version or if you can exclude some parts of it to reduce program size.
For example, use a simplified OLED library that doesn’t include extra features like font scaling or animations.

### Use Bitfields for Storing Flags

When you need to store several boolean flags, use bitfields instead of using separate bool variables, which can take up a byte each.

```
struct Flags {
    uint8_t flag1 : 1;
    uint8_t flag2 : 1;
    uint8_t flag3 : 1;
};
Flags status;
```

### Optimize Function Calls

Reduce the number of function calls, as each function call adds overhead. Consider merging functions or using inline functions where applicable.

### Check Compiler Optimizations

Use compiler optimizations such as -Os (optimize for size) or specific optimizations for the memory. You can enable them in the Arduino IDE's platform.txt file or manually edit the build flags.

### Defining constants

| **Aspect**          | **const int**             | **#define**               | **constexpr**           |  
|----------------------|---------------------------|---------------------------|-------------------------|  
| **Memory Usage**    | Stored in SRAM (RAM)      | No SRAM usage (macro)     | Stored in Flash (Program Memory) if not referenced dynamically |  
| **Type Safety**     | Type-checked by compiler  | No type safety            | Type-checked by compiler |  
| **Debugging**       | Easier to debug           | Harder to trace errors    | Easier to debug         |  
| **Scope**           | Scoped to the variable    | Global scope             | Scoped to the variable  |  
| **Code Clarity**    | Better readability        | May become unclear        | Better readability      |  
| **Compiler Behavior** | Respects scoping rules   | Simple text replacement   | Respects scoping rules and evaluated at compile-time |  
| **Optimization**    | Compiler may optimize     | No optimization guarantee | Guaranteed compile-time evaluation |  
| **Availability**    | Available in C++98+       | Always available          | Available from C++11+   |  


