# 第 5 章 《C++ Primer Plus中文版 (第六版)》 編程練習題之我解
## 5.1

**題：** 編寫一個要求用戶輸入兩個整數的程序。該程序將計算並輸出這兩個整數之間（包括這兩個整數）所有整數的和。這里假設先輸入較小的整數。例如，用戶輸入的是2和9，則程序將指出2～9之間所有整數的和為44。


**解：**

```Cpp
#include <iostream>

int main() {
    using namespace std;
    int number1, number2;

    cout << "Enter the first number: ";
    cin >> number1;

    cout << "Enter the second number: ";
    cin >> number2;

    if (number1 > number2) {
        int tmp;
        tmp = number1;
        number1 = number2;
        number2 = tmp;
    }

    int s = 0;
    for (int num=number1; num < number2+1; ++num) {
        s += num;
    }

    cout << "Sum the number from " << number1 << " to " << number2 
         << ", sum = " << s << endl;

    return 0;
}

```


## 5.2

**題：** 使用 `array` 對象（而不是數組）和 `long double`（而不是 `long long`）重新編寫程序清單5.4，並計算 100! 的值。

**解：**

```Cpp
#include <iostream>
#include <array>


const int ar_size = 100;
int main() {
    using namespace std;

    array<long double, ar_size> factorials;

    factorials[0] = factorials[1] = 1;
    for (int i = 2; i < ar_size+1; ++i) {

        factorials[i] = i * factorials[i-1];
    }

    for (int i = 0; i < ar_size + 1; ++i) {

        cout << i << "! = " << factorials[i] << "\n";
    }
    cout << endl;

    return 0;
}

```


## 5.3

**題：** 編寫一個要求用戶輸入數字的程序。每次輸入後，程序都將報告到目前為止，所有輸入的累計和。當用戶輸入 0 時，程序結束。


**解：**

```Cpp
#include <iostream>


int main() {
    using namespace std;

    double s = 0;
    double ch;

    while (1) {

        cout << "Enter a number (int/double) (0 to exit): ";
        cin >> ch;

        if (ch == 0) {
            break;
        }

        s += ch;
        cout << "Until now, the sum of the number you inputed is: " 
             << s << endl;
    }

    return 0;
}

```


## 5.4

**題：** Daphne以10%的單利投資了100美元。也就是說，每一年的利潤都是投資額的10%，即每年10美元。而Cleo以5%的覆利投資了100美元。也就是說，利息是當前存款（包括獲得的利息）的5%，Cleo在第一年投資100美元的盈利是5%—得到了105美元。下一年的盈利是105美元的5%—即5.25美元，依此類推。請編寫一個程序，計算多少年後，Cleo的投資價值才能超過Daphne的投資價值，並顯示此時兩個人的投資價值。


**解：**

```Cpp
#include <iostream>

int main() {
    using namespace std;

    double daphne_account = 100;
    double cleo_account = 100;

    int year = 0;
    while (cleo_account <= daphne_account) {
        ++year;

        daphne_account += 10;
        cleo_account += cleo_account * 0.05;
    }

    cout << "After " << year << " Years. " 
         << "Cleo's account is " << cleo_account
         << " while more than the one of Daphne which is " 
         << daphne_account << "." << endl;

    return 0;
}

```


## 5.5

**題：** 假設要銷售《C++ For Fools》一書。請編寫一個程序，輸入全年中每個月的銷售量（圖書數量，而不是銷售額）。程序通過循環，使用初始化為月份字符串的 `char *` 數組（或 `string` 對象數組）逐月進行提示，並將輸入的數據儲存在一個int數組中。然後，程序計算數組中各元素的總數，並報告這一年的銷售情況。


**解：**

```Cpp

#include <iostream>
#include <string>

int main() {
    using namespace std;

    string months[12] = {"Jan", "Feb", "Mar", "Apr", 
                         "May", "Jun", "Jul", "Aug", 
                         "Sep", "Oct", "Nov", "Dec"};
    int sell[12];
    int total_sales = 0;

    cout << "Enter the sales of book <<C++ for Fools>> each month." << endl;
    for (int i=0; i < 12; ++i) {

        cout << months[i] << ":";
        cin >> sell[i];

        total_sales += sell[i];
    }

    cout << "\nThe total sales is " << total_sales << endl;
    for (int i=0; i < 12; ++i) {

        cout << months[i] << ": " << sell[i] << endl;
    }


    return 0;
}
```

## 5.6

**題：** 完成編程練習5，但這一次使用一個二維數組來存儲輸入——3年中每個月的銷售量。程序將報告每年銷售量以及三年的總銷售量。


**解：**

```Cpp
#include <iostream>
#include <string>


int main() {
    using namespace std;

    string months[12] = {"Jan", "Feb", "Mar", "Apr", 
                         "May", "Jun", "Jul", "Aug", 
                         "Sep", "Oct", "Nov", "Dec"};
    int sells[3][12];
    int total_sales[3] = {0, 0, 0};

    for (int i=0; i<3; ++i) {

        cout << "Enter " << i+1 << " year(s) sales of book <<C++ for Fools>> each month." << endl;
        for (int j=0; j<12; ++j) {
            cout << months[j] << ": ";
            cin >> sells[i][j];

            total_sales[i] += sells[i][j];

        }
    }

    for (int i=0; i<3; ++i) {
        cout << i+1 << " year(s) total sales is " 
             << total_sales[i] << endl;
    }

    cout << "There years total sales is " 
         << total_sales[0] + total_sales[1] + total_sales[2] << endl;

    return 0;
}

```


## 5.7

**題：** 設計一個名為 `car` 的結構體，用它存儲下述有關汽車的信息：生產商（存儲在字符數組或 `string` 對象中的字符串）、生產年份（整數）。編寫一個程序，向用戶詢問有多少輛汽車。隨後，程序使用new來創建一個由相應數量的 `car` 結構體組成的動態數組。接下來，程序提示用戶輸入每輛車的生產商（可能由多個單詞組成）和年份信息。請注意，這需要特別小心，因為它將交替讀取數值和字符串（參見第4章）。最後，程序將顯示每個結構的內容。該程序的運行情況如下:

How many cars do you wish to catalog? 2
Car #1: 
Please enter the maker: Hudson Hornet
Please enter the year made: 1952
Car #2:
Please enter the maker: Kaiser
Please enter the year made: 1951
Here is your collection:
1952 Hudson Hornet
1951 Kaiser


**解：**

```Cpp
#include <iostream>
#include <string>


int main() {
    using namespace std;

    struct Car {
        string company;
        int year;  
    };

    int car_num = 0;
    cout << "How many cars do you wish to catalog? ";
    cin >> car_num;
    cin.get()  // 讀取輸入流末尾的回車

    Car *cars = new Car[car_num];
    for (int i=0; i < car_num; ++i) {
        cout << "Please enter the maker: ";
        cin >> (cars+i)->company;

        cout << "Please enter the year made: ";
        cin >> (cars+i)->year;
    }

    cout << "\nHere is your collection: \n";
    for (int i=0; i < car_num; ++i) {
        cout << cars[i].year << " " << cars[i].company << endl;
    }

    delete [] cars;
    return 0;
}
```


## 5.8

**題：** 編寫一個程序，它使用一個 `char`數組和循環，每次讀取一個單詞，直到用戶輸入 `done` 為止。隨後，該程序指出用戶輸入了多少個單詞（不包括done在內）。下面是該程序的運行情況：

Enter words (type 'done' to stop):
anteater birthday category dumpster
envy finagle genometry done for sure

You entered a total of 7 words.

> 您應在程序中包含頭文件 `cstring`，並使用函數 `strcmp()` 來進行比較測試。


**解：**

```Cpp
#include <iostream>
#include <cstring>

int main() {
    using namespace std;

    int word_count = 0;
    char ch[80];
    cout << "Enter a word (type 'done' to stop the program.): \n";
    do {
        cin >> ch;

        if (strcmp(ch, "done") != 0) {
            word_count++;
        }

    } while (strcmp(ch, "done") != 0);

    cout << "\nYou entered a total of " << word_count << " words." << endl;

    return 0;
}

```



## 5.9

**題：** 編寫一個滿足前一個練習中描述的程序，但使用 `string` 對象而不是字符數組。請在程序中包含頭文件 `string`，並使用關系運算符來進行 比較測試。


**解：**

```Cpp
#include <iostream>
#include <string>

int main() {
    using namespace std;

    int word_count = 0;
    string ch;
    cout << "Enter a word (type 'done' to stop the program.): \n";
    do {
        cin >> ch;

        if (ch != "done") {
            word_count++;
        }

    } while (ch != "done");

    cout << "\nYou entered a total of " << word_count << " words." << endl;

    return 0;
}

```


## 5.10

**題：** 編寫一個使用嵌套循環的程序，要求用戶輸入一個值，指出要顯示多少行。然後，程序將顯示相應行數的星號，其中第一行包括一個星號，第二行包括兩個星號，依此類推。每一行包含的字符數等於用戶指定的行數，在星號不夠的情況下，在星號前面加上句點。該程序的運 行情況如下：

Enter number of rows: 5
Output: 
....*
...**
..***
.****
*****


**解:**

```Cpp
#include <iostream>

int main() {

    using namespace std;
    int line_num = 0;

    cout << "Enter the number of rows: ";
    cin >> line_num;

    cout << "Output:" << endl;
    for (int i = line_num; i > 0; --i) {

        for (int j = i-1; j > 0; --j) {
            cout << ".";
        }
        for (int j = line_num - (i-1); j > 0; --j) {
            cout << "*";
        }
        cout << "\n";
    }

    return 0;
}

```


