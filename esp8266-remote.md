---
title: esp8266-remote
tags:
  - esp8266
  - 嵌入式
category: 嵌入式 / ESP32
---

# esp8266-remote

ESP8266 遥控器（C++）
## GitHub

[仓库链接](https://github.com/hqz-2024/esp8266-remote)

## 相关

[[项目总览]]

## 项目 README

### 功能说明

按下BOOT按钮后，电机1和电机2同时满速正转5秒后自动停止，运行期间LED以250ms间隔闪烁（亮度30%）。

### 硬件连接

#### 电机驱动器引脚
- **电机1**
  - INA1 → GPIO15 (D8)
  - INB1 → GPIO13 (D7)

- **电机2**
  - INA2 → GPIO12 (D6)
  - INB2 → GPIO14 (D5)

#### 控制引脚
- **BOOT按钮** → GPIO0
- **LED指示灯** → GPIO16 (D0)

### 工作流程

1. 上电后电机处于停止状态，LED熄灭
2. 按下BOOT按钮触发电机启动
3. 两个电机同时满速正转（PWM=255）
4. LED开始闪烁，亮度30%（PWM=77），闪烁频率2Hz
5. 运行5秒后电机自动停止，LED熄灭
6. 可再次按下BOOT按钮重复上述流程

### 电机控制逻辑

| 状态 | INA | INB | 说明 |
|------|-----|-----|------|
| 停止 | LOW | LOW | 待机状态 |
| 正转 | PWM | LOW | 可调速正转 |
| 反转 | LOW | PWM | 可调速反转 |
| 刹车 | HIGH | HIGH | 快速制动 |

### 参数配置

- 电机运行时间：5000ms
- LED闪烁间隔：250ms
- LED亮度：30% (PWM值77/255)
- 电机速度：100% (PWM值255/255)
- 串口波特率：115200

### 编译上传

1. 使用Arduino IDE打开motor.ino
2. 选择开发板：ESP8266 Generic Module
3. 选择正确的串口
4. 点击上传
