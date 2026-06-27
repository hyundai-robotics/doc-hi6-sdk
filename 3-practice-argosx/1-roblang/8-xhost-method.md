#### 3.1.8 手动引用 xhost 模块方法
<hr>

##### get(url, query)
* 描述:
  OpenAPI GET 方法。
* 参数:  
  url (str): API 端点路径。ex. "project/rgen"  
             **不包括** 基本 URL (http://192.168.1.150:8888/)。仅提供后面的路径。
  query: str. OpenAPI 查询。
* 返回:
  str. 响应值。

<hr>

##### put(url, body)
* 描述:
  OpenAPI PUT 方法。
* 参数:
  url (str): API 端点路径。ex. "project/rgen"  
             **不包括** 基本 URL (http://192.168.1.150:8888/)。仅提供后面的路径。
  body: str. 请求体。
* 返回:
  str. 响应体。

<hr>

##### post(url, body)
* 描述:
  OpenAPI POST 方法。
* 参数:
  url (str): API 端点路径。ex. "project/rgen"  
             **不包括** 基本 URL (http://192.168.1.150:8888/)。仅提供后面的路径。
  body: str. 请求体。
* 返回:
  str. 响应体。

<hr>

##### hist_print(msg)
* 描述:
  同 printh()，只应用用户参数/hist_print_level 设置。
* 参数:
  msg: str. 消息。
* 返回:
  None

<hr>

##### printh(msg)
* 描述:
  打印到历史日志。
* 参数:
  msg: str. 消息。
* 返回:
  None

<hr>

##### issue_alarm(task_no, type, code)
* 描述:
  发布错误或警告事件。
* 参数:
  task_no: int. 任务编号 (0~7)
  type:
    'E': 错误
    'W': 警告
  code: int. 警报代码编号
* 返回:
  None

<hr>

##### issue_notice(task_no, code, msg, delay_sec)
* 描述:
  发布通知事件。
* 参数:
  task_no: int. 任务编号 (0~7)
  code: int. 警报代码编号
  msg: str. 通知消息
  delay_sec: float. 隐藏前的延迟时间 (秒)
* 返回:
  None

<hr>

##### set_job_state_msg(task_no, msg)
* 描述:
  在教学显示器上设置作业状态消息。
* 参数:
  task_no: int. 任务编号 (0~7)
  msg: str. 要显示的状态消息
* 返回:
  None

<hr>

##### io_set_so(sig_no, val)
* 描述:
  设置系统 I/O 输出位。
* 参数:
  sig_no: int. 信号编号 (0~959)
  val: int. (1 或 0)
* 返回:
  0: ok
  -1: 索引范围超出

<hr>

##### io_get_in_bit(sigcode)
* 描述:
  根据信号代码获取用户 I/O 输入位。
* 参数:
  sigcode: int. 信号代码 (例如 30017 对 fb3.di17)
* 返回:
  0 或 1

<hr>

##### io_set_out_bit(sigcode, val)
* 描述:
  根据信号代码设置用户 I/O 输出位。
* 参数:
  sigcode: int. 信号代码
  val: int. (1 或 0)
* 返回:
  0: ok
  -1: 索引范围超出

<hr>

##### io_set_pulse_by_sigcode(sigcode, onoff, count, on_ms, off_ms, lag_ms, non_update)
* 描述:
  生成 I/O 脉冲输出
* 参数:
  sigcode: int
  onoff:
    1: 开脉冲
    0: 非脉冲关 (延迟关)
    -1: 关脉冲
  count: int. 脉冲计数
  on_ms: int. 开的宽度 (毫秒)
  off_ms: int. 关的宽度 (毫秒)
  lag_ms: int. 延迟的宽度 (毫秒)
  non_update:
    1: 如果已经注册，则不更新
    0: 重新注册脉冲
* 返回:
  0: ok
  -2: 已经注册

<hr>

##### io_assign_set_in_bit(sigcode)
* 描述:
  设置信号代码为分配的输入 I/O
* 参数:
  sigcode: int
* 返回:
  0: ok
  -1: 无效的信号代码

<hr>

##### io_assign_set_out_bit(sigcode)
* 描述:
  设置信号代码为分配的输出 I/O
* 参数:
  sigcode: int
* 返回:
  0: ok
  -1: 无效的信号代码

<hr>

##### io_set_triggout(task_no, fbname, val, ofs, ax_no, type)
* 描述:
  触发输出 I/O
* 参数:
  task_no: int
  fbname: str. (例如 fb3.do17, dob3)
  val: int
  ofs: int. 偏移时间 (毫秒) 或 偏移距离 (毫米)
  ax_no: int. 0(TCP), 1~ 轴编号
  type:
    0x01: OT (基于时间)
    0x02: OD (基于距离)
    0x04: 强制输出
    0x10: OX
    0x20: OY
    0x30: OZ
* 返回:
  1: 缓存满
  2: 完成
  0: ok
  -1 ~ -4: 错误

<hr>

##### io_n_blocks()
* 描述:
  获取 I/O 块的数量
* 返回:
  int

<hr>

##### io_size_block_addr()
* 描述:
  获取块中的位 (地址空间)
* 返回:
  int

<hr>

##### io_fbname_from_sigcode(sigcode, is_out)
* 描述:
  从信号代码获取 fbname
* 参数:
  sigcode: int
  is_out: 1 输出 / 0 输入
* 返回:
  fbname 字符串

<hr>

##### solve_expr_as_string(task_no, expr)
* 描述:
  求解表达式并返回字符串结果
* 参数:
  task_no: int
  expr: str
* 返回:
  str

<hr>

##### solve_expr_as_int(task_no, expr)
* 描述:
  求解表达式并返回整数结果
* 参数:
  task_no: int
  expr: str
* 返回:
  int

<hr>

##### exec_mode()
* 描述:
  检查执行模式
* 返回:
  True 或 False

<hr>

##### cont_mode()
* 描述:
  检查连续模式
* 返回:
  True 或 False

<hr>

##### req_to_continue()
* 描述:
  请求主机进入连续模式
* 返回:
  None

<hr>

##### set_err_code(code)
* 描述:
  设置错误代码
* 参数:
  code: int
* 返回:
  None

<hr>

##### lang_timer()
* 描述:
  获取语言计时器值
* 返回:
  int (毫秒)

<hr>

##### set_lang_timer(timeout)
* 描述:
  设置语言计时器值
* 参数:
  timeout: int (毫秒)
* 返回:
  None

<hr>

##### branch_to_addr(addr)
* 描述:
  跳转到地址
* 参数:
  addr: str
* 返回:
  None

<hr>

##### abs_path(name)
* 描述:
  获取文件系统中的绝对路径
* 参数:
  name: home, project, log, jobs, vars, backup, fbrr, module, apps_main, help
* 返回:
  绝对路径字符串

<hr>

##### sci_open(port)
* 描述:
  打开串口
* 参数:
  port: int
* 返回:
  0: OK
  -1: 不 OK

<hr>

##### sci_close(port)
* 描述:
  关闭串口
* 参数:
  port: int
* 返回:
  0: OK
  -1: 已经关闭

<hr>

##### sci_send_bytes(port, data)
* 描述:
  串口发送字节数据
* 参数:
  port: int
  data: bytes
* 返回:
  0: OK
  -1: 不 OK

<hr>

##### sci_recv_bytes(port, len)
* 描述:
  串口接收字节数据
* 参数:
  port: int
  len: int
* 返回:
  bytes

<hr>

##### sci_clear_buf(port)
* 描述:
  清除串口缓冲区
* 参数:
  port: int
* 返回:
  None

<hr>

##### sci_send(port, data)
* 描述:
  串口发送字符串数据
* 返回:
  0: OK
  -1: 不 OK

<hr>

##### sci_recv(port)
* 描述:
  串口接收字符串数据
* 参数:
  port: int
* 返回:
  str

<hr>