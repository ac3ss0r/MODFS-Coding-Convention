# MODFS CODING CONVENTION
A custom coding convention for most relevant languages designed for maximum readability, performance and safety.

## Namings

1. Namings in the source code should be based on the standard library namings for the used programming language. This makes the code clean & readable when using the default libraries.

```C#
class CSharpClass {
    void CSharpMethod() {}
}
```
```Java
class JavaClass {
    void javaMethod() {}
}
```
```C++
class cpp_class {
    void cpp_method() {}
}
```

2. Macros (both methods & constant definitions) should be capital case unless existing members are redefined on purpose (for example, obfuscation). This way it's always easy to differentiate macro usages from regular members usages.

```C
#define TEST    // <--- GOOD
#define TEST(x)

#define test
#define test(x) // <--- BAD
```

## Formatting

1. The braces should be always at the same line. This involves class, method, loop, branch and all other definitions. This reduces the overall code height and makes it more readable.

```C++
class foo {
int foo () {
if (foo) {
while (foo) {
for (;;;) {
```

2. Identation should be always 4 spaces, tabs shouldn't be used.

```C++
char *get_extension(char *path) {
    char *result = {0};
    while (*path) {
    if (*path == '.')
            result = path + 1;
        else if (*path == '\\' || *path == '/')
            result = {0};
        path++;
    }
    return result;
}
```

4. If the loop/condition block is 1 line braces shouldn't be used. This requirement is not strict, but recommended

```C++

if (cond) { // <--- BAD
    action;
}

if (cond)   // <--- GOOD
    action;
while (cond)
    action;
for (int i=0; i<10;i++)
    action;
```

5. There should be 1 line break between member definitions but only if their height is > 1 line. This involves methods, fields, etc

```C++
void foo() {
    // Body
}

void bar() { 
    // Body
}

void foo() { }
void bar() { }
int foo;
int bar;
```

6. Line breaks should be used to mark logical blocks, or not used at all. Line breaks shouldn't be used without a proper reason

```C++
struct info;           // Initialize user
info.name = "Ivan";
info.surname = "Ivanov";

struct server;         // Initialize server
server.address = "127.0.0.1";
server.port = 8080;

if (connect(server)) { // Connect & send data
    printf("Connected to server\n");
    server.send(info);
    server.disconnect();
}
```

8. The code should be as compact as possible but still remain clean & readable.

## Implementation

1. Recursion usage should be avoided as much as possible to prevent stack overflow & control flow errors. Recursive implementations can be "unwrapped" to use loops instead.

```C++

int factorial(int n) {       // <--- BAD 
    if (n == 0) return 1;
    else return n * factorial(n - 1);
}

int factorial(int n) {       // <--- GOOD
    int result = 1;
    for (int i = 1; i <= n; i++)
        result *= i;
    return result;
}
```

2. All loops should have fixed bounds to prevent soft-locking the program. This doesn't involve string-related loops, such as strlen/strtok, etc.

```C++
while (1)                     // <--- BAD
while (1 && i < MAX_ITER)     // <--- GOOD
```

3. Heap usage (malloc, calloc, new, etc.) should be avoided as much as possible. This doesn't involve pre-made libraries which are considered to be 100% secure (linked lists, for example).

```C++
header* parse_header(char *data) {          // <--- BAD
    header* result = (header*)malloc(sizeof(header));
    // fill up result
    return result;
}

int parse_header(char *data, header *ptr) { // <--- GOOD
    int status;
    // fill up header ptr by ref
    return status;
}
```

4. Avoid embeding multiple braced expressions (if/for/while etc.) inside each other, use **break**, **continue**, **return** instead. This prevents deep bracing & improves readability. This will get very noticeable while writing large and complex methods.

```C++
void itarate_modules(module* ptr) { // <--- BAD
    while (*ptr) {
        if (ptr->is_native || ptr->has_data) {
            method* mtd_list = module->methods;
            while (*method) {
                if (method->is_public || method->is_static) {
                    // Process method
                }
                method++;
            }
        }
        ptr++;
    }
}

void itarate_modules(module *ptr) { // <--- GOOD
    while (*ptr) {
        if (!ptr->is_native || !ptr->has_data)
            continue;
        method* mtd_list = module->methods;
        while (*method) {
            if (!method->is_public || !method->is_static)
                continue;
            // Process method
            method++;
        }
        ptr++;
    }
}
```

<div align=center>
    <img width="100%" src="Images/branch_sample.png"><br/>
    <text>This is what might happen if this rule isn't followed</text>
</div>
<br/>

5. Use header-guards named by the header filename in capital-case. Don't use just #pragma once, since it's not supported by all compilers - use either both or just header guards.

```C
#ifndef HEADER_H // <--- GOOD
#define HEADER_H
// the header code
#endif

#pragma once    //  <--- BAD
```

## Class Rules

1. Private members should have a **_** suffix.

```C++
class shape {
private:
    int height_;
    int width_;
};
```

2. Always explicitly specify the visibility of class members.

```C++
class shape { // <--- BAD
    int height_;
    int width_;
};

class shape { // <--- GOOD
private:
    int height_;
    int width_;
};
```

3. Prefer seperating variable and method declarations by visibility sections.

```C++
class shape {
public:
    int height;
    int width;

public:
    void draw() { }
    void resize() { }
}
```

4. Private section should be at the end of the class.

```C++
class shape {
public:
    int height;
    int width;

private:
    void draw() { }
    void resize() { }
};
```

5. Separate between visibility sections for readability.

```C++
class shape { // <--- BAD
public:
    int height;
    int width;
private:
    void draw() { }
    void resize() { }
};

class shape { // <--- GOOD
public:
    int height;
    int width;

private:
    void draw() { }
    void resize() { }
};
```

6. Avoid using inheritance when unnecessary.
