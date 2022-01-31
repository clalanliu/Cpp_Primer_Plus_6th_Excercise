# 第 3 章 《C++ Primer Plus中文版 (第六版)》 編程練習題之我解

## 3.1

**題：** 編寫一個小程序，要求用戶使用一個整數指出自己的身高（單位為英寸），然後將身高轉換為英尺和英寸。該程序使用下劃線字符來指示輸入位置。另外，使用一個const符號常量來表示轉換因子。

**解:**

```Cpp
#include <iostream>
const int Foot2inch = 12;

int main() {

    using namespace std;

    int input_height = 0;
    cout << "Please input you height in inch: __\b\b";
    cin >> input_height;

    int height_foot = input_height / Foot2inch;
    int height_inch = input_height % Foot2inch;

    cout << "Your height in inch is: " << input_height 
         << "; transforming in foot and inch is: " 
         << height_foot << " foot "
         << height_inch << " inch." << endl;

    return 0;
}
```


## 3.2

**題：** 編寫一個小程序，要求以幾英尺幾英寸的方式輸入其身高，並以磅為單位輸入其體重。（使用3個變量來存儲這些信息。）該程序報告其BMI（Body Mass Index，體重指數）。為了計算BMI，該程序以英寸的方式指出用戶的身高（1英尺為12英寸），並將以英寸為單位的身高轉換為以米為單位的身高（1英寸=0.0254米）。然後，將以磅為單位的體重轉換為以千克為單位的體重（1千克=2.2磅）。最後，計算相應的BMI—體重（千克）除以身高（米）的平方。用符號常量表示各種轉 換因子。

**解：**

```Cpp
#include <iostream>

const int Foot2Inch = 12;
const double Inch2Meter = 0.0254;
const double Kg2Pound = 2.2;

double BMI(double weight, double height) {
    return weight/(height*height);
}

int main() {

    using namespace std;

    double height_foot = 0;
    double height_inch = 0;
    double weight_pound = 0;

    cout << "Please enter your height in foot and Inch2Meter." << endl;
    cout << "Enter the foot of height: __\b\b";
    cin >> height_foot;

    cout << "Enter the inch of height: __\b\b";
    cin >> height_inch;

    cout << "Please enter your weight in pound: __\b\b";
    cin >> weight_pound;

    double height_meter = (height_foot * Foot2Inch + height_inch) * Inch2Meter;
    double weight_kg = weight_pound / Kg2Pound;
    double bmi = BMI(weight_kg, height_meter);

    cout << "Your BMI is: " << bmi << endl;

    return 0;
}
```


## 3.3

**題：** 編寫一個程序，要求用戶以度、分、秒的方式輸入一個緯度，然後以度為單位顯示該緯度。1度為60分，1分等於60秒，請以符號常量的方式表示這些值。對於每個輸入值，應使用一個獨立的變量存儲它。 下面是該程序運行時的情況：

```bash
Enter a latitude in degree, minutes, and seconds:
First, enter the degree: 37
Next, enter the minutes of arc: 51
Finally, enter the seconds of arc: 19

37 degrees, 51 minutes, 19 seconds = 37.8553 degrees.

```

**解：**

```Cpp

#include <iostream>


int main() {

    using namespace std;

    double degree, minutes, seconds;

    cout << "Enter a latitude in degree, minutes and seconds." << endl;
    cout << "First, enter the degree: ";
    cin >> degree;

    cout << "Next, enter the minutes of arc: ";
    cin >> minutes;

    cout << "Finally, enter the seconds of arc: ";
    cin >> seconds;

    double degree2 = degree + minutes/60 + seconds/3600;
    cout << degree << " degrees, " << minutes << " minutes, "
         << seconds << " seconds = " << degree2 << endl;

    return 0;
}
```


## 3.4

**題：** 編寫一個程序，要求用戶以**整數**方式輸入秒數（使用long或long long變量存儲），然後以天、小時、分鐘和秒的方式顯示這段時間。使用符號常量來表示每天有多少小時、每小時有多少分鐘以及每分鐘有多 少秒。該程序的輸出應與下面類似：

```bash
Enter the number of seconds: 31600000
31600000 seconds = 365 days, 17 hours, 46 minutes, 40 seconds.

```

**解：**

```Cpp

#include <iostream>

int main() {

    using namespace std;

    long total_seconds;

    cout << "Enter the number of seconds: ";
    cin >> total_seconds;

    int days = total_seconds / 86400;
    int hours = (total_seconds % 86400) / 3600;
    int minutes = ((total_seconds % 86400) % 3600) / 60;
    int seconds = ((total_seconds % 86400) % 3600) % 60;

    cout << total_seconds << "seconds = " 
         << days << " days, " 
         << hours << " hours, "
         << minutes << " minutes, "
         << seconds << " seconds." << endl;

    return 0;
}

```


## 3.5

**題：** 編寫一個程序，要求用戶輸入全球當前的人口和中國當前的人口（或其他國家的人口）。將這些信息存儲在long long變量中，並讓程序顯示中國（或其他國家）的人口占全球人口的百分比。該程序的輸出 應與下面類似：

```bash
Enter the world's population: 7850176700
Enther the population of China: 1411780000
The population of the China is 17.9841% of the world population.

```

**解：**

```Cpp

#include <iostream>

int main() {
    using namespace std;

    long long population_world, population_China;
    cout << "Enter the world's population: ";
    cin >> population_world;
    cout << "Enter the population of China: ";
    cin >> population_China;

    double rate = double(population_China)/population_world;
    cout << "The population of the China is " << rate * 100
         << "% of the world population." << endl;

    return 0;
}

```


## 3.6 

**題：** 編寫一個程序，要求用戶輸入驅車里程（英里）和使用汽油量（加侖），然後指出汽車耗油量為一加侖的里程。如果願意，也可以讓程序要求用戶以公里為單位輸入距離，並以升為單位輸入汽油量，然後 指出歐洲風格的結果—即每100公里的耗油量（升）。

**解：**

```Cpp

#include <iostream>

int main() {
    using namespace std;
    double kilometer, oil_liter;

    cout << "Enter the distance that you've dirver in kilometer: ";
    cin >> kilometer;

    cout << "Enter the comsumption of oil: ";
    cin >> oil_liter;

    double kilometer_per_liter = kilometer / oil_liter;
    cout << "The average fuel comsumption is " 
         << 100 / kilometer_per_liter << " L/100km" << endl;
}

```


## 3.7

**題：** 編寫一個程序，要求用戶按歐洲風格輸入汽車的耗油量（每100公里消耗的汽油量（升）），然後將其轉換為美國風格的耗油量—每加侖多少英里。

> 注意，除了使用不同的單位計量外，美國方法（距離/燃 料）與歐洲方法（燃料/距離）相反。100公里等於62.14英里，1加侖等於3.875升。因此，19mpg大約合12.4l/100km，27mpg大約合 8.71/100km。 

**解：**

```Cpp

#include <iostream>

int main() {
    using namespace std;

    const double Km2Mile = 0.6214;
    const double Gallon2Litre = 3.875;

    double fuel_comsuption_en = 0.0;
    cout << "Enter the fuel comsuption in European standard: ";
    cin >> fuel_comsuption_en;

    double fuel_comsuption_us = (100 * Km2Mile) / (fuel_comsuption_en/Gallon2Litre);
    cout << "The fuel comsuption in US standard is " << fuel_comsuption_us 
         << " Miles/Gallon (mpg)." << endl;  

    return 0;
}

```
