# 第 10 章 《C++ Primer Plus中文版 (第六版)》 編程練習題之我解

## 10.2

**題：** 
Here is a rather simple class definition:

    class Person {
        private:
        static const LIMIT = 25;
        string lname; // Person’s last name
        char fname[LIMIT]; // Person’s first name
        public:
        Person() {lname = ""; fname[0] = ‘\0’; } // #1
        Person(const string & ln, const char * fn = "Heyyou"); // #2
        // the following methods display lname and fname
        void Show() const; // firstname lastname format
        void FormalShow() const; // lastname, firstname format
    };

(It uses both a string object and a character array so that you can compare
how the two forms are used.) Write a program that completes the
implementation by providing code for the undefined methods. The program in
which you use the class should also use the three possible constructor calls
(no arguments,one argument, and two arguments) and the two display methods.
Here’s an example that uses the constructors and methods:

    Person one; // use default constructor
    Person two("Smythecraft"); // use #2 with one default argument
    Person three("Dimwiddy", "Sam"); // use #2, no defaults
    one.Show();
    cout << endl;
    one.FormalShow();
    // etc. for two and three

**解：**

```Cpp
#include <string>
#include <iostream>
#include <cstring>

class Person
{
    private:
        static const int LIMIT = 25;
        std::string lname;
        char fname[LIMIT];
    public:
        Person(){lname = ""; fname[0] = '\0';}
        Person(const std::string &ln, const char *fn = "Heyyou");
        void Show() const;
        void FormalShow() const;
};

Person::Person(const std::string &ln, const char *fn)
{
    lname = ln;
    strncpy(fname, fn, LIMIT-1);
    fname[LIMIT-1] = '\0';
}
void Person::Show() const
{
    std::cout << "Name: " << lname << "\nfname: " << fname << std::endl;
}
void Person::FormalShow() const
{
    std::cout << "Formal name: " << fname << " " << lname << std::endl;
}

int main(void)
{
    Person one;
    Person two("Smythecraft");
    Person three("Dimwiddy", "Sam");
    one.Show();
    one.FormalShow();
    std::cout << std::endl;
    two.Show();
    two.FormalShow();
    std::cout << std::endl;
    three.Show();
    three.FormalShow();
    return 0;
}
```

## 10.3

**題：** 
Do Programming Exercise 1 from Chapter 9 but replace the code shown there with an appropriate golf class declaration. Replace setgolf(golf &, const char*, int) with a constructor with the appropriate argument for providing initial values. Retain the interactive version of setgolf() but implement it by using the constructor. (For example,for the code for setgolf(), obtain the data,pass the data to the constructor to create a temporary object, and assign the temporary object to the invoking object, which is *this.)
**解：**

```Cpp
#include <iostream>
#include <cstring>

class Golf
{
    private:
        static const int Len = 40;
        char fullname[Len];
        int handicap;
    public:
        Golf(const char *name = "", int hc = 0);
        int setgolf();
        void sethandicap(int hc);
        void showgolf() const;
};

Golf::Golf(const char *name, int hc)
{
    strncpy(fullname, name, Len-1);
    fullname[Len-1] = '\0';
    handicap = hc;
}
int Golf::setgolf()
{
    char str[Len];
    int hand;
    std::cout << "Enter full name: ";
    std::cin.get(str, Len);
    if(str[0] == '\0')
    {
        std::cin.clear();
        return 0;
    }
    while(std::cin.get() != '\n')
        continue;
    std::cout << "Enter handicap: ";
    (std::cin >> hand).get();
    Golf tmp {str, hand};
    *this = tmp;
    return 1;
}
void Golf::sethandicap(int hc)
{
    handicap = hc;
}
void Golf::showgolf() const
{
    std::cout << "Name: " << fullname
              << "\nhandicap: " << handicap << std::endl;
}

const int SIZE = 5;
int main(void)
{
    Golf ann {"Ann Birdfree", 24};
    ann.showgolf();
    ann.sethandicap(30);
    ann.showgolf();
    Golf arr[SIZE];
    int player = 0;
    while(player < SIZE && arr[player].setgolf())
        player++;
    for(int i = 0; i < player; i++)
        arr[i].showgolf();
    return 0;
}
```

## 10.4

**題：** 
Do Programming Exercise 4 from Chapter 9 but convert the Sales structure and its associated functions to a class and its methods. Replace the setSales(Sales &, double[], int) function with a constructor. Implement the interactive setSales(Sales &) method by using the constructor. Keep the class within the namespace SALES.

**解：**

```Cpp
#include<iostream>

namespace SALES
{
    class Sales
    {
    private:
       enum { QUARTERS = 4 };
       double sales[QUARTERS]{};
       double average;
       double s_max;
       double s_min;
    public:
        Sales(){ s_max = s_min = average = 0; }
        Sales(const double ar[], int n);
        void setSales();
        void showSales() const;
    };

    Sales::Sales(const double ar[], int n)
    {
        double max, min, total = .0;
        for(int i = 0; i < QUARTERS; i++)
        {
            if(n > i) sales[i] = ar[i];
            else sales[i] = .0;
            total += sales[i];
        }
        max = min = sales[0];
        for(int i = 0; i < n && i < QUARTERS; i++)
        {
            if(max < sales[i]) max = sales[i];
            if(min > sales[i]) min = sales[i];
        }
        s_max = max;
        s_min = min;
        average = (n < QUARTERS) ? total/n : total/QUARTERS;
    }
    void Sales::setSales()
    {
        double arr[QUARTERS];
        for(int i = 0; i < QUARTERS; i++)
        {
            std::cout << "Enter sales #" << i+1 << ": ";
            std::cin >> arr[i];
        }
        Sales tmp(arr, QUARTERS);
        *this = tmp;
    }
    void Sales::showSales() const
    {
        for(int i = 0; i < QUARTERS; i++)
            std::cout << "sales #" << i+1 << ": " << sales[i] << std::endl;
        std::cout << "average: " << average << "\n"
                  << "max:     " << s_max << "\n"
                  << "min:     " << s_min << std::endl;
    }
}  // namespace SALES

int main(void)
{
    SALES::Sales sal1;
    sal1.setSales();
    sal1.showSales();
    double ar[3] = {120.2, 302.5, 177.3};
    SALES::Sales sal2(ar, 3);
    sal2.showSales();
    return 0;
}
```

## 10.5

**題：** 
Consider the following structure declaration:

    struct customer {
        char fullname[35];
        double payment;
    };

Write a program that adds and removes customer structures from a stack, represented by a Stack class declaration. Each time a customer is removed, his or her payment should be added to a running total, and the running total should be reported. Note: You should be able to use the Stack class unaltered; just change the typedef declaration so that Item is type customer instead of unsigned long.
**解：**

```Cpp
struct customer {
    char fullname[35];
    double payment;
};

typedef customer Item;
class Stack
{
    private:
        enum {MAX = 10};
        Item items[MAX]{};
        int top;
    public:
        Stack() { top = 0; }
        bool isempty() const;
        bool isfull() const;
        bool push(const Item & item);
        bool pop(Item & item);
};

bool Stack::isempty() const
{
    return top == 0;
}
bool Stack::isfull() const
{
    return top == MAX;
}
bool Stack::push(const Item & item)
{
    if(top < MAX)
    {
        items[top++] = item;
        return true;
    } else
        return false;
}
bool Stack::pop(Item & item)
{
    if(top > 0)
    {
        item = items[--top];
        return true;
    } else
        return false;
}

#include <cctype>
#include <iostream>

int main(void)
{
    Stack st;
    char ch;
    double total = 0;
    customer po;
    std::cout << "Pleace enter A to add customer, \n"
         << "P to process a PO, or Q to quit.\n";
    while(std::cin >> ch && toupper(ch) != 'Q')
    {
        while(std::cin.get() != '\n')
            continue;
        if(!isalpha(ch))
        {
            std::cout << '\a';
            continue;
        }
        switch(ch)
        {
            case 'A':
            case 'a': std::cout << "Enter full name: ";
                      std::cin.get(po.fullname, 35);
                      while(std::cin.get() != '\n')
                          continue;
                      std::cout << "Enter payment amount: ";
                      std::cin >> po.payment;
                      if(st.isfull())
                          std::cout << "stack already full\n";
                      else
                          st.push(po);
                      break;
            case 'P':
            case 'p': if(st.isempty())
                          std::cout << "stack already empty\n";
                      else {
                          st.pop(po);
                          std::cout << "PO #" << (total += po.payment) << " popped\n";
                      }
                      break;
        }
        std::cout << "Please enter A to add customer,\n"
             << "P to process a PO, or Q to quit.\n";
    }
    std::cout << "Bye\n";
    return 0;
}
```

## 10.6

**題：** 

**解：**

```Cpp

```

## 10.7

**題：** 

**解：**

```Cpp

```

## 10.8

**題：** 

**解：**

```Cpp

```