##  常用API

测试-为每个命令确定好命令码

```cpp
modbus_t *mb;
int ret;
//创建一个modbus-rtu对象，指定串口号，波特率，校验位，数据位，停止位
//成功返回指针，否则返回NULL, 会调用malloc申请内存
mb = modbus_new_rtu("/dev/ttySP1", 115200, 'N', 8, 1);    //linux
mb = modbus_new_rtu("COM1", 115200, 'N', 8, 1);        //windows
//创建modbus-tcp对象，指定IP地址和端口号
mb = modbus_new_tcp("127.0.0.1", 502);    //TCP/IP

//设置从机地址，成功返回0, 否则返回-1
ret = modbus_set_slave(mb, slave);

//连接Modbus主机，成功返回0, 否则返回-1
ret = modbus_connect(mb);

//设置响应超时时间1s,200ms
ret = modbus_set_response_timeout(mb, 1, 200000);

//读取寄存器数据,起始地址2, 数量5, 保存到table数组中
//成功返回5, 否则返回-1
uint16_t *table;
ret = modbus_read_registers(mb, 2, 5, table); //命令码0x03

//modbus设备关闭和释放内存
modbus_close(mb);
modbus_free(mb);

//写单个寄存器, 地址2写入56, 成功返回1，否则返回-1
ret = modbus_write_register(mb, 2, 56); //命令码0x06

//写多个寄存器, 地址12起始，写入5个数据，成功返回5，否则返回-1
uint16_t table[5] = {11, 22, 33, 44, 55};
ret = modbus_write_registers(mb, 12, 5, table); //命令码0x10

//写单个线圈，线圈地址写入TRUE,成功返回1，否则返回-1
ret = modbus_write_bit(mb, 11, TRUE); //命令码0x05

//查看错误信息
char *err_str;
err_str = modbus_strerror(errno);
```

## 使用流程

创建

```cpp
	modbus_t *ctx;
    // modbus_new_tcp
    ctx =  modbus_new_rtu("COM1",9600, 'N', 8, 1);
```

connect

```cpp
modbus_connect(ctx)
```

设置从站

```cpp
modbus_set_slave(ctx,1);
```

```cpp
// 步骤一：创建modbus RTU
    modbus_t *pModbus = 0;
    pModbus = modbus_new_rtu("com5",
                             115200,
                             'E',
                             8,
                             1);
    if(!pModbus)
    {
        LOG << "Failed to modbus_new_rtu";
        return false;
    }
    LOG << "Succeed to modbus_new_rtu";
    // 步骤二: 485 RTU 模式
    modbus_rtu_set_serial_mode(pModbus, MODBUS_RTU_RS485);
    // 步骤三: 设置从机站号 1
    modbus_set_slave(pModbus, 1);
    // 步骤四：设置超时时间 100 ms
    modbus_set_response_timeout(pModbus, 0, 100 * 1000);
    // 步骤五: 连接 (注意：经过测试，只是485和232只是打开串口，并未交互）
    int ret = modbus_connect(pModbus);
    if(ret)
    {
        LOG << "Failed to modbus_connect, ret =" << ret;
        return false;
    }
    LOG << "Succeed to modbus_connect, ret =" << ret;
    // 步骤六：读取线圈
    uint8_t buffer8t[10] = {0x00};
    ret = modbus_read_bits(pModbus, 25, 10, buffer8t);
    if(ret <= 0)
    {
        LOG << "Failed to modbus_read_registers, ret =" << ret;
        return false;
    }
    LOG << "Succeed to modbus_read_registers, ret =" << ret;
    // 步骤七：打印返回
    for(int index = 0; index < 10; index++)
    {
        LOG << QString("%1").arg(buffer8t[index]);
    }
    // 步骤八：读取寄存器
    uint16_t buffer16t[10] = {0x00};
    ret = modbus_read_registers(pModbus, 95, 10, buffer16t);

    if(ret <= 0)
    {
        LOG << "Failed to modbus_read_registers, ret =" << ret;
        return false;
    }
    LOG << "Succeed to modbus_read_registers, ret =" << ret;
    // 步骤九：打印返回
    for(int index = 0; index < 10; index++)
    {
        LOG << QString("%1").arg(buffer16t[index]);
    }

    // 步骤十：读取寄存器
    memset(buffer16t, 0, 10);
    ret = modbus_read_input_registers(pModbus, 100, 1, buffer16t);
    if(ret <= 0)
    {
        LOG << "Failed to modbus_read_registers, ret =" << ret;
        return false;
    }
    LOG << "Succeed to modbus_read_input_registers, ret =" << ret;
    // 步骤十一：打印返回
    for(int index = 0; index < 10; index++)
    {
        LOG << QString("%1").arg(buffer16t[index]);
    }
    return true;
}

```

```cpp
MODBUS_FC_READ_COILS (0x01) - 读取线圈状态。请求设备返回其当前状态为"on"或"off"的线圈列表。
MODBUS_FC_READ_DISCRETE_INPUTS (0x02) - 读取离散输入状态。请求设备返回其当前状态为"on"或"off"的离散输入列表。
MODBUS_FC_READ_HOLDING_REGISTERS (0x03) - 读取保持寄存器。请求设备返回其当前保持寄存器的值。
MODBUS_FC_READ_INPUT_REGISTERS (0x04) - 读取输入寄存器。请求设备返回其当前输入寄存器的值。
MODBUS_FC_WRITE_SINGLE_COIL (0x05) - 写入单个线圈。请求设备将特定线圈设置为"on"或"off"。
MODBUS_FC_WRITE_SINGLE_REGISTER (0x06) - 写入单个寄存器。请求设备设置特定寄存器的值为请求的值。
MODBUS_FC_READ_EXCEPTION_STATUS (0x07) - 读取异常状态。请求设备返回其当前异常状态（如果有）。
MODBUS_FC_WRITE_MULTIPLE_COILS (0x0F) - 写入多个线圈。请求设备设置多个线圈的状态。
MODBUS_FC_WRITE_MULTIPLE_REGISTERS (0x10) - 写入多个寄存器。请求设备设置多个寄存器的值。
MODBUS_FC_REPORT_SLAVE_ID (0x11) - 报告从设备ID。请求设备返回其从设备ID。
MODBUS_FC_MASK_WRITE_REGISTER (0x16) - 掩码写入寄存器。请求设备将特定寄存器的值设置为一个新的值，但只有那些位与掩码中相应位为"1"的位会被修改。
MODBUS_FC_WRITE_AND_READ_REGisters (0x17) - 写入和读取寄存器。请求设备首先写入一个值，然后读取另一个设备的寄存器，并将结果返回给发送请求的设备。
```

构造原始请求

```cpp
	//注意 在发送前依旧需要设置从站 即使这个请求中已经有写从站了
	uint8_t raw_req[] = { 0x01, MODBUS_FC_READ_HOLDING_REGISTERS, 0x00, 0x01, 0x0, 0x05 };
	//从站地址-功能码-起始地址-个数
	// 发送原始请求  
	rc = modbus_send_raw_request(ctx, raw_req, req_length);
	if (rc == -1) {
		printf("发送请求失败\n");
		// 处理发送错误...  
		return ;
	}
```

