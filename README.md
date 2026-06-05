# openpilot 外置雷达扩展插件

<table>
  <tr>
    <td><a href="https://youtu.be/UFTRnajgVLs"><img src="https://img.youtube.com/vi/UFTRnajgVLs/hqdefault.jpg"></a></td>
    <td><a href="https://youtu.be/DK-5TdNM60k"><img src="https://img.youtube.com/vi/DK-5TdNM60k/hqdefault.jpg"></a></td>
  </tr>
</table>

## 项目概览
本项目可将外置雷达接入 openpilot，提升环境感知能力。

## 为什么做这个项目？
部分已安装 openpilot 的车辆只能使用纯视觉自适应巡航（VOACC）。为车辆加装外置雷达后，可通过传感器融合更准确地检测前车速度与距离，显著提升安全性和跟车表现。

**本指南与代码主要面向丰田车型**。如果你使用其他车型，需要：
   - 修改代码以接入外置雷达接口。
   - 如果雷达安装在车头前方，需要调整车辆前端到雷达的安装距离参数。

## 前置条件
安装本插件前，请确认：
- 车辆已安装 openpilot。
- 车辆支持 openpilot 纵向控制。
- 车辆本身无原厂雷达支持（如 Nissan、Volkswagen、Mazda、Subaru 等）。
- 使用 comma 3 或 3X 设备，且 **openpilot 版本为 0.9.8 或以上**。

## 物料清单（BOM）
1. [雷达](https://www.aliexpress.com/item/1005006713716767.html)，约 236 美元
2. GoPro 配件：
   * [手机夹](https://www.aliexpress.com/item/1005007539814670.html)，约 4.0 美元
   * [1/4 英寸转接件](https://www.aliexpress.com/item/1005006410768280.html)，约 1.0 美元
   * [短螺丝](https://www.aliexpress.com/item/32819832442.html)，约 1.2 美元
   * [快拆卡扣底座](https://www.aliexpress.com/item/1005006410768280.html)，约 1.0 美元
   * [短螺丝](https://www.aliexpress.com/item/32819832442.html)，约 1.2 美元
   * [平面胶贴底座](https://www.aliexpress.com/item/1005006441304068.html)，约 0.99 美元
3. [4 针凤凰端子连接器](https://www.aliexpress.com/item/1005006554550534.html)，约 2.00 美元

## 安装步骤

## 硬件安装
### 丰田线束接法：使用 CAN1 H/L，IGN 供电，GND 接地
![image](https://github.com/user-attachments/assets/fb6b939f-ed82-4c0a-b945-07ef2e38aeb2)
* 其他线束可参考：[Harness Schema](https://github.com/commaai/neo/blob/master/car_harness/v1/)
* 如供电电流不足，可能需要将 VCC 和 GND 接到 OBD2 口。

### 雷达支架安装
![91f0ffd1e2a280c6983ec02e0e43d259](https://github.com/user-attachments/assets/1ea0f87f-c736-4587-bbfe-b7a86333b1ed)

### 实车安装示例
![cf6ec2886d137ccc931ce7a3d0f34eae](https://github.com/user-attachments/assets/f98151be-1a3e-49cb-8697-aa39b19c6ec2)

### 接线图
请按下表将雷达与 openpilot 接到 OBD2 线束：

| 线束（车辆侧） | 雷达线 |
|----------------|--------|
| CAN1L          | CAN L（绿） |
| CAN1H          | CAN H（黄） |
| GND            | Ground（黑） |
| IGN            | 12V（红） |

### 软件配置（基于 0.9.8 测试）

#### opendbc/dbc/u_radar.dbc
上传到 `/data/openpilot/opendbc/dbc/u_radar.dbc`

#### opendbc/dbc/u_radar_config.py
1. 上传到 `/data/openpilot/opendbc/dbc/u_radar_config.py`
2. 修改雷达设置：
```
# 手动停止 openpilot
tmux a
多次按下 "<ctrl> + c"

# 应用设置
cd /data/openpilot/opendbc/dbc/ && python u_radar_config.py
```

#### opendbc/car/radar_interface.py
上传到 `/data/openpilot/opendbc/car/radar_interface.py`

#### 应用 card.py 改动
1. 上传 `panda.diff` 到 `/data`
2. 执行：
```bash
cd /data/openpilot/ && git apply ../card.diff
```

## 注意事项
* **安装支架**：当前支架设计为临时方案，便于快速调整和拆卸雷达。
* **CAN 报文冲突**：该方案为概念验证，可能因报文冲突触发错误；可通过 CAN 过滤器或网关解决。
* **供电不足**：雷达电流需求超过 0.2A；若现有供电不足，请改接 OBD2 供电。

## 贡献
欢迎贡献！如需改进功能，请提交 issue 或 pull request。

## 许可证
本项目使用 MIT 非商业许可证。

完整条款见 [LICENSE](LICENSE.md)。

## 致谢
感谢 openpilot 社区持续的支持与开发贡献。
