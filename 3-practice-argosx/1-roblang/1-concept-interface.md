# 3.1.1 ArgosX 规格和接口插件

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
       - 它包含一个嵌入式LED灯，可以通过通信请求打开和关闭。<br>
       - 它可以同时测量最多100个工件的位移值，并在响应通信请求时报告。
      </td>
    </tr>
   <tr>
      <td>通信接口</td>
      <td>
       - 机器人控制器和ArgosX硬件通过以太网UDP通信相互通信。<br>
        - ArgosX硬件的IP地址是192.168.1.XX。最后一组数字XX应使用拨码开关设置，机器人一侧应相应地发送UDP请求。<br>
        - ArgosX硬件的端口号固定为54321。然而，在未来的产品中可能会发生更改。<br>
        - 在接收到UDP请求后，ArgosX硬件将向发送者的IP地址发送响应。
      </td>
    </tr>
    <tr>
      <td>可以安装的系统数量</td>
      <td>
       - 在机器人控制器中只能安装一个ArgosX系统。换句话说，ArgosX系统在软件上是唯一实例。
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
      <td>req {workpiece#}<br>
        例如 "req 39"</td>
      <td>请求工件#的位移值<br>工件编号（#）范围从1到100</td>
    </tr>
   <tr>
      <td>机器人 ← ArgosX</td>
      <td></td>
      <td>res ({x}, {y}, {z}, {rx}, {ry}, {rz})<br>
            如果测量失败，字符串“fail”将被传送。<br>
            例如 "res (30, 25.7, 11.9, 31.6, 12.8, -54.6)"<br>
            例如 "fail"</td>
      <td>关于工件#的位移值的响应<br>
        x-rz的值是真实数，单位为mm和deg。</td>
    </tr>
    <tr>
      <td>机器人 → ArgosX</td>
      <td>54321</td>
      <td>light-on</td>
      <td>打开LED灯。</td>
    </tr>
    <tr>
      <td>机器人 → ArgosX</td>
      <td>54321</td>
      <td>light-off</td>
      <td>关闭LED灯。</td>
    </tr>
  </tbody>
</table>

#### ArgosX 接口插件的规格


ArgosX的接口插件将按照以下规格开发。



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
      <td>ArgosX硬件的端口号。<br>（设置应该是可能的，因为未来的产品可能会有更改。）<br>例如 54321</td>
    </tr>
    <tr>
      <td rowspan="4">功能</td>
      <td>init( )</td>
      <td>初始化UDP通信的套接字。</td>
    </tr>
    <tr>
      <td>req({workpiece#})</td>
      <td>请求工件#的结果位移值</td>
    </tr>
    <tr>
      <td>res( )</td>
      <td>在等待响应时接收请求。<br>返回值是基于基坐标系统的位移数组字符串。<br>例如 "[30, 25.7, 11.9, 31.6, 12.8, -54.6, \"base\"]"</td>
    </tr>
    <tr>
      <td>close( )</td>
      <td>关闭UDP通信的套接字。</td>
    </tr>
  </tbody>
</table>

##### 照明功能
- 当机器人处于电机开启状态时，ArgosX LED灯也会亮起。
- 当机器人处于电机关闭状态时，ArgosX LED灯也会熄灭。

##### 错误处理
- 当从ArgosX接收到“fail”时，机器人控制器对应于预设编号的通用I/O输出信号将被打开。


##### 监控
通过在教学挂架上打开ArgosX监控面板，可以看到以下信息。

- IP地址
- 端口#
- 错误输入分配号
- 请求计数
- 响应计数



##### 用户栏
当您在教学挂架上打开ArgosX用户栏时，将提供如下所示的UI。

- 开灯按钮：打开ArgosX LED灯。
- 关灯按钮：关闭ArgosX LED灯。