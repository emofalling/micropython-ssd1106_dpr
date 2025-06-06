# `SSD1106-DPR` - 使用`DPR`加速的`SSD1106`驱动程序

使用`DPR`(Difference Page Refresh, 差分页面刷新)加速的MicroPython `SSD1106`驱动程序，支持I2C和SPI接口。

## `DPR`实现机制详解

`DPR`(Difference Page Refresh)技术通过以下核心策略优化显示性能：

1. **双缓冲架构设计**
   - 在原有显示缓冲区（`buffer`）基础上，新增一个独立缓冲区（`prev_buffer`）
   - 该备份缓冲区专门用于存储上一帧完整显示内容，形成双缓冲机制

2. **智能页级更新算法**
   - 每次执行`oled.show()`操作时，系统自动触发页级差异检测
   - 逐页比对当前`buffer`与`prev_buffer`的内容差异
   - 仅传输存在差异的页数据至显示控制器，跳过未修改页面

3. **技术优势**
   - 显著降低无效数据传输量（典型场景下减少50%+数据量）
   - 保持显示流畅性的同时降低功耗
   - 特别适用于静态内容占比高的显示场景

该实现通过空间换时间的策略，在保持显示接口兼容性的前提下，实现了显示刷新效率的质变提升。
