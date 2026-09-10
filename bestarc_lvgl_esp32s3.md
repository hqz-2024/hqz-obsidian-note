---
title: bestarc lvgl esp32s3（5 寸屏）
tags:
  - esp32
  - lvgl
  - 嵌入式
category: 嵌入式 / ESP32
---

# bestarc lvgl esp32s3（5 寸屏）

ESP32-S3 + LVGL 5 寸触摸屏（bestarc_5inch）
## GitHub

[仓库链接](https://github.com/hqz-2024/bestarc_lvgl_esp32s3)

## 相关

[[项目总览]]

## 项目 README

ESP32-S3 + LVGL v9，单工程支持 **主设备端 / 遥控器端** 两种角色，通过宏切换。

### 编译 / 烧录

```
idf.py set-target esp32s3
idf.py build
idf.py -p COMx flash monitor
```

### 角色切换

`main/APP/device_config.h`：

```c
#define DEVICE_ROLE   DEVICE_ROLE_MASTER   // 主设备：BLE Peripheral + UART
#define DEVICE_ROLE   DEVICE_ROLE_REMOTE   // 遥控器：BLE Central（含配置模式）
```

改完必须重新 `idf.py build`。

### 遥控器调试宏（跳过 App 配网）

`main/APP/device_config.h`：

```c
#define REMOTE_DEBUG_FIXED_MAC      1            // 1=启用 0=关闭
#define REMOTE_DEBUG_FIXED_MAC_B0   0xAA         // 主设备 BT MAC 6 字节
#define REMOTE_DEBUG_FIXED_MAC_B1   0xBB
#define REMOTE_DEBUG_FIXED_MAC_B2   0xCC
#define REMOTE_DEBUG_FIXED_MAC_B3   0xDD
#define REMOTE_DEBUG_FIXED_MAC_B4   0xEE
#define REMOTE_DEBUG_FIXED_MAC_B5   0xFF
```

启用后遥控器直接进入正常模式扫描该 MAC，不写 NVS、不进入配置屏。

### 运行行为

- **主设备**：广播 `BYS`，最多 2 路连接（App + 遥控器），Notify 多播。
- **遥控器（未绑定）**：广播 `BYS_remote`，等待 App 写入 12 字节（前 6=MAC，第 7,8=0xFF,0x00）保存后重启。
- **遥控器（已绑定）**：扫描名为 `BYS` 且 Manuf Data 前 6 字节匹配 MAC 的设备并连接。
- **5 连按 BOOT 键**：清除 NVS 中的 MAC 并重启进入配置模式。

### 目录

- `main/APP/ble/ble_server.c` 主设备 GATT 服务
- `main/APP/ble/ble_remote.c` 遥控器 BLE（配置/正常双模式）
- `main/APP/comm/uart_comm.c` 主设备 UART1 协议
- `main/APP/comm/remote_nvs.c` 遥控器 MAC 持久化
- `main/APP/comm/remote_button.c` BOOT 5 连按复位
- `main/APP/ui/screens/scr_dashboard.c` 主界面
- `main/APP/ui/` C 数组素材
- `main/APP/ui/fonts/` 字体素材
