# 3.1.8 Manual for referring to the xhost module methods

<html>
<body>
<h2>get(url, query)</h2>

<h3>Description:</h3>
OpenAPI GET method.<br><br>
<h3>Args:</h3>
<b>url</b>: str. OpenAPI URL.<br>
<b>query</b>: str. OpenAPI query.<br>

<h3>Returns:</h3>
str. responded value.<br>
<br>

<hr>
<h2>put(url, body)</h2>

<h3>Description:</h3>
OpenAPI PUT method.<br><br>
<h3>Args:</h3>
<b>url</b>: str. OpenAPI URL.<br>
<b>body</b>: str. body of the request.<br>

<h3>Returns:</h3>
str. body of the response.<br>
<br>

<hr>
<h2>post(url, body)</h2>

<h3>Description:</h3>
OpenAPI POST method.<br><br>
<h3>Args:</h3>
<b>url</b>: str. OpenAPI URL.<br>
<b>body</b>: str. body of the request.<br>

<h3>Returns:</h3>
str. body of the response.<br>
<br>

<hr>
<h2>hist_print(msg)</h2>

<h3>Description:</h3>
Same as printh() except the user-param/hist_print_level setting is applied.<br><br>
<h3>Args:</h3>
<b>msg</b>: str. message.<br>

<h3>Returns:</h3>
	
<br>

<hr>
<h2>printh(msg)</h2>

<h3>Description:</h3>
print to history log.<br><br>
<h3>Args:</h3>
<b>msg</b>: str. message.<br>

<h3>Returns:</h3>
	
<br>

<hr>
<h2>issue_alarm(task_no, type, code)</h2>

<h3>Description:</h3>
issue error or warning event.<br><br>
<h3>Args:</h3>
<b>task_no</b>: int. task number (0~7).<br>
<b>type</b>: <br>
&nbsp&nbsp <b>'E'</b>: error<br>
&nbsp&nbsp <b>'W'</b>: warning<br>
<b>code</b>: alarm code number.<br>

<h3>Returns:</h3>
	
<br>

<hr>
<h2>issue_notice(task_no, code, msg, delay_sec)</h2>

<h3>Description:</h3>
issue notice event.<br><br>
<h3>Args:</h3>
<b>task_no</b>: int. task number (0~7).<br>
<b>code</b>: int. alarm code number.<br>
<b>msg</b>: str. notice message.<br>
<b>delay_sec</b>: float. time to delay before hiding (sec).<br>

<h3>Returns:</h3>
	
<br>

<hr>
<h2>set_job_state_msg(task_no, msg)</h2>

<h3>Description:</h3>
set the job state message on the teach pendant.<br><br>
<h3>Args:</h3>
<b>task_no</b>: int. task number (0~7).<br>
<b>msg</b>: str. state message to show.<br>

<h3>Returns:</h3>
	
<br>

<hr>
<h2>io_set_so(sig_no, val)</h2>

<h3>Description:</h3>
set the system i/o output bit.<br><br>
<h3>Args:</h3>
<b>sig_no</b>: int. signal number (0~959).<br>
<b>val</b>: int. value to set. (1 or 0)<br>

<h3>Returns:</h3>
&nbsp&nbsp <b>0</b>: ok<br>
&nbsp&nbsp <b>-1</b>: index-range exceeded.<br>

<br>

<hr>
<h2>io_get_in_bit(sigcode)</h2>

<h3>Description:</h3>
get the user i/o input bit using sigcode.<br><br>
<h3>Args:</h3>
<b>sigcode</b>: int. signal-code (e.g. 30017 for fb3.di17)<br>

<h3>Returns:</h3>
signal value, 0 or 1.<br>
<br>

<hr>
<h2>io_set_out_bit(sigcode, val)</h2>

<h3>Description:</h3>
get the user i/o output bit using sigcode.<br><br>
<h3>Args:</h3>
<b>sigcode</b>: int. signal-code (e.g. 30017 for fb3.do17)<br>
<b>val</b>: int. value to set. (1 or 0)<br>

<h3>Returns:</h3>
&nbsp&nbsp <b>0</b>: ok<br>
&nbsp&nbsp <b>-1</b>: index-range exceeded.<br>

<br>

<hr>
<h2>io_set_pulse_by_sigcode(sigcode, onoff, count, on_ms, off_ms, lag_ms, non_update)</h2>

<h3>Description:</h3>
make the i/o pulse output<br><br>
<h3>Args:</h3>
<b>sigcode</b>: int. signal-code (e.g. 30017 for fb3.do17)<br>
<b>onoff</b>: <br>
&nbsp&nbsp <b>1</b>: on-pulse<br>
&nbsp&nbsp <b>0</b>: non-pulsed off (lagged-off)<br>
&nbsp&nbsp <b>-1</b>: off-pulse<br>
<b>count</b>: int. pulse count.<br>
<b>on_ms</b>: int. width of on (msec).<br>
<b>off_ms</b>: int. width of off (msec).<br>
<b>lag_ms</b>: int. width of lag (msec).<br>
<b>non_update</b>: <br>
&nbsp&nbsp <b>1</b>: don't update if the pulse is already registered.<br>
&nbsp&nbsp <b>0</b>: reregister the pulse if the pulse is already registered.<br>

<h3>Returns:</h3>
&nbsp&nbsp <b>0</b>: ok<br>
&nbsp&nbsp <b>-2</b>: already registered (when non_update==2).<br>

<br>

<hr>
<h2>io_assign_set_in_bit(sigcode)</h2>

<h3>Description:</h3>
set sigcode as the assigned input i/o<br><br>
<h3>Args:</h3>
<b>sigcode</b>: int. signal-code (e.g. 30017 for fb3.di17)<br>

<h3>Returns:</h3>
0               ok -1              invalid sigcode<br>
<br>

<hr>
<h2>io_assign_set_out_bit(sigcode)</h2>

<h3>Description:</h3>
set sigcode as the assigned output i/o<br><br>
<h3>Args:</h3>
<b>sigcode</b>: int. signal-code (e.g. 30017 for fb3.do17)<br>

<h3>Returns:</h3>
	0               ok -1              invalid sigcode<br>
<br>

<hr>
<h2>io_set_triggout(task_no, fbname, val, ofs, ax_no, type)</h2>

<h3>Description:</h3>
trigger the output i/o<br><br>
<h3>Args:</h3>
<b>task_no</b>: int. task number (0~7).<br>
<b>fbname</b>: str. output variable name (e.g. fb3.do17, dob3)<br>
<b>val</b>: int. output value.<br>
<b>ofs</b>: int. offset-time (msec), or offset-distance (mm)<br>
<b>ax_no</b>: int. 0(TCP), 1~(axis-number) ; the axis observed when the type is OD<br>
<b>type</b>: <br>
&nbsp&nbsp <b>0x01</b>: OT (time-based)<br>
&nbsp&nbsp <b>0x02</b>: OD (distance-based) ; relative distance from the previous position<br>
&nbsp&nbsp <b>0x04</b>: output even if it is an assigned i/o<br>
&nbsp&nbsp <b>0x10</b>: OX (absolute position of X)<br>
&nbsp&nbsp <b>0x20</b>: OY (absolute position of Y)<br>
&nbsp&nbsp <b>0x30</b>: OZ (absolute position of Z)<br>

<h3>Returns:</h3>
&nbsp&nbsp <b>1</b>: buffer full<br>
&nbsp&nbsp <b>2</b>: complete<br>
&nbsp&nbsp <b>0</b>: ok<br>
&nbsp&nbsp <b>-1</b>: the robot axis is locked. (when, ax_no==0)<br>
&nbsp&nbsp <b>-2</b>: the Nth robot axis is locked<br>
&nbsp&nbsp <b>-3</b>: unsupported coordinate system<br>
&nbsp&nbsp <b>-4</b>: the user coordinate system number is unmatched<br>

<br>

<hr>
<h2>io_n_blocks()</h2>

<h3>Description:</h3>
	
<h3>Args:</h3>
	
<h3>Returns:</h3>
int. The number of i/o blocks<br>
<br>

<hr>
<h2>io_size_block_addr()</h2>

<h3>Description:</h3>
	
<h3>Args:</h3>
	
<h3>Returns:</h3>
int. The number of bits (address-space) in a block.<br>
<br>

<hr>
<h2>io_fbname_from_sigcode(sigcode, is_out)</h2>

<h3>Description:</h3>
gets the fbname from sigcode.<br><br>
<h3>Args:</h3>
<b>sigcode</b>: int. signal-code (e.g. 30017)<br>
<b>is_out</b>: 1               output 0               input<br>

<h3>Returns:</h3>
fbname of the sigcode (e.g. fb3.di17)<br>
<br>

<hr>
<h2>solve_expr_as_string(task_no, expr)</h2>

<h3>Description:</h3>
solve the expression and return the result value.<br><br>
<h3>Args:</h3>
<b>task_no</b>: int. task number (0~7).<br>
<b>expr</b>: str. expression of robot language.<br>

<h3>Returns:</h3>
str. the result value of the expression.<br>
<br>

<hr>
<h2>solve_expr_as_int(task_no, expr)</h2>

<h3>Description:</h3>
solve the expression and return the result value as an integer.<br><br>
<h3>Args:</h3>
<b>task_no</b>: int. task number (0~7).<br>
<b>expr</b>: str. expression of robot language.<br>

<h3>Returns:</h3>
int. the resulting integer value of the expression.<br>
<br>

<hr>
<h2>exec_mode()</h2>

<h3>Description:</h3>
check whether the mode is execution mode.<br>
<h3>Args:</h3>
	
<h3>Returns:</h3>
&nbsp&nbsp <b>True</b>: exec-mode.<br>
&nbsp&nbsp <b>False</b>: not exec-mode.<br>

<br>

<hr>
<h2>cont_mode()</h2>
<h3>Description:</h3>
check whether the mode is continue mode.<br>
<h3>Args:</h3>
	
<h3>Returns:</h3>
&nbsp&nbsp <b>True</b>: cont-mode.<br>
&nbsp&nbsp <b>False</b>: not cont-mode.<br>

<br>

<hr>
<h2>req_to_continue()</h2>

<h3>Description:</h3>
request the host to be in continue mode
<h3>Args:</h3>
	
<h3>Returns:</h3>
	
<br>

<hr>
<h2>set_err_code(code)</h2>

<h3>Description:</h3>
set error code.<br><br>
<h3>Args:</h3>
<b>code</b>: int. error code.<br>

<h3>Returns:</h3>
	
<br>

<hr>
<h2>lang_timer()</h2>

<h3>Description:</h3>
	
<h3>Args:</h3>
	
<h3>Returns:</h3>
int. current value of language-timer.<br>
<br>

<hr>
<h2>set_lang_timer(timeout)</h2>

<h3>Description:</h3>
    set the value to the language-timer.<br><br>
<h3>Args:</h3>
<b>timeout</b>: int. initial value (msec)<br>

<h3>Returns:</h3>
	
<br>

<hr>
<h2>branch_to_addr(addr)</h2>

<h3>Description:</h3>
the branch to the address.<br><br>
<h3>Args:</h3>
<b>addr</b>: str. address to branch.<br>

<h3>Returns:</h3>
	
<br>

<hr>
<h2>abs_path(name)</h2>

<h3>Description:</h3>
get absolute-path in the file-system.<br><br>
<h3>Args:</h3>
<b>name</b>: str. 'home', 'project', 'log', 'jobs', 'vars', 'backup', 'fbrr', 'module', 'apps_main', or 'help'<br>

<h3>Returns:</h3>
absolute-path<br>
<br>

<hr>
<h2>sci_open(port)</h2>

<h3>Description:</h3>
serial port open<br>        Args:<br>        port (int): serial port number<br><br>
<h3>Args:</h3>
	
<h3>Returns:</h3>
&nbsp&nbsp <b>0</b>: OK<br>
&nbsp&nbsp <b>-1</b>: Not OK<br>

<br>

<hr>
<h2>sci_close(port)</h2>

<h3>Description:</h3>
serial port close<br>        Args:<br>        port (int): serial port number<br>        <br>
<h3>Args:</h3>
	
<h3>Returns:</h3>
&nbsp&nbsp <b>0</b>: OK<br>
&nbsp&nbsp <b>-1</b>: already closed.<br>

<br>

<hr>
<h2>sci_send_bytes(port, data)</h2>

<h3>Description:</h3>
serial communication sends byte-type data<br>xhost dbg not supporteds<br>
<h3>Args:</h3>
<b>port (int)</b>: serial port number<br>
<b>data (bytes)</b>: input data<br>

<h3>Returns:</h3>
&nbsp&nbsp <b>0</b>: OK<br>
&nbsp&nbsp <b>-1</b>: Not OK<br>

<br>

<hr>
<h2>sci_recv_bytes(port, len)</h2>

<h3>Description:</h3>
	serial communication receives byte-type data<br>xhost dbg not supported<br><br>
<h3>Args:</h3>
<b>port (int)</b>: serial port number<br>
<b>len (int)</b>: length of data<br>	
<h3>Returns:</h3>
	recieved data (bytes)<br>
<br>

<hr>
<h2>sci_clear_buf(port)</h2>

<h3>Description:</h3>
	clear serial buffer<br><br>
<h3>Args:</h3>
<b>port (int)</b>: serial port number<br>	
<h3>Returns:</h3>
	
<br>

<hr>
<h2>sci_send(port, data)</h2>

<h3>Description:</h3>
serial communication sends string-type data<br><br>
<h3>Args:</h3>
	
<h3>Returns:</h3>
&nbsp&nbsp <b>0</b>: OK<br>
&nbsp&nbsp <b>-1</b>: Not OK<br>

<br>

<hr>
<h2>sci_recv(port)</h2>

<h3>Description:</h3>
serial communication receives string-type data<br><br>
<h3>Args:</h3>
<b>port (int)</b>: serial port number<br>

<h3>Returns:</h3>
&nbsp&nbsp <b>'str'</b>: receive string data<br>

<br>

<hr>
</body>
</html>