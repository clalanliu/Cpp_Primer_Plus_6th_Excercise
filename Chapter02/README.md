# 第 2 章 《C++ Primer Plus中文版 (第六版)》 編程練習題之我解

## 2.1

**題：** 編寫一個C++程序，它顯示您的姓名和地址。

**解：**

```Cpp
#include <iostream>

int main() {

    using namespace std;

    cout << "Hi there, I'm ..." << endl;
    return 0;
}
```


## 2.2

**題：** 編寫一個C++程序，它要求用戶輸入一個以 long 為單位的距離， 然後將它轉換為碼（yard，一long 等於 220 碼）。
    
**解：**


```Cpp
#include <iostream>

int main() {

    using namespace std;

    int distance=0, yard;
    cout << "Please input a distance numebr in the unit of Long: ";
    cin >> distance;
    yard = distance * 220;

    cout << "The distance tranform in yards is: " << yard << endl;

    return 0;
}
```

## 2.3

**題：** 編寫一個C++程序，它使用 3 個用戶定義的函數（包括main()），並生成下面的輸出：

```bash

Three blind mice
Three blind mice
See how they run
See how they run

```
其中一個函數要調用兩次，該函數生成前兩行；另一個函數也被調用兩次，並生成其余的輸出。


**解：**


```Cpp
#include <iostream>

using namespace std;

void blind_mice() {
    cout << "Three blind mice." << endl;
    return;
}

void how_they_run() {
    cout << "See how they run" << endl;
    return;
}

int main() {

    blind_mice();
    blind_mice();

    how_they_run();
    how_they_run();
    return 0;
}
```


## 2.4

**題：** 編寫一個程序，讓用戶輸入其年齡，然後顯示該年齡包含多少個月，如下所示：

```bash
Enter your age: 29
```

**解：**

```Cpp
#include <iostream>

int main() {

    using namespace std;

    int years, months;
    cout << "Enter your age: ";
    cin >> years;

    months = years * 12;
    cout << years << " years is " << months << " monthes." << endl;  
    return 0;
}
```


## 2.5

**題：** 編寫一個程序，其中的main( )調用一個用戶定義的函數（以攝氏溫度值為參數，並返回相應的華氏溫度值）。該程序按下面的格式要 求用戶輸入攝氏溫度值，並顯示結果：

```bash
Please enter a Celsius value: 20

20 degrees Celsius is 68 degrees Fahrenheit.
```

轉換公式：華氏溫度 = 1.8×攝氏溫度 + 32.0


**解：**
```Cpp
#include <iostream>

double celsiu2fahrenit(double celsius) {
    return 1.8 * celsius + 32.0;
}

int main() {

    using namespace std;

    double celsius;
    cout << "Please enter a celsius value: ";
    cin >> celsius;

    cout << celsius << " degrees Celsius is " 
         << celsiu2fahrenit(celsius) << " degrees Fahrenheit." << endl;

    return 0;
}
```


## 2.6

**題：** 編寫一個程序，其main( )調用一個用戶定義的函數（以光年值為參數，並返回對應天文單位的值）。該程序按下面的格式要求用戶輸 入光年值，並顯示結果：

```bash
Enter the number of light years: 4.2

4.2 light years = 265608 astromonical units.
```
天文單位是從地球到太陽的平均距離（約150000000公里或93000000英里），光年是光一年走的距離（約10萬億公里或6萬億英里）（除太陽外，最近的恒星大約離地球4.2光年）。請使用double類型，轉換公式為：1光年=63240天文單位.


**解：**

```Cpp

#include <iostream>


double light_years2astromonical_unit(double light_years) {

    return light_years * 63240;
}

int main() {

    using namespace std;

    double light_years;
    cout << "nter the number of light years: ";
    cin >> light_years;

    cout << light_years 
         << " light years = " 
         << light_years2astromonical_unit(light_years)
         << " astromonical units." << endl;

    return 0;
}

```

## 2.7

**題：** 編寫一個程序，要求用戶輸入小時數和分鐘數。在 main() 函數中，將這兩個值傳遞給一個void函數，後者以下面這樣的格式顯示這兩個值：

```bash
Enter the number of hours: 9
Enter the number of minutes: 28

Time: 9:28
```

**解：**

```Cpp
#include <iostream>
using namespace std;

void display_time(double hours, double minutes) {

    cout << "Time: " << hours << ":" << minutes << endl;

    return;
}

int main() {    

    double hours, minutes;
    cout << "Enter the number of hours: ";
    cin >> hours;

    cout << "Enter the number of minutes: ";
    cin >> minutes;

    display_time(hours, minutes);

    return 0;
}
```





