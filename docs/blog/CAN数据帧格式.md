---
title: CAN数据帧格式
createTime: 2025/12/02 09:24:32
permalink: /blog/yjfcrt6t/
---
### CAN数据帧类型

CAN协议定义了四种类型的数据帧：

1. **数据帧（Data Frame）**：传输实际数据。
2. **远程帧（Remote Frame）**：请求传输数据。
3. **错误帧（Error Frame）**：用于报告总线错误。
4. **过载帧（Overload Frame）**：用于请求额外的时间以处理数据。

### 数据帧格式

#### 标准帧格式（11位标识符）

标准帧格式又称为基本帧格式，使用11位的标识符。其数据帧结构如下：

1. **起始位（SOF, Start of Frame）**：1位，标志帧的开始。
2. **标识符（Identifier）**：11位，用于标识消息的优先级。
3. **控制位（RTR, Remote Transmission Request）**：1位，0表示数据帧，1表示远程帧。
4. **控制字段（IDE, Identifier Extension）**：1位，0表示标准帧，1表示扩展帧。
5. **保留位（r0）**：1位，保留为0。
6. **数据长度码（DLC, Data Length Code）**：4位，表示数据字段的字节数（0到8字节）。
7. **数据字段（Data Field）**：0到8字节，实际传输的数据。
8. **CRC字段（CRC, Cyclic Redundancy Check）**：15位，用于错误检测。
9. **CRC定界符（CRC Delimiter）**：1位，固定为显性位（1）。
10. **应答字段（ACK, Acknowledge Field）**：
    - **应答槽（ACK Slot）**：1位，由接收节点驱动为显性位（0）表示接收成功。
    - **应答定界符（ACK Delimiter）**：1位，固定为显性位（1）。
11. **结束位（EOF, End of Frame）**：7位，标志帧的结束。
12. **间隔位（IFS, Interframe Space）**：3位，帧间隔时间。


| SOF | Identifier     | RTR  | IDE  | r0   | DLC  | Data Field       | CRC                 | CRC Delimiter | ACK Slot | ACK Delimiter | EOF      | IFS  |  
| --- |  ---           |  --- |  --- |  --- |  --- |  ---             |  ---                |  ---          |  ---     |  ---          |  ---     |  --- |  
| 1   | 111 1111 1111  | 0    | 0    | 0    | 1000 | 0000 0000 *长度 | 000 0000 0000 0000  | 1             | 1        | 1111111       | 111 1111 | 111  |


#### 扩展帧格式（29位标识符）

扩展帧格式使用29位的标识符，其数据帧结构如下：

1. **起始位（SOF, Start of Frame）**：1位，标志帧的开始。
2. **标识符（Identifier）**：11位，基本部分。
3. **扩展标识符（SRR, Substitute Remote Request）**：1位，替代远程请求位。
4. **标识符扩展位（IDE, Identifier Extension）**：1位，1表示扩展帧。
5. **扩展标识符（Extended Identifier）**：18位，扩展部分。
6. **控制字段（RTR, Remote Transmission Request）**：1位，0表示数据帧，1表示远程帧。
7. **保留位（r1, r0）**：2位，保留为0。
8. **数据长度码（DLC, Data Length Code）**：4位，表示数据字段的字节数（0到8字节）。
9. **数据字段（Data Field）**：0到8字节，实际传输的数据。
10. **CRC字段（CRC, Cyclic Redundancy Check）**：15位，用于错误检测。
11. **CRC定界符（CRC Delimiter）**：1位，固定为显性位（1）。
12. **应答字段（ACK, Acknowledge Field）**：
    - **应答槽（ACK Slot）**：1位，由接收节点驱动为显性位（0）表示接收成功。
    - **应答定界符（ACK Delimiter）**：1位，固定为显性位（1）。
13. **结束位（EOF, End of Frame）**：7位，标志帧的结束。
14. **间隔位（IFS, Interframe Space）**：3位，帧间隔时间。



