# My Code School

## Introduction to Pointers in C/C++

### 1) Introduction to pointers in C/C++

First point is that we have to understand how various data types or various variables are stored in computers memory. Memory -> RAM.

**Pointers** - variables that store address of another variable.

N  | Memory
---|------
208|b = 10
204|a = 4
   |
   |
   |
64 |p = 204

```C++
int a;
int *p; // points to an integer - store this adress

p = &a; // &a gives us the address of that particular variable

printf("%d\n", p);  // 204
printf("%d\n", &a); // 204
printf("%d\n", &p); // 64
printf("%d\n", *p); // 5

*p = 8; // modify value of p and a
printf("%d", a); // 8
```

### 2) Working with pointers

```C++
int a;  // integer
int *p; // pointer to integer

char c;   // character
char *p0; // pointer to character

double d;   // double
double *p1; // pointer to double

p = &a; // p points to a
```

```C++
int a = 10;
int *p;
p = &a; // &a = adress of a

printf("%d\n", p);  // 3800000
printf("%d\n", *p); // 10
printf("%d", &a);   // 3800000

*p = 12; // deferencing
printf("%d", a); // 12
```

```C++
// these declarations are the same:

int *p;
int* p;
```

```C++
int a = 10;
int *p;
p = &a;

printf("Size of integer is %d bytes.\n", sizeof(int));

printf("%d", p);     // 2002
printf("%d", p + 1); // 2006
printf("%d", p + 2); // 2010

printf("%d", *(p + 1)); // garbage value
```

### 3) Pointer types, pointer arithmetic, void pointers

Pointers are strongly typed.

int*-> int

char* -> char

int -> 4 bytes

char -> 1 byte

float -> 4 bytes

```C++
int a = 1025;
int *p;
p = &a;

printf("Size of integer is %d bytes.", sizeof(int)); // 4
printf("Address = %d, value = %d\n", p, *p); // 4123338 | 1025
printf("Adress = %d, value = %d\n", p + 1, *(p + 1)); // 4123340 | garbage

char *p0;
p0 = (char*)p; // typecasting
printf("Size of char is %d bytes.", sizeof(char)); // 1
printf("Address = %d, value = %d\n, p0, *p0); // 4123338 | 1
// 1025 = 00000000 00000000 00000100 00000001
```

```C++
int a = 1025;
int *p;

// void pointer - generic pointer
void *p0; // it's not mapped to a particular datatype

p0 = p; // there's no need to do casting
printf("Adress = %d\n.", p0); // 3344104
```

### 4) Pointers to Pointers in C/C++

```C++
int x = 5;
int *p = &x;
*p = 6;

int **q; // pointer to pointer
q = &p   // q can store the address of p

int*** r = &q;


printf("%d\n", *p);       // 6
printf("%d\n", *q);       // memory address of x
printf("%d\n", *(*q));    // 6
printf("%d\n", *(*r));    // memory address of x
printf("%d\n", *(*(*r))); // 6

***r = 10;
printf("%d\n", x); // 10
```

### 5) Pointers as Function Arguments - Call by Reference

When a program on an application is started computer sets aside (reserves) some amount of memory for the execution of this program.

|Application Memory|
|------------------|
|Heap|
|Stack|
|Static/Global|
|Code (Text)|

Example of calling by **value**:

```C++
void increment(int a)
{
   a = a + 1;
}

int main()
{
   int a;
   a = 10;
   increment(a);
   printf("a = %d\n", a); // 10

   return 0;
}
```

Example of calling by **reference**:

```C++
void increment(int *p)
{
   *p = *p + 1;
}

int main
{
   int a;
   a = 10;
   increment(&a); // passing the address of the variable
   printf("a = %d\n", a); // 11

   return 0;
}
```

It is important to know that we are saved from creating a new copy of a complex datatype.

### 6) Pointer and Arrays

#### Array

```C++
int A[5];
```

Memória:

| Address | Value |
|---------| ------|
|216| A[4]|
|212| A[3]|
|208| A[2]|
|204| A[1]|
|200| A[0]|

#### Pointers

Element at index ```i```:

- Adress: ```&A[i]``` or ```(A + i)```
- Value: ```A[i]``` or ```*(A + i)```

```C++
// Pointers and Arrays

int main()
{
   int A[] = {2, 4, 5, 8, 1};

   printf("%d\n", A);     // address of the first element
   printf("%d\n", &A[0]); // address of the first element
   printf("%d\n", A[0]);  // print first element (value)
   printf("%d\n", *A);    // print the first element (value)

   return 0;
}
```

```C++
int main()
{
   int A[] = {2, 4, 5, 8, 1};

   for(int i = 0; i < 5; i++)
   {
      printf("Address = %d\n", &A[i]);
      printf("Address = %d\n", A + i);
      printf("Value = %d\n", A[i]);
      printf("Value = %d\n", *(A + 1));
   }

   return 0;
}
```

### 7) Arrays as Function Arguments

```C++
// arrays as function arguments
int sumOfElements(int A[], int size)
{
   int i, sum = 0;

   for(i = 0; i < size; i++)
   {
      sum += A[i];
   }
   return sum;
}

int main()
{
   int A[] = {1, 2, 3, 4, 5};

   int sizeOfA = sizeof(A); // calculate size of the array
   int numberOfElements = sizeof(A)/sizeof(A[0]); // calculate the number of elements

   int total = sumOfElements(A, size);
   printf("Sum of elements = %d\n", total); // 15

   return 0;
}
```

The compiler implicitly converts this ```int A[]``` to ```int *A```. So ```int A[]``` is not interpreted as an array but as pointer to integer. Instead of copying the valuee of the variable we are just copying and storing the address of the variable - **call by reference**. It makes sense because array may be very large in size, so it does not make much sense to create a new copy of array ach time it is unnecessarily using a lot of memory. So we always need to pass the **size** of the array.

Arrays are always called by reference:

```C++
void Double(int *A, int size) // int* A or int A[]
{
   int i, sum = 0;
   for(i = 0; i < size; i++)
   {
      A[i] = 2 * A[i];
   }
}

int main()
{
   int A[] = {1, 2, 3, 4, 5};
   int size = sizeof(A)/sizeof(A[0]);
   int i;

   Double(A, size);
   for(i = 0; i < size; i++)
   {
      printf("%d ", A[i]);
   }
   // prints: 2 4 6 8 10

   return 0;
}
```

### 8) Character Arrays and Pointer - Part 1

Character arrays - **strings** - are a group or set of characters, for example: names, phrases, sentences, etc..

#### 1)

How to store strings: the character array must be large enougth to accommodate the string. ```size of array >= number of characters in string + 1```

```C++
char c[8];
c[0] = 'J'; c[1] = 'o'; c[2] = 'h'; c[3] = 'n';
printf("%s\n", C); // John!#@*&|
c[4] = '\0';
printf("%s\n", C); // John
```

| Memory | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
|--------|---|---|---|---|---|---|---|---|
|c| J | o | h | n | trash | trash | trash | trash |

How to know when the string end? We need to put a special character ```\0``` (null character).

| Memory | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
|--------|---|---|---|---|---|---|---|---|
|c| J | o | h | n | \0 | trash | trash | trash |

```C++
#include<string.h>

int main()
{
   char c[] = "John";
   printf("Size in bytes = %d\n", sizeof(c)); // 5
   int len = strlen(c);
   printf("Length = %d\n", len); // 4

   // char C[4] = "JOHN"; ----> compilation error (because of the size)

   char c2[5] = {'J', 'o', 'h', 'n', '\0'}; // other type of initialization

   return 0;
}
```

#### 2)

Arrays and pointers are different types that are used in similar manner.

```C++
char c1[6] = "Hello";
char *c2;
c2 = c1; // poiter c2 now points to the first byte of c1
print("%s\n", c2[1]); // e
c2[0] = 'A'; // c2 can modify c1
```

#### 3)

Array are always passed to function by reference. When we pass array to functions we only pass the base address of the array in a pointer variable, and we do not pass a whole copy of the array.

Because arrays are large in size it is inefficient to create a copy of the same array for each function.

```C++
void print(char *C)
{
   int i = 0;
   while(C[i] != '\0') // we can also do: *(C + i)
   {
      printf("%c ", C[i]);
      i++;
   }
   printf("\n");
}

int main()
{
   char C[20] = "Hello";
   print(C);
}
```

### 9) Character Arrays and Pointer - Part 2

```C++
void print(char *C)
{
   while(*C != '\0')
   {
      printf("%c", *C);
      C++;
   }
   printf("\n");
}

int main()
{
   char C[20] = "Hello";
   // char *C = "Hello"; --->> this string is stored as compile time constant - cannot be modified
   print(C);

   return 0;
}
```

### 10) Pointers and 2-D (multi-dimensional) Arrays

- One-dimensional array
  
```int A[5]```

| n | 200 | 204 | 208 | 212 | 216 |
|---|-----|-----|-----|-----|-----|
| data |  2  |  4  |  6  |  8  | 10 |
|-|A[0]|A[1]|A[2]|A[3]|A[4]|

```C++
int *p = A
printf("%d\n", p); // 200
printf("%d\n", *p); // 2
printf("%d\n", *(p + 2)); // 6
```

- Two-dimensional array

```int B[2][3]```

|n|400|412|
|-|-|-|-|
|data| 1 - 3 - 6 | 2 - 4 - 8 |
|-|B[0]|B[1]|

```C++
int (*p)[3] = B;

// B = &B[0]                          == 400
// *B = B[0] = &B[0][0]               == 1
// B + 1 = &B[1]                      == 412
// *(B + 1) = B[1] = &B[1][0]         == 412
// *(B + 1) + 2 = B[1] + 2 = &B[1][2] == 420
// *(*B + 1)                          == 3
```

### 11) Pointers and Multidimensional Arrays

|n| 400 | 404 | 408 | 412 | 416 | 420 |
|-|-----|-----|-----|-----|-----|-----|
| data | 2 | 3 | 6 | 4 | 5 | 8 |
| - |B[0][0]|B[0][1]|B[0][2]|B[1][0]|B[1][1]|B[1][2]|

```C++
int B[2][3]

// print
// B -> 400
// *B -> 400
// B[0] -> 400 (one dimensional array)
// &B[0][0] -> 400

// B[i][j] = *(B[i] + j) = *(*(B + i) + j)
```

```int C[3][2][2]```

|n|800|816|832|
|-|---|---|---|
|data| 2 - 5 - 7 - 9 | 3 - 4 - 6 - 1 | 0 - 8 - 11 - 13|
|-|C[0]|C[1]|C[2]|

```C++
int C[3][2][2];
int (*p)[2][2] = C;

// print
// C --->> 800
// *C = C[0] = &C[0][0]
// C[i][j][k] = *(cC[i][j] + k) = *(*(C[i] + j) + k) = *(*(*(C + i) + j) + k)
// *(C[0][1] + 1) = C[0][1][1] -->> 9  
// *(C[1] + 1) = C[1][1] = &C[1][1][0] -->> 824
```

```C++++++
// Pointers and multi-dimensional arrays
int main()
{
   int C[3][2][2] = {{{2, 5}, {7, 9}},
                    {{3, 4}, {6, 1}},
                    {{0, 8}, {11, 13}}};
   // print address values
   printf("%d %d %d %d", C, *C, C[0], &C[0][0]); // 4192172 4192172 4192172 4192172

   printf("%d\n", *(C[0][0] + 1)); // 5

   return 0;
}
```

#### Passing multi-dimentional arrays as function arguments

```C++
// Pointers and multi-dimensional arrays

void FuncA(int *A);        // or A[]
void FuncB(int (*B)[3]);   // or A[][3]
void FuncC(int C[][2][2]); // or (*C)[2][2]


int main()
{
   int A[2] = {1, 2};
   FuncA(A); // A returns int*

   int B[2][3] = {{2, 4, 6}, {5, 7, 8}}; // B returns a pointer to array of 3 integers - B return int (*)[3]
   FuncB(B);

   int C[3][2][2] = {{{2, 5}, {7, 9}},
                    {{3, 4}, {6, 1}},
                    {{0, 8}, {11, 13}}};
   FuncC(C);

   return 0;
}
```

### 12) Pointers and Dynamic Memory - Stack vs Heap

Application's memory:

|Memory|
|------|
|Heap|
|Stack|
|Static/Global|
|Code (text)|

#### Code

Part of memory assigned to store the instructions that need to be executed.

#### Static/Global

Store all the static and global variables that are not declared inside a function, and that have the whole lifetime of an application, they are accessible anywhere during the application's life cycle.

#### Stack

Store all information of functions calls and all the local variables.

The amount of memory set aside for these three segments, the text, static/global and stack, does not grow while the application is running.

The allocation and deallocation of memory onto the stack happens by a set rule. When a function is called, it is pushed onto the stack, when it finishes, it is popped out of the stack or removed from the stack. It is not possible to manipulate the scope of the variable if it is on the stack. Another limitation is that if we need to declare a large data type, like an array as local variable, then we need to know the size of the array at compile time.

##### Stack Overflow

The operating system allocates some amount of reserved space for the stack, but the actual allocation of the stack frame and the local variable happens from the stack during runtime and if our call stack grows beyond reserved memory for the stack, then this is called stack overflow and the program will crash. Example: program with a bad recursive function.

#### Heap

Unlike stack, application's heap is not fixed. It's size can vary. The programer can totally control how much memory to use from the heap, till what time to keep the data in the memory during the application's lifetime and heap can grow as long as you do not run out of memory on the system itself.

We can also call heap: free store/pool of memory or **dynamic memory**.

To use dynamic memory in C, we need to know about four functions: **malloc**, **calloc**, **realloc** and **free**. In C++: **new**, **delete**.

Example:

```C++
// C

int main()
{
   // goes on stack
   int a;
   int *p;

   // goes on heap
   p = (int*)malloc(sizeof(int));
   // write 10 on the block of memory in the heap
   *p = 10;
   // deallocate memory
   free(p);

   // large amount of contiguous memory allocated in the heap
   p = (int*)malloc(20*sizeof(int));
   free(p);

   return 0;
}
```

```C++
// C++

int main()
{
   // goes on stack
   int a;
   int *p;
   // goes on heap
   p = new int;
   *p = 10;
   delete p;

   p = new int[20];
   delete[] p;

   return 0;
}
```

### 13) Dynamic memory allocation in C - malloc calloc realloc free

Allocate blocks of memory: ```malloc```, ```calloc``` and ```realloc```.

Deallocate block of memory: ```free```.

#### ```malloc```

```void* malloc(size_t size);```

Malloc returns a void pointer that gives us the address of the first byte in this block allocated.

```C++
int *p = (int*)malloc(3*sizeof(int));

printf("%d ", p); // address of memory

*p = 2;       // first item of the array
*(p + 1) = 4; // second item
*(p + 2) = 6; // third item
p[3] = 8; // same thing
```

All the manipulation on dynamically allocated memory happens through pointers.

#### ```calloc```

It does the same thing as ```calloc```.

```void* calloc(size_t num, size_t size);```

```C++
int *p = (int*)calloc(3, sizeof(int));
```

When malloc allocates some amount of memory, it does not initialize the bytes with any value, so if you do not fill in any value into these addresses allocatedby ```malloc```, you would have some garbage there. But if you allocate memory through ```calloc```, it sets all byte positions to **zero**.

#### ```realloc```

If we have a block of memory, dynamically allocated and we want to change the size of it, then we can use ```realloc```.

```void*realloc(void* Ptr, size_t size);```

#### Example

```C++
int main()
{
   int n;
   printf("Enter size of the array:\n");
   scanf("%d", &n);

   // dynamically allocated array
   int *A = (int*)malloc(n * sizeof(int));
   // int *A = (int*)calloc(n, sizeof(int));

   for(int i = 0; i < n; i++)
   {
      A[i] = i + 1;
   }

   for(int i = 0; i < n; i++)
   {
      printf("%d ", A[i]);
   }

   // create a new block and copies the values of the previous memory block A
   int *B = (int*)realloc(A, 2*n*sizeof(int));

   // equivalent to malloc
   int *B = (int*)realloc(NULL, n * sizeof(int));

   free(A); // same as int *A = (int*)realloc(A, 0);
   free(B);

   return 0;
}
```

### 14) Pointers as function returns in C/C++

A pointer is another data type that stores the address of another data type.

```C++
// call by value
int Add(int a, int b)
{
   int c = a + b;
   return c;
}

int main()
{
   int a = 2, b = 4;
   // call by value
   int c = Add(a, b); // value in a/b are copied to a/b of Add

   return 0;
}
```

```C++
// call by reference
int Add(int* a, int* b)
{
   // a/b are pointers to integers local to Add
   printf("Address of a in Add = %d\n", &a); // 3537372
   printf("Value in a of Add (address of main) = %d\n", a); // 3537612
   printf("Value at address stored in a of Add = %d\n", *a); // 2
   int c = (*a) + (*b);
   return c;
}

int main()
{
   int a = 2, b = 4;
   printf("Address of a in main = %d\n", &a); // 3537612
   // call by reference
   int c = Add(&a, &b); // a/b are local integers

   return 0;
}
```

Now we want to return a pointer to integer from this function.

This code bellow has a problem, try to discover this problem analysing what happens in the stack memory.

```C++
int* Add(int* a, int* b) // returning a pointer to integer
{
   int c = (*a) + (*b);
   return &c; // address
}

int main()
{
   int a = 2, b = 4;
   int* ptr = Add(&a, &b); // ptr points to ? (value can be overwritten)
   printf("Sum = %d\n", *ptr);
}
```

So what are the use cases in which we may want to return pointers from functions? Usually if we have address of some memory block in the heap section or in the global section, then we can safely return the address of these blocks, because anything has to be explicitly de-allocated. And anything which is in the glocal section lives for the entire lifetime.

```C++
int *Add(int* a, int* b)
{
   int* c = (int*)malloc(sizeof(int)); // allocate memory space in the heap
   *c = (*a) + (*b);
   return c;
}

int main()
{
   int a = 2, b = 4;
   int* ptr = Add(&a, &b);
   printf("Sum = %d\n", *ptr);

   return 0;
}
```

### 15) Function Pointers in C / C++

Function pointers are used to store address of functions.

```C++
int Add(int a, int b)
{
   return a + b;
}

int main()
{
   // pointer to function that should take
   // (int, int) as argument/parameter and return int
   int (*p)(int, int);
   p = &Add; // p now points to add
   // now with p we can execut Add

   int c;
   c = (*p)(2, 3); // de-referencing and executing the function
   printf("%d", c);

   return 0;
}
```

```C++
void PrintHello()
{
   printf("Hello\n");
}

void PrintHello2(char* name)
{
   printf("Hello %s\n", name);
}

int main()
{
   void (*ptr)();
   ptr = PrintHello;
   ptr(); // executes function

   void (*ptr2)(char*);
   ptr = PrintHello2;
   ptr("Virginia");

   return 0;
}
```

### 16) Function pointers and callbacks

Function pointers: can be passed as arguments to functions.

```C++
void A()
{
   printf("Hello");
}

void B(void (*ptr)()) // function pointer as argument
{
   ptr(); // call-back function that "ptr" points to
}

int main()
{
   // p is a function pointer - takes no arguments and returns void
   void (*p)() = A; // initialize p as address of A
   B(p);

   // or we can simply:
   B(A);

   return 0;
}
```

```C++
int compare(int a, int b)
{
   if(a > b) return 1; // 1 2 3 4 5 6
   else  return -1;
}

int absolute_compare(int a, int b) // compare negative values
{
   if(abs(a) > abs(b)) return 1;
   return -1;
}

void BubbleSort(int *A, int n, int (*compare)(int, int))
{
   int i, j, temp;
   for(i = 0; i < n; i++)
   {
      for(j = 0; j < n - 1; j++)
      {
         if(compare(A[j] > A[j + 1]) > 0) // swap
         {
            temp = A[j];
            A[j] = A[j + 1];
            A[j + 1] = temp;
         }
      }
   }
}

int main()
{
   int i, A[] = {3, 2, 1, 5, 6, 4};
   BubbleSort(A, 6, compare);
   for(i = 0; i < 6; i++)
      printf("%d ", A[i]);
}
```

```C++
/*
void pointers are generic pointers
we can typecast them to a pointer of any data type  */
int compare(const void* a, const void* b)
{
   // typecast the void* to int*
   // the first * is to get the value
   int A = *((int*)a);
   int B = *((int*)b);

   return abs(A) - abs(B);
}

int main()
{
   int i, A[] = {-31, 22, -1, 50, -6, 4}; // => {-1, 4, -6, 22, -31, 50}
   qsort(A, 6, sizeof(int), compare);
   for(i = 0; i < 6; i++) printf("%d ", A[i]);
}
```

### 17) Memory leak in C/C++

Memory leak - our application is holding on to some unused memory on the heap; improper use of dynamic memory.

(example of a game using malloc to allocate memory on the heap, and wihout free function, the memory consumption of the program continues to grow - we can see this opening the windows task manager, for example)