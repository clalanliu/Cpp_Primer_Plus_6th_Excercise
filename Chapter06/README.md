# 第 6 章 《C++ Primer Plus中文版 (第六版)》 編程練習題之我解

## 6.1

**題：** 編寫一個程序，讀取鍵盤輸入，直到遇到 `@`符號為止，並回顯輸入（數字除外），同時將大寫字符轉換為小寫，將小寫字符轉換為大 寫（別忘了`cctype`函數系列）。


**解：**

```Cpp
#include <iostream>
#include <cctype>


int main() {
    using namespace std;
    char ch;

    cout << "Enter any charater: ";
    while ((ch=cin.get()) != '@') {

        if (isdigit(ch)) {
            continue;
        } else if (islower(ch)) {
            ch = toupper(ch);
        } else if (isupper(ch)) {
            ch = tolower(ch);
        }

        cout << ch;

    }

    cout << "** done **" << endl;

    return 0;
}
```


## 6.2

**題：** 編寫一個程序，最多將10個 `donation` 值讀入到一個 `double` 數組中（如果您願意，也可使用模板類 `array` ）。程序遇到非數字輸入時將結束輸入，並報告這些數字的平均值以及數組中有多少個數字大於平均值。


**解：**

```Cpp
#include <iostream>
#include <array>


int main() {
    using namespace std;
    
    const unsigned int size = 10;
    array<double, size> donation;

    double sum_value = 0;
    unsigned int large_avg = 0, n = 0;

    cout << "Enter 10 double value or type non-digital value to exit: ";
    while ((n < size) && (cin >> donation[n])) {
        
        sum_value += donation[n];
        ++n;
    }

    double avg = sum_value / n;
    for (int i=0; i < n; i++) {

        if (donation[i]>avg)
            ++large_avg;
    }

    cout << "The average value is: " << avg
         << ", there are " << large_avg
         << " larger than average value." << endl;

    return 0;
}

```


## 6.3

**題：** 編寫一個菜單驅動程序的雛形。該程序顯示一個提供4個選項的菜單——每個選項用一個字母標記。如果用戶使用有效選項之外的字母進行響應，程序將提示用戶輸入一個有效的字母，直到用戶這樣做為止。然後，該程序使用一條 `switch` 語句，根據用戶的選擇執行一個簡單操作。該程序的運行情況如下：

```bash
Please enter one of the following choices:

c) carnivore    p) pianist
t) tree         g) game
f

Please enter a c, p, t, or g: q
A maple is a tree.

```

**解：**

```Cpp
#include <iostream>

int main() {

    using namespace std;
    cout << "Please enter one of the following choice: \n";
    cout << "c) carnivore\tp) pianist.\n"
         << "t) tree\tg) game" << endl;

    bool exit = false;
    char c;
    while (!exit && (cin >> c)) {

        switch (c) {
            case 'c': 
                cout << "Tiger is a carnivore." << endl;
                exit = true;
                break;
            case 'p':
                cout << "Mozart is a great pianst." << endl;
                exit = true;
                break;
            case 't':
                cout << "A maple is a tree." << endl;
                exit = true;
                break;
            case 'g':
                cout << "Supper Mario is a great game." << endl;
                exit = true;
                break;

            default:
                cout << "Please enter c, p, t, or g: q" << endl;
                break;
        }
    }
    return 0;
}

```


## 6.4

**題：** 加入 `Benevolent Order of Programmer` 後，在BOP大會上，人們便可以通過加入者的真實姓名、頭銜或秘密BOP姓名來了解他（她）。請編寫一個程序，可以使用真實姓名、頭銜、秘密姓名或成員偏好來列出成員。編寫該程序時，請使用下面的結構：

```Cpp
// Benevolent order of programmers strcture

struct bop {
    char fullname[strsize]; // real name
    char title[strsize];    // job title
    char bopname[strsize];  // secret BOP name
    int preference;         // 0 = fullname, 1 = title, 2 = bopname
};

```

該程序創建一個有上述結構體組成的小型數組，並將其初始化為適當的值。另外，該程序使用一個循環，讓用戶在下面的選項中進行選擇：

```bash
a. display by name     b. display by title
c. display by bopname  d. display by preference
q. quit
```

注意，`display by preference` 並不意味著顯示成員的偏好，而是意味著根據成員的偏好來列出成員。例如，如果偏好號為 1，則選擇 d 將顯示成員的頭銜。該程序的運行情況如下：

```bash
Benevolent order of Programmers report.

a. display by name     b. display by title
c. display by bopname  d. display by preference
q. quit

Enter your choices: a
Wimp Macho
Raki Rhodes
Celia Laiter
Hoppy Hipman
Pat Hand

Next choice： d   
Wimp Macho
Junior Programmer
MIPS
Analyst Trainee
LOOPY

Next choice: q
Bye!

```


**解：**


```Cpp
#include <iostream>


int main() {

    using namespace std;

    const int strsize = 80;
    struct Bop {
        char fullname[strsize]; // real name
        char title[strsize];    // job title
        char bopname[strsize];  // secret BOP name
        int preference;         // 0 = fullname, 1 = title, 2 = bopname
    };

    const int size = 5;
    const Bop bops[size] = {
        {"Wimp Macho", "bbb", "c", 0},
        {"Raki Rhodes", "2XXXX", "3XXXXX", 1},
        {"Celia Laiter", "2AAAA", "3AAAAA", 2},
        {"Hoppy Hipman", "2BBBB", "3BBBBB", 0},
        {"Pat Hand", "4CCCC", "3CCCCC", 1}
    };

    cout << "Benevolent order of Programmers report.\n";
    cout << "a. display by name     b. display by title\n"
         << "c. display by bopname  d. display by preference\n"
         << "q. quit" << endl;

    char ch;
    while (cin >> ch) {

        if (ch == 'q') { 
            break; 
        }

        for (int i=0; i < size; ++i) {

            switch (ch) {
                case 'a':
                    cout << bops[i].fullname << "\n";
                    break;
                case 'b':
                    cout << bops[i].title << "\n";
                    break;
                case 'c':
                    cout << bops[i].bopname << "\n";
                    break;
                case 'd':
                    cout << bops[i].preference << "\n";
                    break;

                default:
                    break;
            }
        }

        cout << "Next choice: ";
    }
    cout << "** Done **" << endl;
    return 0;
}

```


## 6.5

**題：** 在 `Neutronia` 王國，貨幣單位是 `tvarp`，收入所得稅的計算方式如下：

- 5000 tvarps：不收稅;
- 5001～15000 tvarps：10%;
- 15001～35000 tvarps：15%;
- 35000 tvarps以上：20%;

例如，收入為 `38000 tvarps` 時，所得稅為 `5000 * 0.00 + 10000 * 0.10 + 20000 * 0.15 + 3000 * 0.20`，
即 `4600 tvarps`。請編寫一個程序，使用循環來 要求用戶輸入收入，並報告所得稅。當用戶輸入負數或非數字時，循環將結束。


**解：**

```Cpp
#include <iostream>

int main() {
    using namespace std;
    const double tax_rate1 = 0.1;
    const double tax_rate2 = 0.15;
    const double tax_rate3 = 0.20;

    double income = 0.0, tax = 0.0;
    cout << "Please enter your income: ";
    while ((cin >> income) && (income > 0)) {

        if (income <= 5000) {
            tax = 0.0;
        } else if (income <= 15000 ) {

            tax = (income - 5000) * tax_rate1;
        } else if (income <= 35000) {

            tax = (15000 - 5000) * tax_rate1 + (income - 15000) * tax_rate2;
        } else {
            tax = (15000 - 5000) * tax_rate1 + (35000 - 15000) * tax_rate2 + (income - 35000) * tax_rate3;
        }

        cout << "Income = " << income << ", tax = " << tax << endl;
        cout << "Please enter your income again or enter a negative value to quit: ";
    }

    return 0;
}

```


## 6.6

**題：** 編寫一個程序，記錄捐助給 “維護合法權利團體” 的資金。該程序要求用戶輸入捐獻者數目，然後要求用戶輸入每一個捐獻者的姓名和款項。這些信息被儲存在一個動態分配的結構體數組中。每個結構體有兩個成員：用來儲存姓名的字符數組（或 `string`對象）和用來存儲款項的 `double`成員。讀取所有的數據後，程序將顯示所有捐款超過 10000 的捐款者的姓名及其捐款數額。

該列表前應包含一個標題，指出下面的捐款者是重要捐款人 Grand Patrons。然後，程序將列出其他的捐款者，該列表要以 `Patrons` 開頭。如果某種類別沒有捐款者，則程序將打印單詞 `none`。該程序只顯示這兩種類別，而不進行排序。


**解：**

```Cpp
#include <iostream>
#include <string>


int main() {

    using namespace std;

    const int Grand_Amount = 10000; 

    struct Patron {
        string name;
        double amount;
    };

    int contribute_num = 0;
    cout << "Enter the number of contributor: ";
    cin >> contribute_num;
    cin.get();  // 讀取輸入流中的回車符

    Patron *p_contribution = new Patron [contribute_num];
    for (int i = 0; i < contribute_num; ++i) {
        cout << "Enter the name of " << i + 1 << " contributor: ";
        getline(cin, p_contribution[i].name);

        cout << "Enter the amount of donation: ";
        cin >> p_contribution[i].amount;
        cin.get();  // 讀取輸入流中的回車符
    }

    unsigned int grand_amount_n = 0;
    cout << "\nGrand patron: " << endl;
    for (int i = 0; i < contribute_num; ++i) {

        if (p_contribution[i].amount > Grand_Amount) {
            cout << "Contributor name: " << p_contribution[i].name << "\n"
                 << "Contributor amount: " << p_contribution[i].amount << endl;
            ++grand_amount_n;
        }
    }

    if (grand_amount_n == 0) {
        cout << "None" << endl;
    }

    bool is_empty = true;
    cout << "\nPatrons: " << endl;
    for (int i=0; i < contribute_num; ++i) {
        cout << "Contributor name: " << p_contribution[i].name << "\n"
             << "Contributor amount: " << p_contribution[i].amount << endl;

        is_empty = false;
    }

    if (is_empty) {
        cout << "** None **" << endl;
    }

    return 0;
}

```


## 6.7

**題：** 編寫一個程序，它每次讀取一個單詞，直到用戶輸入 `q`。然後，該程序指出有多少個單詞以元音打頭，有多少個單詞以輔音打頭，還有多少個單詞不屬於這兩類。為此，方法之一是，使用 `isalpha()` 來區分以字母和其他字符打頭的單詞，然後對於通過了 `isalpha()` 測試的單詞，使用 `if` 或 `switch` 語句來確定哪些以元音打頭。

該程序的運行情況如下：


```bash
Enter words (q to quit):
The 12 awesome oxen ambled
quietly across 15 meters of lawn. q

5 words beginning with vowels
4 words beginning with consonants
2 others

```

**解：**

```Cpp
#include <iostream>
#include <cctype>
#include <string>


int main() {

    using namespace std;

    unsigned int vowels = 0;
    unsigned int consonants = 0;
    unsigned int other = 0;
    string input;

    cout << "Enter words (q to quit): " << endl;
    while (cin >> input) {
        if (input == "q")
            break;

        if (isalpha(input[0])) {
            switch(toupper(input[0])) {

                case 'A':;
                case 'E':;
                case 'I':;
                case 'O':;
                case 'U':
                    ++vowels;
                    break;

                default:
                    ++consonants;
                    break;
            }
        } else {
            ++other;
        }
    }

    cout << vowels << " words beginning with vowels.\n"
         << consonants << " words beginning with consonants.\n"
         << other << " words beginning with other letter." << endl;

    return 0;
}

```


## 6.8

**題：** 編寫一個程序，它打開一個文件文件，逐個字符地讀取該文件，直到到達文件末尾，然後指出該文件中包含多少個字符。


**解：**

```Cpp
#include <iostream>
#include <fstream>
#include <string>


int main() {
    using namespace std;

    string fn;
    ifstream in_file_handle;

    unsigned int n = 0;
    char ch;

    cout << "Enter a file name: ";
    getline(cin, fn);

    in_file_handle.open(fn.c_str());
    while ((ch = in_file_handle.get()) != EOF) {
        ++n;
    }
    in_file_handle.close();

    cout << "There are " << n << " characters in " 
         << fn << " file." << endl;

    return 0;
}

```


## 6.9

**題：** 完成編程練習6，但從文件中讀取所需的信息。該文件的第一項 應為捐款人數，余下的內容應為成對的行。在每一對中，第一行為捐款人姓名，第二行為捐款數額。即該文件類似於下面：

```bash
4 Sam Stone
2000
Freida Flass
100500
Tammy Tubbs
5000
Rich Raptor
55000
```

**解：**

```Cpp
#include <iostream>
#include <fstream>
#include <string>


int main() {

    using namespace std;
    const int Grand_Amount = 10000;
    string file_name; 
    ifstream in_file_handle;

    struct Patron {
        string name;
        double amount;
    };

    int contribute_num = 0;
    cout << "Enter a file name: ";

    getline(cin, file_name);
    in_file_handle.open(file_name.c_str());
    in_file_handle >> contribute_num;
    in_file_handle.get();  // 讀取空白

    Patron *p_contribution = new Patron [contribute_num];
    for (int i = 0; i < contribute_num; ++i) {
        /*
         * 4 Sam Stone
         * 2000
         * Freida Flass
         * 100500
         * Tammy Tubbs
         * 5000
         * Rich Raptor
         * 55000
         *
         */
        getline(in_file_handle, p_contribution[i].name);
        in_file_handle >> p_contribution[i].amount;
        in_file_handle.get();   // 讀掉空白(包括滯留在行末的回車符)
    }
    in_file_handle.close();

    unsigned int grand_amount_n = 0;
    cout << "\nGrand patron: " << endl;
    for (int i = 0; i < contribute_num; ++i) {

        if (p_contribution[i].amount > Grand_Amount) {
            cout << "Contributor name: " << p_contribution[i].name << "\n"
                 << "Contributor amount: " << p_contribution[i].amount << endl;
            ++grand_amount_n;
        }
    }

    if (grand_amount_n == 0) {
        cout << "None" << endl;
    }

    bool is_empty = true;
    cout << "\nPatrons: " << endl;
    for (int i=0; i < contribute_num; ++i) {
        cout << "Contributor name: " << p_contribution[i].name << "\n"
             << "Contributor amount: " << p_contribution[i].amount << endl;

        is_empty = false;
    }

    if (is_empty) {
        cout << "** None **" << endl;
    }

    delete [] p_contribution;
    return 0;
}


```

