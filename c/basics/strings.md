String is a sequence of characters terminated with a null character `\0` at the end.

# How to declare strings

Declaring string is similar to one-dimensional character array. Below is the syntax:

```c
char var-name[size];
```

# How to initialize strings

Strings can be initialized in a number of ways:
```c
char str[] = "OneCompiler";

char str[20] = "OneCompiler";

char str[] ={'O','n','e','C','o','m','p','i','l','e','r','\0'};

char str[20] ={'O','n','e','C','o','m','p','i','l','e','r','\0'}
```

# How to read strings from console

```c
#include <stdio.h>
int main()
{
    char str[20];
    scanf("%s", str); // to read the string values from stdin
    printf("You have entered: %s", str); // to print the sting
    return 0;
}
```
### Check Result [here](https://onecompiler.com/c/3vm5393vz)

Consider if you have given `One Compiler` as input in the stdin. Surprisingly you get `One` alone as output. That is because a white space is present between `One` and `Compiler`.  So how can you read a entire line which also has white spaces in it.

Consider your input string is `Hello Everyone.. Happy Learning!!`

```c
#include <stdio.h>
int main()
{
    char str[50];
    fgets(str, sizeof(str), stdin);  // to read string from stdin
    printf("You entered: ");
    puts(str);    // to print entire string
    return 0;
}
```

### Check Result [here](https://onecompiler.com/c/3vm53mc6r)

# String Functions

C has various in-built string functions which can manipulate null-terminated strings.

| Function name | Description|
|----|----|
|strlen(str)| to return the length of string str|
|strcat(str1, str2)| to concatenate string str2 onto the end of string str1.|
|strcpy(str1, str2)| To copy string str2 into string str1.|
|strcmp(str1, str2)| returns 0 if str1 and str2 are the same and less than 0 if str1 < str2 and a positive number if str1 > str2|
|strchr(str, c)| Returns a pointer to the first occurrence of character c in string str|
|strstr(str1, str2)| Returns a pointer to the first occurrence of string str2 in string str1|


## Examples

```c
#include <stdio.h>
#include <string.h>

int main () {

   char str1[20] = "Happy";
   char str2[20] = "learning";
   char str3[20];
   char str4[20] = "learning";
   char str[50] = "Hello world, learning is fun.";

   int  length, cmp , cmp1 ;
   int *ptr, * ptr1;  
   
   length = strlen(str1); // to find length of a string
   printf("length of string is :  %d\n", length );
   
   strcat( str1, str2); // concatenates str1 and str2 
   printf("Concatenation of str1 and str2:   %s\n", str1 );

   strcpy(str3, str1); // to copy a string into another
   printf("string copy of str1 to str3 :  %s\n", str3 );

   cmp = strcmp(str2,str4); // to compare two strings
   printf("string compare result :  %d\n", cmp );
   
   
   cmp1 = strcmp(str1,str4); // to compare two strings
   printf("string compare result :  %d\n", cmp1 );
   
   ptr = strchr(str1, 'p'); // usage of strchr
   printf("pointer to the first occurrence of p in string Happy :  %p\n", (void*)ptr );
   
   ptr1 = strstr(str, str2); // usage of strstr
   printf("pointer to the first occurrence of str2 in str :  %p\n", (void*)ptr1 );
   

   return 0;
}
```

### Check result [here](https://onecompiler.com/c/3vm54ajd2)

# Buffer Overflow Risks and Safe String Handling

## Buffer Overflow Prevention

String operations in C can lead to buffer overflows if not handled carefully. Here are common risks and safer alternatives:

### Unsafe vs Safe String Operations

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

int main() {
    char buffer[10];
    char large_input[50] = "This string is too long for the buffer";
    
    // UNSAFE: strcpy doesn't check buffer size
    // strcpy(buffer, large_input);  // This would cause buffer overflow!
    
    // SAFE: Use strncpy with size limit
    strncpy(buffer, large_input, sizeof(buffer) - 1);
    buffer[sizeof(buffer) - 1] = '\0';  // Ensure null termination
    printf("Safe copy: %s\n", buffer);
    
    // UNSAFE: strcat doesn't check buffer size
    char dest[20] = "Hello ";
    char src[] = "World, this is a long string";
    // strcat(dest, src);  // This could cause buffer overflow!
    
    // SAFE: Use strncat with size limit
    strncat(dest, src, sizeof(dest) - strlen(dest) - 1);
    printf("Safe concatenation: %s\n", dest);
    
    return 0;
}
```

## Safer String Handling Practices

### 1. Always Check Buffer Bounds

```c
#include <stdio.h>
#include <string.h>

// Safe string copy function
void safe_strcpy(char *dest, const char *src, size_t dest_size) {
    if (dest_size > 0) {
        strncpy(dest, src, dest_size - 1);
        dest[dest_size - 1] = '\0';
    }
}

// Safe string concatenation function
void safe_strcat(char *dest, const char *src, size_t dest_size) {
    size_t dest_len = strlen(dest);
    if (dest_len < dest_size - 1) {
        strncat(dest, src, dest_size - dest_len - 1);
    }
}

int main() {
    char buffer[20];
    
    safe_strcpy(buffer, "Hello", sizeof(buffer));
    printf("After safe copy: %s\n", buffer);
    
    safe_strcat(buffer, " World!", sizeof(buffer));
    printf("After safe concatenation: %s\n", buffer);
    
    return 0;
}
```

### 2. Input Validation and Length Checking

```c
#include <stdio.h>
#include <string.h>

int main() {
    char input[50];
    
    printf("Enter a string (max 49 characters): ");
    
    // SAFER: Use fgets instead of scanf for string input
    if (fgets(input, sizeof(input), stdin) != NULL) {
        // Remove newline character if present
        size_t len = strlen(input);
        if (len > 0 && input[len - 1] == '\n') {
            input[len - 1] = '\0';
        }
        
        printf("You entered: %s\n", input);
        printf("Length: %zu characters\n", strlen(input));
    } else {
        printf("Input error\n");
    }
    
    return 0;
}
```

### 3. Dynamic Memory Allocation for Strings

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

char* safe_string_duplicate(const char* source) {
    if (source == NULL) return NULL;
    
    size_t len = strlen(source);
    char* copy = malloc(len + 1);
    
    if (copy != NULL) {
        strcpy(copy, source);
    }
    
    return copy;
}

int main() {
    const char* original = "Hello, World!";
    char* copy = safe_string_duplicate(original);
    
    if (copy != NULL) {
        printf("Original: %s\n", original);
        printf("Copy: %s\n", copy);
        
        // Don't forget to free allocated memory
        free(copy);
    } else {
        printf("Memory allocation failed\n");
    }
    
    return 0;
}
```

### 4. Modern C String Functions (C11)

If your compiler supports C11, you can use safer string functions:

```c
#define __STDC_WANT_LIB_EXT1__ 1
#include <stdio.h>
#include <string.h>

int main() {
    char dest[20];
    const char* src = "Hello, World!";
    
    // C11 safe string functions (if available)
    #ifdef __STDC_LIB_EXT1__
    if (strcpy_s(dest, sizeof(dest), src) == 0) {
        printf("Safe copy successful: %s\n", dest);
    } else {
        printf("Safe copy failed\n");
    }
    #else
    // Fallback to manual safe copy
    if (strlen(src) < sizeof(dest)) {
        strcpy(dest, src);
        printf("Manual safe copy: %s\n", dest);
    } else {
        printf("Source string too long\n");
    }
    #endif
    
    return 0;
}
```

## Best Practices Summary

1. **Always specify buffer sizes** when using string functions
2. **Use `strncpy()` instead of `strcpy()`** and ensure null termination
3. **Use `strncat()` instead of `strcat()`** and check remaining space
4. **Use `fgets()` instead of `scanf()`** for reading strings
5. **Validate input lengths** before processing
6. **Initialize buffers** and always null-terminate strings
7. **Free dynamically allocated memory** to prevent memory leaks
8. **Consider using safer alternatives** like `snprintf()` for formatted strings
