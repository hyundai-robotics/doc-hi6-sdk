# 3.2.3 Manual for referring to the callback functions

<table>
  <thead>
    <tr>
      <th style="text-align:left">Python callback functions</th>
      <th style="text-align:left">Point in time when calling occurs inside the main software</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>on_app_init()</td>
      <td>
       After self-diagnosis
      </td>
    </tr>
   <tr>
      <td>on_before_self_diagnosis_proc()</td>
      <td>
       Before self-diagnosis
      </td>
    </tr>
    <tr>
      <td>on_mot_servoerror_detect()</td>
      <td>
       When detecting a servo error
      </td>
    </tr>
    <tr>
      <td>on_mot_on_ready_check_remote_auto()</td>
      <td>
       Motor on ready, remote/auto
      </td>
    </tr>
    <tr>
      <td>on_entry_teach_mode()</td>
      <td>When entering teaching mode</td>
    </tr>
    <tr>
      <td>on_entry_auto_mode()</td>
      <td>
        When entering auto mode
      </td>
    </tr>
     <tr>
      <td>on_system_status_chk_proc()</td>
      <td>When checking the system for any abnormalities (10 ms)</td>
    </tr>
    <tr>
      <td>on_period_low()</td>
      <td>
       When calling based on the lowest priority cycle (5 ms)
      </td>
    </tr>
   <tr>
      <td>on_motor_on()</td>
      <td>
       Motor on
      </td>
    </tr>
    <tr>
      <td>on_motor_off()</td>
      <td>
       Motor off
      </td>
    </tr>
    <tr>
      <td>on_stop(task_no:int)</td>
      <td>
       When stopping
      </td>
    </tr>
    <tr>
      <td>on_restart(task_no:int)</td>
      <td>When starting</td>
    </tr>
    <tr>
      <td>on_reset0()</td>
      <td>
      reset 0	
      </td>
    </tr>
     <tr>
      <td>on_cur_job_selected_by_tp(task_no:int)</td>
      <td>When the TP selects the job program.</td>
    </tr>
    <tr>
      <td>on_step_func_no_change_for_clear(task_no:int,..)</td>
      <td>
       When changing the program counter
      </td>
    </tr>
    <tr>
      <td>on_job_end(task_no:int)</td>
      <td> Executing the job end command
      </td>
    </tr>
    <tr>
      <td>init_signal_output_status()</td>
      <td>When initializing the application signal output</td>
    </tr>
    <tr>
      <td>set_ext_io_sig_proc()</td>
      <td>
       When handling assigned signals
      </td>
    </tr>
    <tr>
      <td>is_assigned_input(sigcode:int) /</br>
      is_assigned_output(sigcode:int)</td>
      <td>When returning whether sigcode is assigned for the input/output signals</td>
    </tr>
    <tr>
      <td>update_input_assign_info() / </br>
      update_output_assign_info()</td>
      <td>When setting up the look-up table for the host's assigned input/output signals</td>
    </tr>
  </tbody>
</table>