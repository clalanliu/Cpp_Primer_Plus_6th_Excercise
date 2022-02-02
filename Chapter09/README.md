# 第 9 章 《C++ Primer Plus中文版 (第六版)》 編程練習題之我解

## 9.1

**題：** 
Here is a header file:

// golf.h -- for pe9-1.cpp

const int Len = 40;
struct golf
{
    char fullname[Len];
    int handicap;
};

// non-interactive version:
// function sets golf structure to provided name, handicap
// using values passed as arguments to the function
void setgolf(golf & g, const char * name, int hc);

// interactive version:
// function solicits name and handicap from user
// and sets the members of g to the values entered
// returns 1 if name is entered, 0 if name is empty string
int setgolf(golf & g);

// function resets handicap to new value
void handicap(golf & g, int hc);

// function displays contents of golf structure
void showgolf(const golf & g);

Note that setgolf() is overloaded. Using the first version of setgolf()
would look like this:

golf ann;
setgolf(ann, "Ann Birdfree", 24);

The function call provides the information that’s stored in the ann
structure. Using the second version of setgolf() would look like this:

golf andy;
setgolf(andy);

The function would prompt the user to enter the name and handicap and store
them in the andy structure.This function could (but doesn’t need to) use the
first version internally.

Put together a multifile program based on this header. One file, named
golf.cpp, should provide suitable function definitions to match the
prototypes in the header file. A second file should contain main() and
demonstrate all the features of the prototyped functions. For example, a
loop should solicit input for an array of golf structures and terminate when
the array is full or the user enters an empty string for the golfer’s name.
The main() function should use only the prototyped functions to access the
golf structures.

**解：**

```Cpp


```

## 9.2

**題：** 
Redo Listing 9.9, replacing the character array with a string object. The program should no longer have to check whether the input string fits, and it can compare the input string to "" to check for an empty line.

**解：**

```Cpp
//  golf.cpp --for sol-9-01.cpp

#include "golf.h"
#include <iostream>
#include <cstring>

void setgolf(golf &g, const char *name, int hc)
{
    strcpy(g.fullname, name);
    g.handicap = hc;
}
int setgolf(golf &g)
{
    std::cout << "Enter full name: ";
    char tmp[Len];
    std::cin.get(tmp, Len);
    if(tmp[0] == '\0')
    {
        std::cin.clear();
        return 0;
    }
    while(std::cin.get() != '\n')
        continue;
    strcpy(g.fullname, tmp);
    std::cout << "Enter handicap: ";
    (std::cin >> g.handicap).get();
    return 1;
}
void handicap(golf &g, int hc)
{
    g.handicap = hc;
}
void showgolf(const golf &g)
{
    std::cout << "Name: " << g.fullname
              << "\nhandicap: " << g.handicap << std::endl;
}
```
```Cpp
//  golf.h --for sol-9-01.cpp

#ifndef GOLF_H_
#define GOLF_H_

const int Len = 40;
struct golf
{
    char fullname[Len];
    int handicap;
};
void setgolf(golf &g, const char *name, int hc);
int setgolf(golf &g);
void handicap(golf &g, int hc);
void showgolf(const golf &g);

#endif
```
```Cpp
#include "golf.h"

const int SIZE = 5;
int main()
{
    golf ann;
    setgolf(ann, "Ann Birdfree", 24);
    showgolf(ann);
    handicap(ann, 30);
    showgolf(ann);
    golf arr[SIZE];
    int player = 0;
    while(player < SIZE && setgolf(arr[player]))
        player++;
    for(int i = 0; i < player; i++)
        showgolf(arr[i]);
    return 0;
}
```
## 9.3

**題：** 
Begin with the following structure declaration:

    struct chaff
    {
        char dross[20];
        int slag;
    };

Write a program that uses placement new to place an array of two such
structures in a buffer. Then assign values to the structure members
(remembering to use strcpy() for the char array) and use a loop to display
the contents. Option 1 is to use a static array, like that in Listing 9.10,
for the buffer. Option 2 is to use regular new to allocate the buffer.

**解：**

```Cpp
#include <iostream>
#include <new>
#include <cstring>
struct chaff
{
    char dross[20];
    int slag;
};
char buffer[2*sizeof(chaff)];

int main(void)
{
    std::cout << &buffer << std::endl;
    
    chaff* ps1 = new (buffer) chaff[2];
    std::cout << ps1 << std::endl;
    strcpy(ps1[0].dross, "option 1");
    strcpy(ps1[1].dross, "option 1");
    ps1[0].slag = 1;
    ps1[1].slag = 2;
    
    chaff* ps2 = new chaff[2];
    std::cout << ps2 << std::endl;
    strcpy(ps2[0].dross, "option 2");
    strcpy(ps2[1].dross, "option 2");
    ps2[0].slag = 1;
    ps2[1].slag = 2;
   
    for(int i = 0; i < 2; i++)
        std::cout << "dross: "   << ps1[i].dross << " slag: " << ps1[i].slag
                  << "\ndross: " << ps2[i].dross << " slag: " << ps2[i].slag
                  << std::endl;
    delete []ps2;
   
    return 0;
}
```

## 9.4

**題：** 
Write a three-file program based on the following namespace:

    namespace SALES
    {
        const int QUARTERS = 4;
        struct Sales
        {
            double sales[QUARTERS];
            double average;
            double max;
            double min;
        };
        // copies the lesser of 4 or n items from the array ar
        // to the sales member of s and computes and stores the
        // average, maximum, and minimum values of the entered items;
        // remaining elements of sales, if any, set to 0
        void setSales(Sales & s, const double ar[], int n);
        // gathers sales for 4 quarters interactively, stores them
        // in the sales member of s and computes and stores the
        // average, maximum, and minimum values
        void setSales(Sales & s);
        // display all information in structure s
        void showSales(const Sales & s);
    }

The first file should be a header file that contains the namespace. The
second file should be a source code file that extends the namespace to
provide definitions for the three prototyped functions. The third file
should declare two Sales objects. It should use the interactive version of
setSales() to provide values for one structure and the non-interactive
version of setSales() to provide values for the second structure. It should
display the contents of both structures by using showSales().

**解：**

```Cpp
//  sales.cpp --for sol-9-04.cpp

#include "sales.h"
#include <iostream>

namespace SALES
{
    void setSales(Sales & s, const double ar[], int n)
    {
        double max, min, total = .0;
        for(int i = 0; i < QUARTERS; i++)
        {
            if(n > i) s.sales[i] = ar[i];
            else s.sales[i] = .0;
            total += s.sales[i];
        }
        max = min = s.sales[0];
        for(int i = 0; i < n && i < QUARTERS; i++)
        {
            if(max < s.sales[i]) max = s.sales[i];
            if(min > s.sales[i]) min = s.sales[i];
        }
        s.max = max;
        s.min = min;
        s.average = (n < QUARTERS) ? total/n : total/QUARTERS;
    }
    void setSales(Sales & s)
    {
        double max, min, total = .0;
        for(int i = 0; i < QUARTERS; i++)
        {
            std::cout << "Enter sales #" << i+1 << ": ";
            std::cin >> s.sales[i];
            total += s.sales[i];
        }
        max = min = s.sales[0];
        for(int i = 0; i < QUARTERS; i++)
        {
            if(max < s.sales[i]) max = s.sales[i];
            if(min > s.sales[i]) min = s.sales[i];
        }
        s.average = total/QUARTERS;
        s.max = max;
        s.min = min;
    }
    void showSales(const Sales & s)
    {
        for(int i = 0; i < QUARTERS; i++)
            std::cout << "sales #" << i+1 << ": " << s.sales[i] << std::endl;
        std::cout << "average: " << s.average << "\n"
                  << "max:     " << s.max << "\n"
                  << "min:     " << s.min << std::endl;
    }
}   // namespace SALES

```
```Cpp
//  sales.h --for sol-9-04.cpp

#ifndef SALES_H_
#define SALES_H_

namespace SALES
{
   const int QUARTERS = 4;
   struct Sales
   {
       double sales[QUARTERS];
       double average;
       double max;
       double min;
   };
   void setSales(Sales & s, const double ar[], int n);
   void setSales(Sales & s);
   void showSales(const Sales & s);

}  // namespace SALES

#endif  // SALES_H_

```
```Cpp
#include "sales.h"

int main(void)
{
    SALES::Sales sal1;
    SALES::setSales(sal1);
    SALES::showSales(sal1);
    double ar[3] = {120.2, 302.5, 177.3};
    SALES::Sales sal2;
    SALES::setSales(sal2, ar, 3);
    SALES::showSales(sal2);
    return 0;
}

```