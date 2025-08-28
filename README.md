![c232e262c0b7010fd1c4b5c2e59aa417](https://github.com/user-attachments/assets/cd8172f9-d725-4d5d-a92a-b2078176af2b)
# Wio-Terminal and Grove-温度传感器 搭建简易环境测温系统
本项目使用 Seeed Wio Terminal 与 Grove-温度传感器 搭建一个简单的环境温度采集与显示系统。通过本教程，你将学会：硬件连接与环境配置，读取传感器数据并在串口监视器中打印，在 Wio Terminal 屏幕上实时显示温度，该项目适合 Arduino 初学者和创客爱好者，帮助你快速上手传感器数据采集与屏幕显示。


🧰 材料清单
| 名称                 | 数量 | 说明                  |
| ------------------ | -- | ------------------- |
| Wio Terminal       | 1  | 主控板                 |
| Grove - 温度传感器 V1.2 | 1  | 使用热敏电阻检测环境温度        |
| Grove 连接线          | 1  | 连接传感器与 Wio Terminal |
| USB-C 数据线          | 1  | 用于供电和上传程序           |


⚙️ 环境准备

1.安装 Arduino IDE（建议 v1.8.19 或 v2.x）。

2.在 开发板管理器 中安装 Seeeduino SAMD Boards（版本 ≥ 1.8.5）。

3.在 库管理器 中安装以下库：
    
    TFT_eSPI（屏幕显示）
    
    Seeed_Arduino_FreeRTOS（可选）
    
4.选择开发板：
    Tools > Board > Seeeduino Wio Terminal
    
5.选择端口（通常为 COMx）。


🛠️ 硬件连接

1.将 Grove 温度传感器插入 Wio Terminal 的其中一个 Multi-Function Grove Connector 口

2.使用 USB-C 数据线连接电脑

![7156de350d974747833048aec20bbe67](https://github.com/user-attachments/assets/23e1c3a8-e7b6-4da0-9081-201399752322)


💻 示例代码
````markdown
#include <math.h>
#include "TFT_eSPI.h"
#include "SPI.h"

const int B = 4275000;            // B value of the thermistor
const int R0 = 100000;            // R0 = 100k
const int pinTempSensor = A0;     // Grove - Temperature Sensor connect to A0

TFT_eSPI tft = TFT_eSPI();  // 创建 TFT 对象

void setup() {
    Serial.begin(9600);

    // 初始化屏幕
    tft.begin();
    tft.setRotation(3);   // 设置屏幕方向
    tft.fillScreen(TFT_BLACK);
    tft.setTextColor(TFT_GREEN, TFT_BLACK);
    tft.setTextSize(3);

    tft.setCursor(20, 20);
    tft.println("Grove Temp Sensor");
}

void loop() {
    int a = analogRead(pinTempSensor);

    float R = 1023.0 / a - 1.0;
    R = R0 * R;

    float temperature = 1.0 / (log(R / R0) / B + 1 / 298.15) - 273.15; // 摄氏度

    // 串口输出
    Serial.print("temperature = ");
    Serial.print(temperature);
    Serial.println(" C");

    // 屏幕输出
    tft.fillRect(20, 80, 200, 40, TFT_BLACK); // 清除旧数据
    tft.setCursor(20, 80);
    tft.print("Temp: ");
    tft.print(temperature, 1); // 保留 1 位小数
    tft.println(" C");

    delay(1000);
}
````


🔍 代码解析

1.analogRead(pinTempSensor)：读取传感器电压值

2.热敏电阻公式：将电压转换为温度

3.tft.printf：在屏幕上显示实时温度

4.Serial.print：在串口监视器中输出调试信息


📊 应用与扩展

📡 将温度数据上传到云平台（如 Arduino IoT Cloud、MQTT）

🌈 配合 Wio Terminal 的 WiFi 功能做远程监测

🔔 增加蜂鸣器或 LED，当温度超过阈值时报警


📚 参考资源

1.[Wio Terminal Wiki](https://wiki.seeedstudio.com/Wio-Terminal-Getting-Started/ "点击打开 Wio Terminal Wiki")


2.[Grove - Temperature Sensor V1.2](https://wiki.seeedstudio.com/Grove-Temperature_Sensor_V1.2/ "点击打开 Grove - Temperature Sensor V1.2")


3.[Arduino 官方网站](https://www.arduino.cc)


📜 许可证

本项目采用 MIT License
。你可以自由使用、修改和分发本项目。


🙌 致谢

感谢 Seeed Studio 提供 Wio Terminal 与 Grove 模块，感谢社区创客的支持与贡献。
