#### 3.1.8 Manual for referring to the xhost module methods
<hr>

##### get(url, query)
* Description:
  OpenAPI GET method.
* Args:  
  url (str): The API endpoint path. ex. "project/rgen"  
             **Do not include** the base URL (http://192.168.1.150:8888/). Only provide the path following it.
  query: str. OpenAPI query.
* Returns:
  str. responded value.

<hr>

##### put(url, body)
* Description:
  OpenAPI PUT method.
* Args:
  url (str): The API endpoint path. ex. "project/rgen"  
             **Do not include** the base URL (http://192.168.1.150:8888/). Only provide the path following it.
  body: str. body of the request.
* Returns:
  str. body of the response.

<hr>

##### post(url, body)
* Description:
  OpenAPI POST method.
* Args:
  url (str): The API endpoint path. ex. "project/rgen"  
             **Do not include** the base URL (http://192.168.1.150:8888/). Only provide the path following it.
  body: str. body of the request.
* Returns:
  str. body of the response.

<hr>

##### hist_print(msg)
* Description:
  Same as printh() except user-param/hist_print_level setting is applied.
* Args:
  msg: str. message.
* Returns:
  None

<hr>

##### printh(msg)
* Description:
  print to history log.
* Args:
  msg: str. message.
* Returns:
  None

<hr>

##### issue_alarm(task_no, type, code)
* Description:
  issue error or warning event.
* Args:
  task_no: int. task number (0~7)
  type:
    'E': error
    'W': warning
  code: int. alarm code number
* Returns:
  None

<hr>

##### issue_notice(task_no, code, msg, delay_sec)
* Description:
  issue notice event.
* Args:
  task_no: int. task number (0~7)
  code: int. alarm code number
  msg: str. notice message
  delay_sec: float. time to delay before hide (sec)
* Returns:
  None

<hr>

##### set_job_state_msg(task_no, msg)
* Description:
  set job state message on teach pendant.
* Args:
  task_no: int. task number (0~7)
  msg: str. state message to show
* Returns:
  None

<hr>

##### io_set_so(sig_no, val)
* Description:
  set system i/o output bit.
* Args:
  sig_no: int. signal number (0~959)
  val: int. (1 or 0)
* Returns:
  0: ok
  -1: index-range exceeded

<hr>

##### io_get_in_bit(sigcode)
* Description:
  get user i/o input bit by sigcode.
* Args:
  sigcode: int. signal-code (e.g. 30017 for fb3.di17)
* Returns:
  0 or 1

<hr>

##### io_set_out_bit(sigcode, val)
* Description:
  set user i/o output bit by sigcode.
* Args:
  sigcode: int. signal-code
  val: int. (1 or 0)
* Returns:
  0: ok
  -1: index-range exceeded

<hr>

##### io_set_pulse_by_sigcode(sigcode, onoff, count, on_ms, off_ms, lag_ms, non_update)
* Description:
  make i/o pulse output
* Args:
  sigcode: int
  onoff:
    1: on-pulse
    0: non-pulsed off (lagged-off)
    -1: off-pulse
  count: int. pulse count
  on_ms: int. width of on (msec)
  off_ms: int. width of off (msec)
  lag_ms: int. width of lag (msec)
  non_update:
    1: don't update if already registered
    0: re-register pulse
* Returns:
  0: ok
  -2: already registered

<hr>

##### io_assign_set_in_bit(sigcode)
* Description:
  set sigcode as assigned input i/o
* Args:
  sigcode: int
* Returns:
  0: ok
  -1: invalid sigcode

<hr>

##### io_assign_set_out_bit(sigcode)
* Description:
  set sigcode as assigned output i/o
* Args:
  sigcode: int
* Returns:
  0: ok
  -1: invalid sigcode

<hr>

##### io_set_triggout(task_no, fbname, val, ofs, ax_no, type)
* Description:
  trigger-out output i/o
* Args:
  task_no: int
  fbname: str. (e.g. fb3.do17, dob3)
  val: int
  ofs: int. offset-time (msec) or offset-distance (mm)
  ax_no: int. 0(TCP), 1~ axis number
  type:
    0x01: OT (time-based)
    0x02: OD (distance-based)
    0x04: force output
    0x10: OX
    0x20: OY
    0x30: OZ
* Returns:
  1: buffer full
  2: complete
  0: ok
  -1 ~ -4: error

<hr>

##### io_n_blocks()
* Description:
  get number of i/o blocks
* Returns:
  int

<hr>

##### io_size_block_addr()
* Description:
  get bits (address-space) in a block
* Returns:
  int

<hr>

##### io_fbname_from_sigcode(sigcode, is_out)
* Description:
  get fbname from sigcode
* Args:
  sigcode: int
  is_out: 1 output / 0 input
* Returns:
  fbname string

<hr>

##### solve_expr_as_string(task_no, expr)
* Description:
  solve expression and return string result
* Args:
  task_no: int
  expr: str
* Returns:
  str

<hr>

##### solve_expr_as_int(task_no, expr)
* Description:
  solve expression and return integer result
* Args:
  task_no: int
  expr: str
* Returns:
  int

<hr>

##### exec_mode()
* Description:
  check execute-mode
* Returns:
  True or False

<hr>

##### cont_mode()
* Description:
  check continue-mode
* Returns:
  True or False

<hr>

##### req_to_continue()
* Description:
  request host to be continue-mode
* Returns:
  None

<hr>

##### set_err_code(code)
* Description:
  set error code
* Args:
  code: int
* Returns:
  None

<hr>

##### lang_timer()
* Description:
  get language timer value
* Returns:
  int (msec)

<hr>

##### set_lang_timer(timeout)
* Description:
  set value to language-timer
* Args:
  timeout: int (msec)
* Returns:
  None

<hr>

##### branch_to_addr(addr)
* Description:
  branch to the address
* Args:
  addr: str
* Returns:
  None

<hr>

##### abs_path(name)
* Description:
  get absolute-path in the file-system
* Args:
  name: home, project, log, jobs, vars, backup, fbrr, module, apps_main, help
* Returns:
  absolute-path string

<hr>

##### sci_open(port)
* Description:
  serial port open
* Args:
  port: int
* Returns:
  0: OK
  -1: Not OK

<hr>

##### sci_close(port)
* Description:
  serial port close
* Args:
  port: int
* Returns:
  0: OK
  -1: already closed

<hr>

##### sci_send_bytes(port, data)
* Description:
  serial send bytes data
* Args:
  port: int
  data: bytes
* Returns:
  0: OK
  -1: Not OK

<hr>

##### sci_recv_bytes(port, len)
* Description:
  serial receive bytes data
* Args:
  port: int
  len: int
* Returns:
  bytes

<hr>

##### sci_clear_buf(port)
* Description:
  clear serial buffer
* Args:
  port: int
* Returns:
  None

<hr>

##### sci_send(port, data)
* Description:
  serial send string data
* Returns:
  0: OK
  -1: Not OK

<hr>

##### sci_recv(port)
* Description:
  serial receive string data
* Args:
  port: int
* Returns:
  str

<hr>
