arm控制API



 void set_muti_move(FLOAT64 *offVal);                                 //设置多轴运动参数



### arm控制器使用



1.设置必要得参数（类比机床参数）

2.程序下载到控制器

3.调用开始函数



## 一些API

```cpp
//似乎是急停 但是下面有个差不多的
MOXA_dg_move_quickstop_func(des_net_addr,motor_id,error_message);
MOXA_set_all_closed_loop_axis_position2_func(des_net_addr,motor_id,msgInfo.actpos_ptr,error_message);
//获取伺服使能状态
MOXA_get_AxisEnableReg_func(des_net_addr,motor_id,AxisEnableReg,error_message);
//程序执行次数
MOXA_get_count_func(des_net_addr,motor_id,msgInfo.doneCnt,error_message);
//伺服使能
MOXA_servo_enable_func(des_net_addr,motor_id,i,true,error_message);

//似乎是清除一些标记 状态、错误信息的作用
MOXA_clear_autorun_command_buf_func(des_net_addr,motor_id,0,error_message);
MOXA_clear_autorun_command_buf_func(des_net_addr,motor_id,1,error_message);
MOXA_clear_errormsg_func(des_net_addr,motor_id,error_message);

//获取当前的偏置类型 G5x
MOXA_get_G54ToG59_params_func(des_net_addr,motor_id,1,val,msgInfo.inuseG5x,error_message);

//获取缓冲空间大小
MOXA_get_num_of_empty_autorun_command_inbuf_func(des_net_addr,motor_id,0,retNum,error_message);

//不明确
MOXA_clear_system_timer_func(des_net_addr,motor_id,0,error_message);

//回零状态
MOXA_HomingProcess_check_proc_state_func(des_net_addr,motor_id,msgInfo.HomingProcess_running_flag,error_message);
//获取主轴DA数据 倍率
MOXA_get_masterAxis_dacrateanddata_func(des_net_addr,motor_id,msgInfo.masterAxis_DacRate,msgInfo.masterAxis_DacData,error_message);
//执行时间
MOXA_get_system_timer_func(des_net_addr,motor_id,0,msgInfo.doTimes,error_message);
//获取GPIO输入和输出状态
MOXA_gpio_in2_func(des_net_addr,motor_id,0,m_gpio_in_state,error_message);
MOXA_get_IOoutreg_func(des_net_addr,motor_id,0,gpio_out_state,error_message);
//切换运行状态
MOXA_switch_run_mode_func(des_net_addr,motor_id,MOXA_RUNNING_MODE_TEST,error_message);

//命令下载（非阻塞式）
MOXA_download_autorun_command_CreatProc_func(des_net_addr,motor_id,0,nSize-index,
                                                             autorun_command_index_ptr,
                                                             autorun_command_sub_index01_ptr,
                                                             autorun_command_id_ptr,
                                                             autorun_command_data_ptr,
                                                             error_message);

//命令下载（阻塞式）
MOXA_download_autorun_command_func(des_net_addr,motor_id,0,nSize-index,
                                                                autorun_command_index_ptr,
                                                                autorun_command_sub_index01_ptr,
                                                                autorun_command_id_ptr,
                                                                autorun_command_data_ptr,
                                                                error_message);

//设置偏置
MOXA_set_G54ToG59_params_func(des_net_addr,motor_id,nG5x,G54toG59_params_ptr,error_message);
```

