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
      <td>on_cur_job_selected_by_tp(task_no:int)</td>
      <td>When the TP selects the job program.</td>
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
  </tbody>
</table>