# VPN
> 虚拟专用网络的功能是：在公用网络上建立专用网络，进行加密通讯。在企业网络中有广泛应用。VPN网关通过对数据包的加密和数据包目标地址的转换实现远程访问。VPN有多种分类方式，主要是按协议进行分类。VPN可通过服务器、硬件、软件等多种方式实现。

## 功能

VPN属于远程访问技术，简单地说就是利用公用网络架设专用网络。例如某公司员工出差到外地，他想访问企业内网的服务器资源，这种访问就属于远程访问。
在传统的企业网络配置中，要进行远程访问，传统的方法是租用DDN（数字数据网）专线或帧中继，这样的通讯方案必然导致高昂的网络通讯和维护费用。对于移动用户（移动办公人员）与远端个人用户而言，一般会通过拨号线路（Internet）进入企业的局域网，但这样必然带来安全上的隐患。
让外地员工访问到内网资源，利用VPN的解决方法就是在内网中架设一台VPN服务器。外地员工在当地连上互联网后，通过互联网连接VPN服务器，然后通过VPN服务器进入企业内网。为了保证数据安全，VPN服务器和客户机之间的通讯数据都进行了加密处理。有了数据加密，就可以认为数据是在一条专用的数据链路上进行安全传输，就如同专门架设了一个专用网络一样，但实际上VPN使用的是互联网上的公用链路，因此VPN称为虚拟专用网络，其实质上就是利用加密技术在公网上封装出一个数据通讯隧道。有了VPN技术，用户无论是在外地出差还是在家中办公，只要能上互联网就能利用VPN访问内网资源，这就是VPN在企业中应用得如此广泛的原因。

## 原理

1. 通常情况下，[VPN网关](https://baike.baidu.com/item/VPN%E7%BD%91%E5%85%B3)采取双网卡结构，外网卡使用公网IP接入[Internet](https://baike.baidu.com/item/Internet)。
2. 网络一(假定为公网internet)的终端A访问网络二(假定为公司内网)的终端B，其发出的访问数据包的目标地址为终端B的内部IP地址。
3. 网络一的VPN网关在接收到终端A发出的访问数据包时对其目标地址进行检查，如果目标地址属于网络二的地址，则将该数据包进行封装，封装的方式根据所采用的VPN技术不同而不同，同时VPN网关会构造一个新VPN数据包，并将封装后的原数据包作为VPN数据包的负载，VPN数据包的目标地址为网络二的VPN网关的外部地址。
4. 网络一的VPN网关将VPN数据包发送到[Internet](https://baike.baidu.com/item/Internet)，由于VPN数据包的目标地址是网络二的VPN网关的外部地址，所以该数据包将被Internet中的[路由](https://baike.baidu.com/item/%E8%B7%AF%E7%94%B1)正确地发送到网络二的VPN网关。
5. 网络二的VPN网关对接收到的数据包进行检查，如果发现该数据包是从网络一的VPN网关发出的，即可判定该数据包为VPN数据包，并对该数据包进行解包处理。解包的过程主要是先将VPN数据包的包头剥离，再将数据包反向处理还原成原始的数据包。
6. 网络二的VPN网关将还原后的原始数据包发送至目标终端B，由于原始数据包的目标地址是终端B的IP，所以该数据包能够被正确地发送到终端B。在终端B看来，它收到的数据包就和从终端A直接发过来的一样。
7. 从终端B返回终端A的数据包处理过程和上述过程一样，这样两个网络内的终端就可以相互通讯了。 [1] 

通过上述说明可以发现，在[VPN网关](https://baike.baidu.com/item/VPN%E7%BD%91%E5%85%B3)对数据包进行处理时，有两个参数对于VPN通讯十分重要：原始数据包的目标地址（VPN目标地址）和远程VPN网关地址。根据VPN目标地址，VPN网关能够判断对哪些数据包进行VPN处理，对于不需要处理的数据包通常情况下可直接转发到上级路由；远程VPN网关地址则指定了处理后的VPN数据包发送的目标地址，即VPN隧道的另一端VPN网关地址。由于网络通讯是双向的，在进行VPN通讯时，隧道两端的VPN网关都必须知道VPN目标地址和与此对应的远端VPN网关地址。

## 常见问题

**错误691：提示“由于域上的用户名和/或密码无效而拒绝访问”**

1. 一般的原因是VPN连接时输入的账户和/或密码不正确，或是没有使用VPN服务的权限。
2. VPN一个账号默认仅限一台电脑使用，检查您的用户名有无登录重复。
3. 若您是在使用的途中掉线了，不要急着再次连接，请耐心等待几分钟。

若还是提示错误，请联系网络管理员。

**错误691：提示“端口已断开连接”**

1. 市面上有一小部分的路由器对VPN支持不好，从而引起错误691、只能连接几台机、经常掉线等多种问题，有时候还会出现错误800。原因是路由器采用[NAT](https://baike.baidu.com/item/NAT)方式，不能让VPN协议穿透。
2. 如果计算机中开启了系统防火墙，可以先关闭后再重试。
3. 如果偶尔出现，重拨几次，或者重新启动计算机及路由器后再重试。
4. 如果是通过局域网或者通过路由器上网的用户，请网络管理员在服务器或者路由器上打开UDP端口1701~1704。
5. 如果路由器中不能设置，可以尝试将计算机直接连到外网，用单机拨号方式连接互联网，再重试VPN拨号。
6. 部分网络如校园网、广电网、长城宽带、宽带通，容易出现691错误，需要与网络接入部门联系。
7. 安装了简化版的操作系统容易缺少相关组件，可以下载安装错误691注册表文件。

**错误721：提示“远程计算机没反应”**

1. 这种情况可能是网络延迟造成的，可以多连几次试试，如果还是不行，可以尝试以下解决方法：
2. 单击“开始”，然后单击“运行”。
3. 在“运行”中，键入regedit.exe，然后单击“确定”。
4. 在注册表编辑器中，找到以下子项：HKEY_LOCAL_MACHINE/SYSTEM/CurrentControlSet/Control/Class/{4D36E972-E325-11CE-BFC1-08002bE10318}/<000x>，其中<000x>是WAN微型端口（PPTP）驱动程序的网络适配器。
5. 在“编辑”菜单上，指向“新建”，然后单击“DWORD 值”。
6. 键入ValidateAddress，然后按 Enter。该值的默认设置为“1”（打开）；因此，您可以通过将其设置为“0”将其关闭。
7. 退出注册表编辑器。
8. 重新启动计算机。

**错误742/741：提示“远程服务器不支持加密”**

1. 选择VPN连接，右键属性，点击安全。
2. 在数据加密项选择“没有加密也可以连接”（Win7下点击网络共享中心—更改适配器—点击VPN连接图标—查看属性—安全数据加密—选择“没有加密也可以连接”）。

**错误800：提示是“不能建立VPN连接，VPN服务器不能到达”**

1. 如果计算机中开启了系统防火墙，可以先关闭后再重试。
2. 如果有安装路由器的用户，建议重启一下路由器。
3. 部分网络如校园网、广电网、长城宽带、宽带通，容易出现800错误，需要与网络接入部门联系。
4. 桌面右键单击“我的电脑”或“计算机”，打开“管理”，在“服务和应用程序”中，点击“服务”，找到“IPsec Policy Agent”服务，检查有没有禁用该服务。如果为禁用状态则改为自动状态，启动该服务。

**错误619**

1. 如果打开了防火墙（包括系统自带的）：关闭防火墙，或者设置防火墙允许UDP 1701端口。
2. 使用路由器上网：不使用路由器，或者映射UDP 1701端口。
3. 如果无以上两种现象存在，但是还出现619错误：关闭所有正在使用网络的软件，重新启动计算机然后重新进行连接。

**连上国外VPN后打开国内网页速度很慢**

因为连接上了VPN的国外线路，本地网络出口已经变更为了国家带宽出口，因此在连接VPN的状态下访问国内的网页速度是比较慢的，可简单理解成：因为线路的传输需要从国内到国外，再从国外返回国内。而如果是访问国外网页的话，此时线路方式从国内直接传输到国外是相对最快的。而且还取决于您所选的线路距离，如果您选择的是美国的线路，那么访问国内的网页肯定较慢。

**移动终端连接不上**

1. 当前网络是3G网络：3G网络通常都不稳定，不保证每次都可以连接上。
2. 如果正在办公室使用公司的无线网络但连不上VPN或发生无法响应PPTP服务器错误：请详细咨询相关人员所处的网络宽带服务商是否支持VPN，其次看所处的网络路由器是否禁止了VPN端口。
3. 电脑上可以连接VPN，但手机就不行：请确认电脑中的VPN是否处于“正在连接”的状态，如果是，请先断开电脑中的，再次连接手机试试。请注意一个账号不能同时登录在两个设备中。

**错误789**

连接尝试失败，因为安全层在初始化与远程计算机的协商时遇到一个处理错误

1. 单击“开始”，单击“运行”，键入“regedit”，然后单击“确定”

2. 找到下面的注册表子项，然后单击它：

HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Rasman\Parameters

3. 在“编辑”菜单上，单击“新建”->“DWORD值”

4. 在“名称”框中，键入“ProhibitIpSec”

5. 在“数值数据”框中，键入“1”，然后单击“确定”

6. 退出注册表编辑器，然后重新启动计算机



## 参考链接

[虚拟专用网络](https://baike.baidu.com/item/%E8%99%9A%E6%8B%9F%E4%B8%93%E7%94%A8%E7%BD%91%E7%BB%9C/8747869?fromtitle=VPN&fromid=382304&fr=aladdin)

