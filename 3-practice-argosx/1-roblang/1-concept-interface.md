# 3.1.1 ArgosX 的规格和接口插件

#### ArgosX 视觉系统的规格


##### 基本规格
<table>
  <thead>
    <tr>
      <th style="text-align:left">项目</th>
      <th style="text-align:left">描述</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>功能</td>
      <td>
       - 它包含一个嵌入式 LED 灯，可以通过通信请求打开和关闭。<br>
       - 它可以同时测量最多 100 个工件的位移值，并对通信请求做出响应。
      </td>
    </tr>
   <tr>
      <td>通信接口</td>
      <td>
       - 机器人控制器和 ArgosX 硬件通过以太网 UDP 通信相互通信。<br>
        - ArgosX 硬件的 IP 地址为 192.168.1.XX。最后一组数字 XX 应通过拨码开关设置，机器人端应相应发送 UDP 请求。<br>
        - ArgosX 硬件的端口号固定为 54321。不过，它可能在将来的产品中发生变化。<br>
        - 在收到 UDP 请求后，ArgosX 硬件将向发送者的 IP 地址发送响应。
      </td>
    </tr>
    <tr>
      <td>可以安装的系统数量</td>
      <td>
       - 机器人控制器中只能安装一个 ArgosX 系统。换句话说，ArgosX 系统在软件方面是单实例的。
      </td>
    </tr>
  </tbody>
</table>

##### 协议
<table>
  <thead>
    <tr>
      <th style="text-align:left">传输方向</th>
      <th style="text-align:left">端口#</th>
      <th style="text-align:left">语法和示例</th>
      <th style="text-align:left">含义</th>
    </tr>
  </thead>
  <tbody>
<tr>
      <td>机器人 → ArgosX</td>
      <td>54321</td>
      <td>请求 {工件#}<br>
        例如 "req 39"</td>
      <td>请求工件 # 的位移值<br>工件编号 (#) 范围从 1 到 100 </td>
    </tr>
   <tr>
      <td>机器人 ← ArgosX</td>
      <td></td>
      <td>响应 ({x}, {y}, {z}, {rx}, {ry}, {rz})<br>
            如果测量失败，字符串 "fail" 将被传输。<br>
            例如 "res (30, 25.7, 11.9, 31.6, 12.8, -54.6)"<br>
            例如 "fail"</td>
      <td>关于工件 # 的位移值的响应<br>
        x-rz 的值为实数，其单位为 mm 和 deg。</td>
    </tr>
    <tr>
      <td>机器人 → ArgosX</td>
      <td>54321</td>
      <td>灯光开</td>
      <td>打开 LED 灯。</td>
    </tr>
    <tr>
      <td>机器人 → ArgosX</td>
      <td>54321</td>
      <td>灯光关</td>
      <td>关闭 LED 灯。</td>
    </tr>
  </tbody>
</table>

#### ArgosX 接口插件的规格


ArgosX 的接口插件将遵循以下规格。



##### 机器人语言
<table>
  <thead>
    <tr>
      <th style="text-align:left">项目</th>
      <th style="text-align:left">语法</th>
      <th style="text-align:left">描述</th>
    </tr>
  </thead>
  <tbody>
    <tr>
<td>模块</td>
<td>argosx</td>
<td></td>
</tr>
<tr>
<td rowspan="2">属性</td>
<td>ip_addr</td>
<td>ArgosX硬件的IP地址字符串（可以设置。）<br>例如 "192.168.1.44"</td>
</tr>
<tr>
<td>port</td>
<td>ArgosX硬件的端口号。<br>（设置应该是可能的，因为未来产品可能会有变更。）<br>例如 54321</td>
</tr>
<tr>
<td rowspan="4">功能</td>
<td>init( )</td>
<td>初始化用于UDP通信的套接字。</td>
</tr>
<tr>
<td>req({workpiece#})</td>
<td>请求工件#的结果位移值</td>
</tr>
<tr>
<td>res( )</td>
<td>在等待响应时接收请求。<br>返回值是基于基坐标系的位移数组字符串。<br>例如 "[30, 25.7, 11.9, 31.6, 12.8, -54.6, \"base\"]"</td>
</tr>
<tr>
<td>close( )</td>
<td>关闭用于UDP通信的套接字。</td>
</tr>
</tbody>
</table>

##### 照明功能
- 当机器人处于电机开启状态时，ArgosX LED灯也将被打开。
- 当机器人处于电机关闭状态时，ArgosX LED灯也将被关闭。

##### 错误处理
- 当从ArgosX接收到"fail"时，相应预设编号的机器人控制器的通用I/O输出信号将被激活。

##### 监控
通过在教学挂件上打开ArgosX监控面板，您可以看到以下信息。

- IP地址
- 端口号
- 错误输入分配编号
- 请求计数
- 响应计数
##### 用户栏
当您在教学挂件上打开ArgosX用户栏时，将提供如下所示的UI。

- 开灯按钮：开启ArgosX LED灯。
- 关灯按钮：关闭ArgosX LED灯。