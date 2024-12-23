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


