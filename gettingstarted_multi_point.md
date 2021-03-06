# 多路数据归并处理
本教程能帮助你快速学习如何使用多路数据归并处理模板进行单设备多测点间的数据运算处理。
## 前提条件
- EnOS平台登录账号
- 已被授权流数据处理模块
- 已接入设备并且设备已经在发送数据

## 教程目标及数据准备
**教程目标**

本教程要实现的场景是：将原始采集点*testA_raw*和*testB_raw*的数据求和，取值结果输出给新点*test_C*。

**数据准备**

- 模型配置

本教程使用的模型*testModel*的配置如下：

Feature Type|Name|Identifier|Point Type |Data Type
---|---|---|---|---
Measure Point	 | testA_raw | testA_raw|AI |DOUBLE
Measure Point	 | testB_raw|testB_raw|AI |DOUBLE
Measure Point	 | testC|testC|AI |DOUBLE
- 存储配置：对*testA_raw*、*testB_raw*、*testC*点进行存储配置。这三个测点都可以配置为AI原始数据和AI分钟级归一化数据。具体配置方法，请参考[存储策略配置](https://www.envisioniot.com/docs/data-asset/zh_CN/latest/configuring_tsdb_storage.html)。
- 数据接入：请参考教程[设备连接快速入门](https://www.envisioniot.com/docs/device-connection/zh_CN/latest/gettingstarted_device_connection.html)来完成*testA_raw*和*testB_raw*测点的数据的采集。

## 操作步骤
使用多路数据归并处理模板对单设备多测点数据进行处理的步骤如下：
- 创建并配置多路数据归并处理流
- 保存并发布流任务
- 启动流任务
- 查看流任务任务运行结果

## 第一步：创建并配置多路数据归并处理流
1. 进入控制台，点击 **流数据处理 > 流开发** 菜单可浏览当前组织所有已创建的流任务，双击某一流任务，可进行详情查看并编辑

2. 在任务列表上方，点击  **+** 添加新任务，并选择**Multi Point Merge**模板。

3. **多路时序数据设置**：点击**添加模型**，选择*testModel*模型下的*testA_raw*、*testB_raw*、*testC*测点。选择的测点在编写数据处理策略表达式时会用于自动提示。

4. **触发方式配置**：选择**按点触发**的数据处理触发方式；选择**上一个值**作为时序插补策略。

   注：按点触发是指当指定测点的数据达到时，按照编写的数据处理逻辑表达式开始一次计算。当计算开始时，如果该时间切片上某一路参与计算的测点的数据未到达，则使用该测点的上一时刻数据参与计算。

5. **数据处理策略配置**：点击**新增策略**，完成以下数据处理策略配置。

   - ​输出点：选择测点*testC*作为承载计算结果的测点。

   - 触发点：如果同一时刻，*testA_raw*比*testB_raw*数据较晚上送，则选择*testA_raw*作为触发计算的测点。当系统检测到*testA_raw*的数据到来时，则触发输出逻辑。

   - 输出逻辑：编写输出点的输出逻辑表达式。此教程中，输出逻辑为：
   ```scala
   ${testModel::testA_raw}+${testModel::testB_raw}
   ```


## 第二步：保存并发布流任务
流任务配置完成后，需要对配置进行保存，保存后可激活发布按钮。点击**发布**，进入**流式数据处理 > 流运维**页面，就能看到已发布的所有任务流了。

## 第三步：启动流任务
在**流运维**页面，找到已经发布的流任务，点击**启动**按钮启动流任务。

## 第四步：查看任务运行结果
在**流运维**页面，在**名称**一栏中，点击已启动的流任务名称，可查看流任务运行情况：

- **Summary**: 查看流任务运行情况总结，比如整体处理记录统计、各个时间段聚合情况。
- **Log**: 点击页面右上角**View Logs**图标，可查看流任务运行日志。
- **Results**: 可通过接口*getAssetsRawDataByTimeRange*来获取输出点*test_C*的数据。