# 3.2.3 callback 함수 참조설명서

<table>
  <thead>
    <tr>
      <th style="text-align:left">Python callback 함수들</th>
      <th style="text-align:left">main s/w 내에서 호출되는 시점</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>on_app_init()</td>
      <td>
       자기 진단 후
      </td>
    </tr>
   <tr>
      <td>on_before_self_diagnosis_proc()</td>
      <td>
       자기 진단 전
      </td>
    </tr>
    <tr>
      <td>on_mot_servoerror_detect()</td>
      <td>
       서보 에러 검지
      </td>
    </tr>
     <tr>
      <td>on_system_status_chk_proc()</td>
      <td>시스템 이상 상태 여부 확인 (10ms)</td>
    </tr>
    <tr>
      <td>on_period_low()</td>
      <td>
       최저 우선순위 주기 호출(5ms)
      </td>
    </tr>
   <tr>
      <td>on_motor_on()</td>
      <td>
       모터 on
      </td>
    </tr>
    <tr>
      <td>on_motor_off()</td>
      <td>
       모터 off
      </td>
    </tr>
    <tr>
      <td>on_stop(task_no:int)</td>
      <td>
       정지
      </td>
    </tr>
    <tr>
      <td>on_restart(task_no:int)</td>
      <td>기동</td>
    </tr>
     <tr>
      <td>on_cur_job_selected_by_tp(task_no:int)</td>
      <td>TP에 의한 job program 선택</td>
    </tr>
    <tr>
      <td>init_signal_output_status()</td>
      <td>응용 신호 출력 초기화</td>
    </tr>
    <tr>
      <td>set_ext_io_sig_proc()</td>
      <td>
       할당 신호 처리
      </td>
    </tr>
  </tbody>
</table>