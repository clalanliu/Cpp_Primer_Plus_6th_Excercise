# 第 8 章 《C++ Primer Plus中文版 (第六版)》 編程練習題之我解

## 8.1

**題：** 
Write a function that normally takes one argument, the address of a string, and prints that string once. However, if a second, type int, argument is provided and is nonzero, the function should print the string a number of times equal to the number of times that function has been called at that point. (Note that the number of times the string is printed is not equal to the value of the second argument; it is equal to the number of times the function has been called.) Yes, this is a silly function, but it makes you use some of the techniques discussed in this chapter. Use the function in a simple program that demonstrates how the function works. Hint: use *static*

**解：**

```Cpp
#include <iostream>
void show(const char*, int n = 0);
int main(void)
{
    char str[20] = "Hello, World!";
    show(str);
    show(str);
    show(str, 1);
    return 0;
}
void show(const char *str, int n)
{
    static int cnt = 0;
    if(!n) std::cout << str << "\n";
    else
        for(int i = 0; i < cnt; i++)
            std::cout << str << "\n";
    cnt++;
}

```

## 8.2

**題：** 
The CandyBar structure contains three members. The first member holds the brand name of a candy bar.The second member holds the weight (which may have a fractional part) of the candy bar, and the third member holds the number of calories (an integer value) in the candy bar. Write a program that uses a function that takes as arguments a reference to CandyBar, a pointer-to-char, a double, and an int and uses the last three values to set the corresponding members of the structure. The last three arguments should have default values of “Millennium Munch,” 2.85, and 350. Also the program should use a function that takes a reference to a CandyBar as an argument and displays the contents of the structure. Use const where appropriate.

**解：**

```Cpp
#include <iostream>
#include <cstring>
const int SIZE = 20;
struct CandyBar
{
    char name[SIZE];
    double weight;
    int cal;
};
void fill
    (CandyBar &, const char *str="Millennium Munch", double w=2.85, int c=350);
void display(const CandyBar &);
int main(void)
{
    CandyBar test;
    fill(test);
    display(test);
    fill(test, "Gold Star", 3.15, 410);
    display(test);
    return 0;
}
void fill(CandyBar &candy, const char *str, double w, int c)
{
    strcpy(candy.name, str);
    candy.weight = w;
    candy.cal = c;
}
void display(const CandyBar &candy)
{
    std::cout << "\nName:     " << candy.name
              << "\nWeight:   " << candy.weight
              << "\nCalories: " << candy.cal << std::endl;
}

```

## 8.3

**題：** 
Write a function that takes a reference to a string object as its parameter and that converts the contents of the string to uppercase. Use the toupper() function described in Table 6.4 of Chapter 6.Write a program that uses a loop which allows you to test the function with different input. A sample run might look like this:

    Enter a string (q to quit): go away
    GO AWAY
    Next string (q to quit): good grief!
    GOOD GRIEF!
    Next string (q to quit): q
    Bye.

**解：**
```Cpp
#include <iostream>
#include <string>
using namespace std;
void strUpp(string &);
int main(void)
{
    string str;
    cout << "Enter a string (q to quit): ";
    getline(cin, str);
    while(str != "q")
    {
        strUpp(str);
        cout << str << endl;
        cout << "Next string (q to quit): ";
        getline(cin, str);
    }
    cout << "Bye.\n";
    return 0;
}
void strUpp(string &str)
{
    for(int i = 0; i < str.length(); i++)
        str[i] = toupper(str[i]);
}
```

## 8.4

**題：** 
The following is a program skeleton:

    #include <iostream>
    using namespace std;
    #include <cstring>    // for strlen(), strcpy()
    struct stringy {
        char * str;       // points to a string
        int ct;           // length of string (not counting '\0')
    };
    // prototypes for set(), show(), and show() go here
    int main()
    {
        stringy beany;
        char testing[] = "Reality isn't what it used to be.";
        set(beany, testing); // first argument is a reference,
                             // allocates space to hold copy of testing,
                             // sets str member of beany to point to the
                             // new block, copies testing to new block,
                             // and sets ct member of beany
        show(beany);      // prints member string once
        show(beany, 2);   // prints member string twice
        testing[0] = 'D';
        testing[1] = 'u';
        show(testing);    // prints testing string once
        show(testing, 3); // prints testing string thrice
        show("Done!");
        return 0;
    }

Complete this skeleton by providing the described functions and prototypes. Note that there should be two show() functions,each using default arguments. Use const arguments when appropriate. Note that set() should use new to
allocate sufficient space to hold the designated string.The techniques used here are similar to those used in designing and implementing classes. (You might have to alter the header filenames and delete the using directive, depending on your compiler.)

**解：**
```Cpp
#include <iostream>
#include <cstring>
struct stringy{
    char *str;
    int ct;
};
using namespace std;
void set(stringy &, const char*);
void show(const char*, int n = 1);
void show(const stringy &, int n = 1);
int main(void)
{
    stringy beany;
    char testing[] = "Reality isn't what it used to be.";
    set(beany, testing);    // first argument is a reference,
                            // allocates space to hold copy of testing,
                            // sets str member of beany to point to the
                            // new block, copies testing to new block,
                            // and sets ct member of beany
    show(beany);      // prints member string once
    show(beany, 2);   // prints member string twice
    testing[0] = 'D';
    testing[1] = 'u';
    show(testing);    // prints testing string once
    show(testing, 3); // prints testing string thrice
    show("Done!");
    return 0;
}

void set(stringy &ps, const char *ch)
{
    int len = strlen(ch);
    char *str = new char[len+1];
    ps.str = str;
    strcpy(ps.str, ch);
    ps.ct = len;
}

void show(const stringy &ps, int n)
{
    for(int i = 0; i < n; i++)
        cout << ps.str << endl;
}
void show(const char *pc, int n)
{
    for(int i = 0; i < n; i++)
        cout << pc << endl;
}
```

## 8.5

**題：** 
Write a template function max5() that takes as its argument an array of five items of type T and returns the largest item in the array. (Because the size is fixed, it can be hard-coded into the loop instead of being passed as an argument.) Test it in a program that uses the function with an array of five int value and an array of five double values.

**解：**

```Cpp
#include <iostream>
template <typename T> T max5(T ar[]);
int main(void)
{
    int ar1[] {1, 7, 5, 11, 9};
    double ar2[] {1.3, 7.5, 9.5, 5.1, 9.3};
    std::cout << "ar1: " << max5(ar1) << "\nar2: " << max5(ar2) << std::endl;
    return 0;
}
template <typename T> T max5(T ar[])
{
    T max = ar[0];
    for(int i = 0; i < 5; i++)
        if(max < ar[i]) max = ar[i];
    return max;
}
```

## 8.6

**題：** 
Write a template function maxn() that takes as its arguments an array of items of type T and an integer representing the number of elements in the array and that returns the largest item in the array. Test it in a program that uses the function template with an array of six int value and an array of four double values. The program should also include a specialization that takes an array of pointers-to-char as an argument and the number of pointers as a second argument and that returns the address of the longest string. If multiple strings are tied for having the longest length, the function should return the address of the first one tied for longest. Test the specialization with an array of five string pointers.

**解：**

```Cpp
#include <iostream>
#include <cstring>
template <typename T> T maxn(T ar[], int);
template <> const char* maxn(const char* ar[], int);
int main(void)
{
    int ar1[6] = {1, 7, 5, 11, 9, 3};
    double ar2[4] = {1.3, 7.5, 9.5, 5.1};
    const char* ar3[5] = {"Fiat", "Honda", "Ferrari", "Volvo", "Bugatti"};
    std::cout << "ar1: "   << maxn(ar1, 6)
              << "\nar2: " << maxn(ar2, 4)
              << "\nar3: " << maxn(ar3, 5) << std::endl;
    return 0;
}
template <typename T> T maxn(T ar[], int n)
{
    T max = ar[0];
    for(int i = 0; i < n; i++)
        if(max < ar[i]) max = ar[i];
    return max;
}
template <> const char* maxn(const char* ar[], int n)
{
    const char* str = ar[0];
    for(int i = 0; i < n; i++)
        if(strlen(str) < strlen(ar[i])) str = ar[i];
    return str;
}
```

## 8.7

**題：** 
Modify Listing 8.14 so that it uses two template functions called SumArray() to return the sum of the array contents instead of displaying the contents. The program now should report the total number of things and the sum of all the debts.

**解：**

```Cpp
#include <iostream>
template <typename T> T SumArray(T arr[], int);
template <typename T> T SumArray(T *arr[], int);
struct debts
{
    char name[50];
    double amount;
};
int main(void)
{
    int things[6] = {13, 31, 103, 301, 310, 130};
    struct debts mr_E[3] =
    {
        {"Ima Wolfe", 2400.0},
        {"Ura Foxe", 1300.0},
        {"Iby Stout", 1800.0}
    };
    double *pd[3];
    for(int i = 0; i < 3; i++)
        pd[i] = &mr_E[i].amount;
    std::cout << "Total number of things:\n" << SumArray(things, 6) << "\n";
    std::cout << "Total sum of all debts:\n" << SumArray(pd, 3) << "\n";
    return 0;
}
template <typename T> T SumArray(T arr[], int n)
{
    T sum = 0;
    for(int i = 0; i < n; i++)
        sum += arr[i];
    return sum;
}
template <typename T> T SumArray(T *arr[], int n)
{
    T sum = 0;
    for(int i = 0; i < n; i++)
        sum += *arr[i];
    return sum;
}
```
