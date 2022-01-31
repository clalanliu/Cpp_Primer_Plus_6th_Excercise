# 第 4 章 《C++ Primer Plus中文版 (第六版)》 編程練習題之我解

## 4.1

**題：** 編寫一個程序，如下輸出實例所示的請求並顯示信息：

```bash
What is your first name? Betty Sue
Waht is your last name? Yewe
What letter grade do you deserve? B 
What is your age? 22

Name: Yewe, Betty Sue
Grade: C 
Age: 22

```

> 程序應接受的名字包含多個單詞。另外，程序將向下調整成績。假設用戶請求A、B或C，返回 B、C 或 D。


**解：**

```Cpp
#include <iostream>

int main() {

    using namespace std;

    char first_name[40];
    char last_name[40];
    char grade_letter;
    int age;

    cout << "What is your first name: ";
    cin.getline(first_name, 40);

    cout << "What is your last name: ";
    cin.getline(last_name, 40);

    cout << "What letter grade do you deserve: ";
    cin >> grade_letter;

    cout << "What is your age: ";
    cin >> age;

    cout << "Name: " << last_name << ", " << first_name << endl;
    cout << "Grade: " << char(grade_letter+1) << "\n";
    cout << "Age: " << age << endl;

    return 0;
}

```


## 4.2

**題：** 修改程序清單4.4，使用 C++ `string` 類而不是 `char` 數組。

**解：**

```Cpp
#include <iostream>
#include <string>

int main() {
    using namespace std;

    string name;
    string dessert;

    cout << "Enter your name: \n";
    getline(cin, name);

    cout << "Enter your favorite dessert:\n";
    getline(cin, dessert);

    cout << "I have delicious " << dessert;
    cout << " for you,  " << name << ".\n";

    return 0;
}

```

## 4.3

**題：** 編寫一個程序，它要求用戶首先輸入其名，然後輸入其姓；然後程序使用一個逗號和空格將姓和名組合起來，並存儲和顯示組合結果。請使用char數組和頭文件cstring中的函數。下面是該程序運行時的 情形：

```bash
Enter your first name: Flip
Enter your last name: Fleming
Here's the information in a single string: Fleming, Flip

```

**解：**

```Cpp
#include <iostream>
#include <cstring>

int main() {

    using namespace std;
    char first_name[20], last_name[20];
    char final_name[50];

    cout << "Enter your first name: ";
    cin.getline(first_name, 20);

    cout << "Enther your last name: ";
    cin.getline(last_name, 20);

    strcpy(final_name, last_name);
    strcat(final_name, ", ");
    strcat(final_name, first_name);

    cout << "Here's the information in a single string: " << final_name << endl;

    return 0;
}

```


## 4.4

**題：** 編寫一個程序，它要求用戶首先輸入其名，再輸入其姓；然後程序使用一個逗號和空格將姓和名組合起來，並存儲和顯示組合結果。請使用string對象和頭文件string中的函數。下面是該程序運行時的情形：

```bash
Enter your first name: Flip
Enter your last name: Fleming
Here's the information in a single string: Fleming, Flip

```

**解：**


```Cpp
#include <iostream>
#include <string>

int main() {

    using namespace std;
    string first_name, last_name;
    string final_name;

    cout << "Enter your first name: ";
    getline(cin, first_name);

    cout << "Enther your last name: ";
    getline(cin, last_name);

    final_name = last_name + ", " + first_name;

    cout << "Here's the information in a single string: " << final_name << endl;

    return 0;
}

```


## 4.5

**題：** 結構體 `CandyBar` 包含3個成員。第一個成員存儲了糖塊的品牌；第二個成員存儲糖塊的重量（可以有小數）；第三個成員存儲了糖塊的卡路里含量（整數）。編寫一個程序，聲明這個結構體，創建一個名為 `snack` 的 `CandyBar` 變量，並將其成員分別初始化為 "Mocha Munch"、2.3 和 350。初始化應在聲明 `snack` 時進行。最後，程序顯示 `snack` 變量的內容。


**解：**

```Cpp
#include <iostream>
#include <string>

struct CandyBar
{
    std::string name;
    double weight;
    int calories;
};


int main() {
    using namespace std;

    CandyBar snack = {"Mocha Munch", 2.3, 350};  // 初始化結構體

    cout << "The name of the CandyBar: " << snack.name << "\n";
    cout << "The weight of the candy: " << snack.weight << "\n";
    cout << "The calories information: " << snack.calories << endl;

    return 0;
}

```


## 4.6

**題：** 結構體 `CandyBar` 包含3個成員，如 `編程練習5`所示。請編寫一個程序，創建一個包含 3 個元素的 `CandyBar` 數組，並將它們初始化為所選擇的值，然後顯示每個結構體的內容。


**解：**

```Cpp
#include <iostream>
#include <string>

struct CandyBar
{
    std::string name;
    double weight;
    int calories;
};


int main() {

    using namespace std;

    CandyBar candbar[3] = {
        {"Mocha Munch", 2.3, 350},
        {"Big Rabbit", 5, 300},
        {"Joy Boy", 4.1, 430}
    };

    cout << "The name of the CandyBar: " << candbar[0].name << "\n"
         << "The weight of the candy: " << candbar[0].weight << "\n"
         << "The calories information: " << candbar[0].calories << "\n\n";

    cout << "The name of the CandyBar: " << candbar[1].name << "\n"
         << "The weight of the candy: " << candbar[1].weight << "\n"
         << "The calories information: " << candbar[1].calories << "\n\n";

    cout << "The name of the CandyBar: " << candbar[2].name << "\n"
         << "The weight of the candy: " << candbar[2].weight << "\n"
         << "The calories information: " << candbar[2].calories << endl;

    return 0;
}

```

## 4.7

**題：** William Wingate從事比薩餅分析服務。對於每個披薩餅，他都需要記錄下列信息：

- 披薩餅公司的名稱，可以有多個單詞組成；
- 披薩餅的直徑；
- 披薩餅的重量。

請設計一個能夠存儲這些信息的結構體，並編寫一個使用這種結構體變量的程序。程序將請求用戶輸入上述信息，然後顯示這些信息。請使用 cin（或其它的方法）和cout。

**解：**

```Cpp

#include <iostream>
#include <string>

struct Pizza
{
    std::string company;
    double diameter;
    double weight;
    
};

int main() {
    using namespace std;

    Pizza pizza;
    cout << "Enter the pizza company: ";
    getline(cin, pizza.company);

    cout << "Enter the diameter of pizza: ";
    cin >> pizza.diameter;

    cout << "Enter the weight of pizza: ";
    cin >> pizza.weight;

    cout << "\nHere is the pizza information: \n"
         << "Company: " << pizza.company << "\n"
         << "Diameter: " << pizza.diameter << "\n"
         << "Weight: " << pizza.weight << endl;

    return 0;
}

```

## 4.8

**題：** 完成編程練習7，但使用 `new` 來為結構體分配內存，而不是聲明一個結構體變量。另外，讓程序在請求輸入比薩餅公司名稱之前輸入比薩餅的直徑。


**解：**

```Cpp
#include <iostream>
#include <string>

struct Pizza
{
    std::string company;
    double diameter;
    double weight;
};

int main() {
    using namespace std;

    Pizza *pizza = new Pizza;
    
    cout << "Enter the diameter of pizza: ";
    cin >> pizza->diameter;

    cout << "Enter the weight of pizza: ";
    cin >> pizza->weight;

    // 注意上語句輸入完 weight 後，回車鍵留在輸入流中，以下的 getline 
    // 碰到輸入流中的回車就以為結束了，所以如果沒有這個 cin.get() 先讀
    // 取回車，那麽用戶永遠獲得 company 的值。
    cin.get(); 

    cout << "Enter the pizza company: ";
    getline(cin, pizza->company);

    cout << "\nHere is the pizza information: \n"
         << "Company: " << pizza->company << "\n"
         << "Diameter: " << pizza->diameter << "\n"
         << "Weight: " << pizza->weight << endl;

    delete pizza;

    return 0;
}

```


## 4.9

**題：** 完成編程練習6，但使用 `new` 來動態分配數組，而不是聲明一個包含 3 個元素的 `CandyBar` 數組。


**解：**

```Cpp
#include <iostream>
#include <string>

struct CandyBar
{
    std::string name;
    double weight;
    int calories;
};


int main() {

    using namespace std;

    CandyBar *p_candybar = new CandyBar [3] {
        {"Mocha Munch", 2.3, 350},
        {"Big Rabbit", 5, 300},
        {"Joy Boy", 4.1, 430}
    };

    // 輸出第一個結構體元素，按照數組的方式輸出
    cout << "The name of the CandyBar: " << p_candybar[0].name << "\n"
         << "The weight of the candy: " << p_candybar[0].weight << "\n"
         << "The calories information: " << p_candybar[0].calories << "\n\n";

    // 輸出第二個結構體元素，可以按照指針的邏輯輸出
    cout << "The name of the CandyBar: " << (p_candybar+1)->name << "\n"
         << "The weight of the candy: " << (p_candybar+1)->weight << "\n"
         << "The calories information: " << (p_candybar+1)->calories << "\n\n";

    // 輸出第三個結構體元素，又是數據的方式
    cout << "The name of the CandyBar: " << p_candybar[2].name << "\n"
         << "The weight of the candy: " << p_candybar[2].weight << "\n"
         << "The calories information: " << p_candybar[2].calories << endl;

    delete [] p_candybar;

    return 0;
}

```

## 4.10

**題：** 編寫一個程序，讓用戶輸入三次 40 碼跑的成績（如果您願意，也可讓用戶輸入40米跑的成績），並顯示次數和平均成績。請使用一個 `array`對象來存儲數據（如果編譯器不支持 `array` 類，請使用數組）。

**解：**

```Cpp
#include <iostream>
#include <array>

int main() {

    using namespace std;

    array<double, 3> result;

    cout << "Enter threed result of the 40 meters runing time: \n";
    cin >> result[0];
    cin >> result[1];
    cin >> result[2];

    double ave_result = (result[0] + result[1] + result[2]) / 3;
    cout << "The all three time results are: " << result[0] << ", "
         << result[1] << ", " << result[2] << endl;

    cout << "The average result: " << ave_result << endl;

    return 0;
}

```



