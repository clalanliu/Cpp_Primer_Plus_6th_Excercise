# 第 7 章 《C++ Primer Plus中文版 (第六版)》 編程練習題之我解

## 7.1

**題：** Write a program that repeatedly asks the user to enter pairs of numbers until at least one of the pair is 0. For each pair, the program should use a function to calculate the harmonic mean of the numbers. The function should return the answer to main(), which should report the result. The harmonic mean of the numbers is the inverse of the average of the inverses and can be calculated as follows: harmonic mean = 2.0 × x × y / (x + y)


**解：**

```Cpp
#include <iostream>
double harmonic(double x, double y) { 
    return (2.0*x*y)/(x+y); 
}
int main(){
    std::cout << "Enter pairs of numbers: (0 for terminate):\n";
    double x, y;
    while (std::cin >> x && x != 0 && std::cin >> y && y != 0)
        std::cout << "Harmonic (" << x << ", " << y << ") = " << harmonic(x, y)
                  << "\nEnter pair of numbers:\n";
    return 0;
}
```

## 7.2
**題：** 
Write a program that asks the user to enter up to 10 golf scores, which are to be stored in an array. You should provide a means for the user to terminate input prior to entering 10 scores. The program should display all the scores on one line and report the average score. Handle input, display, and the average calculation with three separate array-processing functions.
**解：** 
```Cpp
#include <iostream>
const int SIZE = 10;
int fill_ar(int ar[], int limit);
void avrage(int ar[], int n);
void show_ar(const int ar[], int n);
int main()
{
    int result[SIZE];
    int size = fill_ar(result, SIZE);
    show_ar(result, size);
    avrage(result, size);
    return 0;
}
int fill_ar(int ar[], int limit)
{
    int i, temp;
    std::cout << "Enter up to 10 golf scores (negative value to loop quit):\n";
    for(i = 0; i < limit; i++)
    {
        std::cout << "Value #" << (i + 1) << ": ";
        std::cin >> temp;
        if(!std::cin)
        {
            std::cin.clear();
            while(std::cin.get() != '\n')
                continue;
            std::cout << "Bad input; input process terminated.\n";
            break;
        } else if (temp < 0)
            break;
        ar[i] = temp;
    }
    return i;
}
void avrage(int ar[], int n)
{
    double temp = 0.0;
    for(int i = 0; i < n; i++)
        temp += ar[i];
    if(n) std::cout << "Avrage score: " << temp/n << std::endl;
}
void show_ar(const int ar[], int n)
{
    for(int i = 0; i < n; i++)
        std::cout << "Property #" << (i + 1) << ": " << ar[i] << std::endl;
}
```

## 7.3
**題：** 
Here is a structure declaration:

    struct box
    {
        char maker[40];
        float height;
        float width;
        float length;
        float volume;
    };

    a.  Write a function that passes a box structure by value and that displays
        the value of each member.
    b.  Write a function that passes the address of a box structure and that
        sets the volume member to the product of the other three dimensions.
    c.  Write a simple program that uses these two functions.
**解：** 
```Cpp
#include <iostream>
using namespace std;
struct box
{
    char maker[40];
    float height;
    float width;
    float length;
    float volume;
};

ostream& operator<<(ostream& os, const box& b)
{
    os << b.maker<< b.height << '/' << b.width << '/' << b.length << '/' << b.volume;
    return os;
}

void set_volumn(box* b){
    b->volume =  b->height*b->width*b->length;
}

int main(){
    box cube = {"Cube", 5, 4, 10, 0};
    cout << cube << endl;
    set_volumn(&cube);
    cout << cube << endl;
}
```

## 7.4
**題：** 
Many state lotteries use a variation of the simple lottery portrayed by Listing 7.4. In these variations you choose several numbers from one set and  call them the field numbers. For example, you might select five numbers from the field of 1–47). You also pick a single number (called a mega number or a power ball, etc.) from a second range, such as 1–27. To win the grand prize, you have to guess all the picks correctly. The chance of winning is the product of the probability of picking all the field numbers times the probability of picking the mega number. For instance, the probability of winning the example described here is the product of the probability of picking 5 out of 47 correctly times the probability of picking 1 out of 27 correctly. Modify Listing 7.4 to calculate the probability of winning this kind of lottery.
**解：** 
```Cpp
#include <iostream>
long double probability(unsigned numbers, unsigned picks);
int main()
{
    using namespace std;
    double total, choices, meganum, limit;
    cout << "Enter the total number of choices on the game card and\n"
         << "the number of picks allowed:\n";
    while ((cin >> total >> choices) && choices <= total)
    {
        cout << "Enter the total number of choices on the game card and\n"
             << "the number of picks allowed for the Mega Number:\n";
        if((cin >> limit >> meganum) && meganum <= limit);
        {
            cout << "You have one chance in ";
            cout << probability(total, choices) * probability(limit, meganum);
            cout << " of winning. \n";
        }
        cout << "Next two numbers(q to quit): ";
    }
    cout << "bye\n";
    return 0;
}
long double probability(unsigned numbers, unsigned picks)
{
    long double result = 1.0;
    long double n;
    unsigned p;
    for (n = numbers, p = picks; p > 0; n--, p--)
        result = result * n / p;
    return result;
}
```

## 7.5
**題：** 
Define a recursive function that takes an integer argument and returns the factorial of that argument. Recall that 3 factorial, written 3!, equals 3 × 2!, and so on, with 0! defined as 1. In general, if n is greater than zero, n! = n * (n - 1)!. Test your function in a program that uses a loop to allow the user to enter various values for which the program reports the factorial.
**解：** 
```Cpp
#include <iostream>
unsigned long int factor(int n);
int main()
{
    std::cout << "Enter non-negative value (q to quit): ";
    int val;
    while (std::cin >> val && val >= 0)
        std::cout << val << "! " << factor(val) << "\nNext value: ";
    return 0;
}
unsigned long int factor(int n)
{
    if (n == 0)
        return 1;
    else
        return n*factor(n - 1);
}
```

## 7.6
**題：** 
Write a program that uses the following functions:

    Fill_array() takes as arguments the name of an array of double values and an array size. It prompts the user to enter double values to be entered in the array. It ceases taking input when the array is full or when the user enters non-numeric input, and it returns the actual number of entries.

    Show_array() takes as arguments the name of an array of double values and an array size and displays the contents of the array.

    Reverse_array() takes as arguments the name of an array of double values and an array size and reverses the order of the values stored in the array.

    The program should use these functions to fill an array, show the array, reverse the array, show the array, reverse all but the first and last elements of the array, and then show the array.
**解：** 
```Cpp
#include <iostream>

const int SIZE = 10;
int Fill_array(double*, int);
void Reverse_array(double*, int);
void Show_array(const double*, int);

int main()
{
    double arr[SIZE];
    int size = Fill_array(arr, SIZE);
    Show_array(arr, size);
    Reverse_array(arr, size);
    Show_array(arr, size);
    return 0;
}
int Fill_array(double *ar, int n)
{
    int i;
    double temp;
    std::cout << "Enter up to " << n << " values (q to quit):\n";
    for(i = 0; i < n; i++)
    {
        std::cout << "Value #" << (i + 1) << ": ";
        std::cin >> temp;
        if(!std::cin) break;
        ar[i] = temp;
    }
    return i;
}
void Reverse_array(double *ar, int n)
{   
    double* i;
    double* j;
    for(i = ar, j = ar + n - 1; i < j; i++, j--)
    {
        double temp = *i;
        *i = *j;
        *j = temp;
    }
}
void Show_array(const double *ar, int n)
{
    std::cout << "Array: ";
    for(int i = 0; i < n; i++)
        std::cout << ar[i] << " ";
    std::cout << std::endl;
}
```

## 7.7
**題：** 
Redo Listing 7.7, modifying the three array-handling functions to each use two pointer parameters to represent a range.The fill_array() function, instead of returning the actual number of items read, should return a pointer to the location after the last location filled; the other functions can use this pointer as the second argument to identify the end of the data.
**解：** 
```Cpp
#include <iostream>
const int Max = 5;
double *fill_array(double*, const double*);
void show_array(const double*, const double*);
void revalue(double, double*, const double*);
int main()
{
    using namespace std;
    double properties[Max];
    double *pt_end = fill_array(properties, properties+Max);
    show_array(properties, pt_end);
    if ((pt_end - properties) > 0)
    {
        cout << "Enter revaluation factor: ";
        double factor;
        while (!(cin >> factor))
        {
            cin.clear();
            while (cin.get() != '\n')
                continue;
            cout << "Bad input; Please enter a number: ";
        }
        revalue(factor, properties, pt_end);
        show_array(properties, pt_end);
    }
    cout << "Done.\n";
    cin.get();
    cin.get();
    return 0;
}
double *fill_array(double *begin, const double *end)
{
    using namespace std;
    double temp;
    double *pt;
    for (pt = begin; pt != end; pt++)
    {
        cout << "Enter value #" << (pt - begin)+1 << ": ";
        cin >> temp;
        if (!cin)
        {
            cin.clear();
            while (cin.get() != '\n')
                continue;
            cout << "Bad input; input process terminated.\n";
            break;
        }
        else if (temp < 0)
            break;
        *pt = temp;
    }
    return pt;
}
void show_array(const double *begin, const double *end)
{
    using namespace std;
    const double *pt;
    for (pt = begin; pt != end; pt++)
    {
        cout << "Property #" << (pt - begin)+1 << ": $";
        cout << *pt << endl;
    }
}
void revalue(double r, double *begin, const double *end)
{
    double *pt;
    for (pt = begin; pt != end; pt++)
        *pt *= r;
}
```

## 7.8
**題：** 
Redo Listing 7.15 without using the array class. Do two versions:

    a.  Use an ordinary array of const char * for the strings representing the
        season names, and use an ordinary array of double for the expenses.

    b.  Use an ordinary array of const char * for the strings representing the
        season names, and use a structure whose sole member is an ordinary array
        of double for the expenses. (This design is similar to the basic design
        of the array class.)
**解：** 
```Cpp
#include <iostream>
const int Seasons = 4;
const char *Snames[Seasons] = {"Spring", "Summer", "Fall", "Winter"};
void fill(double*, int);
void show(double[], int);
int main()
{
    double expenses[Seasons];
    fill(expenses, Seasons);
    show(expenses, Seasons);
    return 0;
}
void fill(double *pa, int n)
{
    for(int i = 0; i < n; i++)
    {
        std::cout << "Enter " << Snames[i] << " expenses: ";
        std::cin >> pa[i];
    }
}
void show(double da[], int n)
{
    double total = 0.0;
    std::cout << "\nEXPENSES\n";
    for(int i = 0; i < n; i++)
    {
        std::cout << Snames[i] << ": $" << da[i] << std::endl;
        total += da[i];
    }
    std::cout << "Total Expenses: $" << total << std::endl;
}
```
```Cpp
#include <iostream>
const int Seasons = 4;
const char *Snames[Seasons] = {"Spring", "Summer", "Fall", "Winter"};
struct expenses { double value[Seasons]; };
void fill(expenses*);
void show(expenses);
int main()
{
    expenses exp;
    fill(&exp);
    show(exp);
    return 0;
}
void fill(expenses *pa)
{
    for(int i = 0; i < Seasons; i++)
    {
        std::cout << "Enter " << Snames[i] << " expenses: ";
        std::cin >> pa->value[i];
    }
}
void show(expenses da)
{
    double total = 0.0;
    std::cout << "\nEXPENSES\n";
    for(int i = 0; i < Seasons; i++)
    {
        std::cout << Snames[i] << ": $" << da.value[i] << std::endl;
        total += da.value[i];
    }
    std::cout << "Total Expenses: $" << total << std::endl;
}
```

## 7.9
**題：** 
This exercise provides practice in writing functions dealing with arrays and structures. The following is a program skeleton. Complete it by providing the described functions:

    #include <iostream>
    using namespace std;
    const int SLEN = 30;
    struct student {
        char fullname[SLEN];
        char hobby[SLEN];
        int ooplevel;
    };
    // getinfo() has two arguments: a pointer to the first element of
    // an array of student structures and an int representing the
    // number of elements of the array. The function solicits and
    // stores data about students. It terminates input upon filling
    // the array or upon encountering a blank line for the student
    // name. The function returns the actual number of array elements
    // filled.
    int getinfo(student pa[], int n);
    // display1() takes a student structure as an argument
    // and displays its contents
    void display1(student st);
    // display2() takes the address of student structure as an
    // argument and displays the structure’s contents
    void display2(const student * ps);
    // display3() takes the address of the first element of an array
    // of student structures and the number of array elements as
    // arguments and displays the contents of the structures
    void display3(const student pa[], int n);
    int main()
    {
        cout << "Enter class size:";
        int class_size;
        cin >> class_size;
        while (cin.get() != '\n')
            continue;
        student * ptr_stu = new student[class_size];
        int entered = getinfo(ptr_stu, class_size);
        for (int i = 0; i < entered; i++)
        {
            display1(ptr_stu[i]);
            display2(&ptr_stu[i]);
        }
        display3(ptr_stu, entered);
        delete [] ptr_stu;
        cout << "Done\n";
        return 0;
    }

**解：** 
```Cpp
#include <iostream>
using namespace std;
const int SLEN = 30;
struct student {
    char fullname[SLEN];
    char hobby[SLEN];
    int ooplevel;
};
int getinfo(student pa[], int n);
void display1(student st);
void display2(const student *ps);
void display3(const student pa[], int n);
int main()
{
    cout << "Enter class size: ";
    int class_size;
    cin >> class_size;
    while (cin.get() != '\n')
        continue;
    student * ptr_stu = new student[class_size];
    int entered = getinfo(ptr_stu, class_size);
    for (int i = 0; i < entered; i++)
    {
        display1(ptr_stu[i]);
        display2(&ptr_stu[i]);
    }
    display3(ptr_stu, entered);
    delete [] ptr_stu;
    cout << "Done\n";
    return 0;
}
int getinfo(student pa[], int n)
{
    int i;
    for(i = 0; i < n; i++)
    {
        cout << "Student #" << i+1 << "\n Full name: ";
        if(!(cin.get(pa[i].fullname, SLEN))) return i;
        while(cin.get() != '\n') continue;
        cout << " Hobby: ";
        cin.getline(pa[i].hobby, SLEN);
        cout << " opplevel: ";
        cin >> pa[i].ooplevel;
        cin.get();
    }
    return i;
}
void display1(student st)
{
    cout << "\n Full name: " << st.fullname
         << "\n Hobby: " << st.hobby
         << "\n opplevel: " << st.ooplevel << endl;
}
void display2(const student *ps)
{
    cout << "\n Full name: " << ps->fullname
         << "\n Hobby: " << ps->hobby
         << "\n opplevel: " << ps->ooplevel << endl;
}
void display3(const student pa[], int n)
{
    for(int i = 0; i < n; i++)
    {
        cout << "\n Full name: " << pa[i].fullname
             << "\n Hobby: " << pa[i].hobby
             << "\n opplevel: " << pa[i].ooplevel << endl;
    }
}
```

## 7.10
**題：** 
Design a function calculate() that takes two type double values and a pointer to a function that takes two double arguments and returns a double. The calculate() function should also be type double, and it should return the value that the pointed-to function calculates, using the double arguments to calculate(). For example, suppose you have this definition for the add()function:

    double add(double x, double y)
    {
        return x + y;
    }

Then, the function call in the following would cause calculate() to pass the  values 2.5 and 10.4 to the add() function and then return the add() return value (12.9):

    double q = calculate(2.5, 10.4, add);

Use these functions and at least one additional function in the add() mold in a program.The program should use a loop that allows the user to enter pairs of numbers. For each pair, use calculate() to invoke add() and at least one other function. If you are feeling adventurous, try creating an array of pointers to add()-style functions and use a loop to successively apply calculate() to a series of functions by using these pointers. Hint: Here’s how to declare such an array of three pointers:

    double (*pf[3])(double, double);

You can initialize such an array by using the usual array initialization syntax and function names as addresses.')
**解：** 
```Cpp
#include <iostream>
const int SIZE = 3;
double add(double x, double y) { return x + y; }
double mult(double x, double y) { return x * y; }
double sub(double x, double y) { return x - y; }
void calculate(double, double, double (*pf[])(double, double), int);
int main(void)
{
    
}
void calculate(double x, double y, double (*pf[])(double,double), int n)
{
    for (int i = 0; i < n; i++)
        std::cout << pf[i](x, y) << std::endl;
}
```
