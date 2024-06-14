
# CODE FORMATTING

1. Identation is always 4 spaces, tabs should never be used

```C++
void foo() {
	return 1;
}
```

2. The braces should always be at the same line. This involves class, method, loop, branch and all other definitions. This allows the code to be compact (take less screen height) and readable as much as possible

```C++
class foo {
void foo () {
if (foo) {
while (foo) {
```

3. There should be 1 line break between methods definitions but only the ones which are > 1 line in height. Same for fields

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

4. Line breaks should be only used when needed to mark logical code blocks

```C++
struct user;
user.name = "Ivan";
user.surname = "Ivanov";

struct server;
server.address = "192.168.0.1";
connect(server);
```

5. The code should be as compact as possible but still remain clean & readable.

# IMPLEMENTATION

1. Never use recursion (replace with loops if needed)

2. All loops should have fixed bounds to prevent soft-locking the program.

```C++
while (1) // wrong
while (1 && i < MAX_ITER)
```

3. Heap usage should be avoided as much as possible. No dynamic allocations (eg. malloc & free). This doesn't include pre-made essentials, such as linked lists and libraries

4. Cast ignored returns values to void if it returns anything

(void)printf("foo");
