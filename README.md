# ESP32 OLED 驱动

本项目使用了 U8g2 单色屏图形库，为你写好了 I2C 的回调函数，你只需配置 scl、sda 的引脚即可使用 U8g2 的所有功能。



ESP32-LyraT 开发板 + 控制器为 SSD1306 的显示屏测试通过，无需任何修改可以直接 clone 编译 烧写 运行。  
其他 ESP32 开发板和显示屏理论上只需做些修改即可使用

## 安装

```bash
git clone https://github.com/hawav/esp32-oled.git
git submodule update --init --recursive
```

## 移植

### 移植到其他 ESP32 开发板

修改 `main.c` 中的

```c
// 根据自身情况修改 I2C 引脚，以下引脚为 esp32-LyraT 开发板默认引脚
u8g2_hal_i2c_cfg_t i2c_cfg = {
    .scl = 23,
    .sda = 18,
};
```

### 移植到其他单色显示屏

修改 `u8g2_hal.c` 中的

```c
u8g2_Setup_ssd1306_i2c_128x64_noname_f(&u8g2,
                                        U8G2_R0,
                                        u8g2_i2c_byte_cb,
                                        u8g2_gpio_and_delay_cb);
```

更改 `u8g2_Setup_ssd1306_i2c_128x64_noname_f` 函数  
[参考这里选择构造函数](https://github.com/olikraus/u8g2/wiki/u8g2setupc#setup-function-reference)  
这么多构造函数，应该选哪个呢？[可能有用的信息](https://github.com/olikraus/u8g2/issues/213#issuecomment-285926535)  
（本项目目前仅支持硬件 I2C，不支持软件模拟、硬件 spi。如果需要这些功能，[这个库](https://github.com/AnderVentura/U8G2-HAL-ESP32)可以帮到你）
