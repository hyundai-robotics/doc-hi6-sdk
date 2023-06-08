# 3.1.1 Specifications of ArgosX and interface plug-ins

## Specifications of the ArgosX vision system


### Basic specifications
<table>
  <thead>
    <tr>
      <th style="text-align:left">Item</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Function</td>
      <td>
       - It contains an embedded LED light, which can be turned on and off via a communication request.</br>
       - It can simultaneously measure the shift values of up to 100 workpieces and report in response to a communication request.
      </td>
    </tr>
   <tr>
      <td>Communication interface</td>
      <td>
       - The robot controller and ArgosX hardware communicate with each other through Ethernet UDP communications.</br>
        - The IP address of the ArgosX hardware is 192.168.1.XX. As the last set of digits, XX, should be set using the dip switch, the robot side should send a UDP request accordingly.</br>
        - The port number on the ArgosX hardware is fixed as 54321. However, it may change in future products.</br>
        - Upon receiving a UDP request, the ArgosX hardware will send a response to the sender’s IP address.
      </td>
    </tr>
    <tr>
      <td>Number of the systems that can be installed</td>
      <td>
       - Only one ArgosX system can be installed in the robot controller. In other words, the ArgosX system is a single instance in terms of software.
      </td>
    </tr>
  </tbody>
</table>

### Protocol
<table>
  <thead>
    <tr>
      <th style="text-align:left">Direction of transmission</th>
      <th style="text-align:left">Port#</th>
      <th style="text-align:left">Grammar and example</th>
      <th style="text-align:left">Meaning</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>robot → ArgosX</td>
      <td>54321</td>
      <td>req {workpiece#}</br>
        e.g. "req 39"</td>
      <td>Request for the shift value of the workpiece #</br>The workpiece number (#) ranges from 1 to 100 </td>
    </tr>
   <tr>
      <td>robot ← ArgosX</td>
      <td></td>
      <td>res ({x}, {y}, {z}, {rx}, {ry}, {rz})</br>
            The string "fail" will be transferred if the measurement fails.</br>
            e.g. "res (30, 25.7, 11.9, 31.6, 12.8, -54.6)"</br>
            e.g. "fail"</td>
      <td>Response regarding the shift value of the workpiece #</br>
        The values of x-rz are real numbers, and their units are mm and deg.</td>
    </tr>
    <tr>
      <td>robot → ArgosX</td>
      <td>54321</td>
      <td>light-on</td>
      <td>Turns the LED light on.</td>
    </tr>
    <tr>
      <td>robot → ArgosX</td>
      <td>54321</td>
      <td>light-off</td>
      <td>Turns the LED light off.</td>
    </tr>
  </tbody>
</table>

## Specifications of the interface plug-ins for ArgosX


The interface plug-ins for ArgosX will be developed with the following specifications.



### Robot language
<table>
  <thead>
    <tr>
      <th style="text-align:left">Item</th>
      <th style="text-align:left">Grammar</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>module</td>
      <td>argosx</td>
      <td></td>
    </tr>
   <tr>
      <td rowspan="2">attribute</td>
      <td>ip_addr</td>
      <td>The IP address string of the ArgosX hardware (it can be set.)</br>e.g. "192.168.1.44"</td>
    </tr>
    <tr>
      <td>port</td>
      <td>The port number of the ArgosX hardware.</br>(setting it should be possible, as there may be changes in future products.)</br>e.g. 54321</td>
    </tr>
    <tr>
      <td rowspan="4">function</td>
      <td>init( )</td>
      <td>Initialize the socket for UDP communication.</td>
    </tr>
    <tr>
      <td>req({workpiece#})</td>
      <td>Request the result shift value of the workpiece #</td>
    </tr>
    <tr>
      <td>res( )</td>
      <td>Receive a request while waiting for a response.</br>The return value is the shift array string based on the base coordinate system.</br>e.g. "[30, 25.7, 11.9, 31.6, 12.8, -54.6, \"base\"]"</td>
    </tr>
    <tr>
      <td>close( )</td>
      <td>Close the socket for UDP communication.</td>
    </tr>
  </tbody>
</table>

### Lighting function
- When the robot is placed in the motor On state, the ArgosX LED light will also be switched on.
- When the robot is placed in the motor Off state, the ArgosX LED light will also be switched off.

### Error handling
- When "fail" is received from ArgosX, the universal I/O output signal of the robot controller corresponding to the preset number will be switched on.


### Monitoring
By opening the ArgosX monitoring panel on the teaching pendant, you can see the following information.

- IP address
- Port #
- Error input assigned number
- Request count
- Response count



### User bar
When you open the ArgosX user bar on the teach pendant, a UI, as shown below, will be provided.

- Light-on button: Turns the ArgosX LED light on.
- Light-off button: Turns the ArgosX LED light off.