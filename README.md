# networkbandwidth
网络带宽监控软件系列
Linux服务器上监控网络带宽的18个常用命令

一些命令可以显示单个进程所使用的带宽。这样一来，用户很容易发现过度使用网络带宽的某个进程。
这些工具使用不同的机制来制作流量报告。nload等一些工具可以读取"proc/net/dev"文件，以获得流量统计信息；而一些工具使用pcap库来捕获所有数据包，然后计算总数据量，从而估计流量负载。
下面是按功能划分的命令名称。

1. 监控总体带宽使用――nload、bmon、slurm、bwm-ng、cbm、speedometer和netload
2. 监控总体带宽使用（批量式输出）――vnstat、ifstat、dstat和collectl
3. 每个套接字连接的带宽使用――iftop、iptraf、tcptrack、pktstat、netwatch和trafshow
4. 每个进程的带宽使用――nethogs

#系列

1. nload

nload是一个命令行工具，让用户可以分开来监控入站流量和出站流量。它还可以绘制图表以显示入站流量和出站流量，视图比例可以调整。用起来很简单，不支持许多选项。
所以，如果你只需要快速查看总带宽使用情况，无需每个进程的详细情况，那么nload用起来很方便。
     nload 
安装nload：Fedora和Ubuntu在默认软件库里面就有nload。CentOS用户则需要从Epel软件库获得nload。
# fedora或centos 
     yum install nload -y 
ubuntu/debian 
     sudo apt-get install nload 


2. iftop
iftop可测量通过每一个套接字连接传输的数据；它采用的工作方式有别于nload。iftop使用pcap库来捕获进出网络适配器的数据包，然后汇总数据包大小和数量，搞清楚总的带宽使用情况。
虽然iftop报告每个连接所使用的带宽，但它无法报告参与某个套按字连接的进程名称/编号（ID）。不过由于基于pcap库，iftop能够过滤流量，并报告由过滤器指定的所选定主机连接的带宽使用情况。
     sudo iftop -n 
n选项可以防止iftop将IP地址解析成主机名，解析本身就会带来额外的网络流量。

安装iftop：Ubuntu/Debian/Fedora用户可以从默认软件库获得它。CentOS用户可以从Epel获得它。
# fedora或centos 
yum install iftop -y 
# ubuntu或 debian 
     sudo apt-get install iftop 
3. iptraf
iptraf是一款交互式、色彩鲜艳的IP局域网监控工具。它可以显示每个连接以及主机之间传输的数据量。下面是屏幕截图。

     sudo iptraf 
安装iptraf：
# Centos（基本软件库） 
     yum install iptraf 
# fedora或centos（带epel） 
     yum install iptraf-ng -y 
# ubuntu或debian 
     sudo apt-get install iptraf iptraf-ng 
4. nethogs
nethogs是一款小巧的"net top"工具，可以显示每个进程所使用的带宽，并对列表排序，将耗用带宽最多的进程排在最上面。万一出现带宽使用突然激增的情况，用户迅速打开nethogs，就可以找到导致带宽使用激增的进程。nethogs可以报告程序的进程编号（PID）、用户和路径。
     sudo nethogs 

安装nethogs：Ubuntu、Debian和Fedora用户可以从默认软件库获得。CentOS用户则需要Epel。
# ubuntu或debian（默认软件库） 
     sudo apt-get install nethogs 
# fedora或centos（来自epel） 
     sudo yum install nethogs -y 
5. bmon
bmon（带宽监控器）是一款类似nload的工具，它可以显示系统上所有网络接口的流量负载。输出结果还含有图表和剖面，附有数据包层面的详细信息。

安装bmon：Ubuntu、Debian和Fedora用户可以从默认软件库来安装。CentOS用户则需要安装repoforge，因为Epel里面没有bmon。
# ubuntu或debian 
     sudo apt-get install bmon 
# fedora或centos（来自repoforge） 
     sudo yum install bmon 
bmon支持许多选项，能够制作HTML格式的报告。欲知更多信息，请参阅参考手册页。
6. slurm
slurm是另一款网络负载监控器，可以显示设备的统计信息，还能显示ASCII图形。它支持三种不同类型的图形，使用c键、s键和l键即可激活每种图形。slurm功能简单，无法显示关于网络负载的任何更进一步的详细信息。
     slurm -s -i eth0 

安装slurm
# debian或ubuntu 
     sudo apt-get install slurm 
# fedora或centos 
     sudo yum install slurm -y 
7. tcptrack
tcptrack类似iftop，使用pcap库来捕获数据包，并计算各种统计信息，比如每个连接所使用的带宽。它还支持标准的pcap过滤器，这些过滤器可用来监控特定的连接。

安装tcptrack：Ubuntu、Debian和Fedora在默认软件库里面就有它。CentOS用户则需要从RepoForge获得它，因为Epel里面没有它。
# ubuntu, debian 
     sudo apt-get install tcptrack 
# fedora, centos（来自repoforge软件库） 
     sudo yum install tcptrack 
8. vnstat
vnstat与另外大多数工具有点不一样。它实际上运行后台服务/守护进程，始终不停地记录所传输数据的大小。之外，它可以用来制作显示网络使用历史情况的报告。
     service vnstat status 
* vnStat daemon is running 
运行没有任何选项的vnstat，只会显示自守护进程运行以来所传输的数据总量。
     vnstat 
Database updated: Mon Mar 17 15:26:59 2014 
eth0 since 06/12/13 
rx:  135.14 GiB      tx:  35.76 GiB      total:  170.90 GiB 
monthly 
rx      |     tx      |    total    |   avg. rate 
 
------------------------+-------------+-------------+------------- 
Feb '14      8.19 GiB  |    2.08 GiB  |   10.27 GiB |   35.60 kbit/s 
Mar '14      4.98 GiB  |    1.52 GiB  |    6.50 GiB |   37.93 kbit/s 
------------------------+-------------+-------------+------------- 
estimated       9.28 GiB |    2.83 GiB  |   12.11 GiB | 
daily 
rx      |     tx      |    total    |   avg. rate 
------------------------+-------------+-------------+------------- 
yesterday     236.11 MiB |   98.61 MiB |  334.72 MiB |   31.74 kbit/s 
today    128.55 MiB |   41.00 MiB |  169.56 MiB |   24.97 kbit/s 
------------------------+-------------+-------------+------------- 
estimated       199 MiB |      63 MiB |     262 MiB | 
想实时监控带宽使用情况，请使用"-l"选项（实时模式）。然后，它会显示入站数据和出站数据所使用的总带宽量，但非常精确地显示，没有关于主机连接或进程的任何内部详细信息。
     vnstat -l -i eth0 
Monitoring eth0...    (press CTRL-C to stop) 
rx:       12 kbit/s    10 p/s          tx:       12 kbit/s    11 p/s 
vnstat更像是一款制作历史报告的工具，显示每天或过去一个月使用了多少带宽。它并不是严格意义上的实时监控网络的工具。
vnstat支持许多选项，支持哪些选项方面的详细信息请参阅参考手册页。
安装vnstat
# ubuntu或debian 
     sudo apt-get install vnstat 
# fedora或 centos（来自epel） 
     sudo yum install vnstat 
9. bwm-ng
bwm-ng（下一代带宽监控器）是另一款非常简单的实时网络负载监控工具，可以报告摘要信息，显示进出系统上所有可用网络接口的不同数据的传输速度。
     bwm-ng 
bwm-ng v0.6 (probing every 0.500s), press 'h' for help 
input: /proc/net/dev type: rate 
/         iface                   Rx                   Tx                T 
ot================================================================= 
==           eth0:           0.53 KB/s            1.31 KB/s            1.84 
KB             lo:           0.00 KB/s            0.00 KB/s            0.00 
KB------------------------------------------------------------------------------------------------------------- 
total:           0.53 KB/s            1.31 KB/s            1.84 
KB/s 
如果控制台足够大，bwm-ng还能使用curses2输出模式，为流量绘制条形图。
     bwm-ng -o curses2 
安装bwm-ng：在CentOS上，可以从Epel来安装bwm-ng。
# ubuntu或debian 
     sudo apt-get install bwm-ng 
# fedora或centos（来自epel） 
     sudo apt-get install bwm-ng 
10. cbm：Color Bandwidth Meter

这是一款小巧简单的带宽监控工具，可以显示通过诸网络接口的流量大小。没有进一步的选项，仅仅实时显示和更新流量的统计信息。
     sudo apt-get install cbm 
11. speedometer
这是另一款小巧而简单的工具，仅仅绘制外观漂亮的图形，显示通过某个接口传输的入站流量和出站流量。
     speedometer -r eth0 -t eth0 

安装speedometer
# ubuntu或debian用户 
     sudo apt-get install speedometer 
12. pktstat

pktstat可以实时显示所有活动连接，并显示哪些数据通过这些活动连接传输的速度。它还可以显示连接类型，比如TCP连接或UDP连接；如果涉及HTTP连接，还会显示关于HTTP请求的详细信息。
     sudo pktstat -i eth0 -nt 
     sudo apt-get install pktstat 
13. netwatch

netwatch是netdiag工具库的一部分，它也可以显示本地主机与其他远程主机之间的连接，并显示哪些数据在每个连接上所传输的速度。
     sudo netwatch -e eth0 -nt 
     sudo apt-get install netdiag 
14. trafshow
与netwatch和pktstat一样，trafshow也可以报告当前活动连接、它们使用的协议以及每条连接上的数据传输速度。它能使用pcap类型过滤器，对连接进行过滤。
只监控TCP连接

     sudo trafshow -i eth0 tcp 
     sudo apt-get install netdiag 
15. netload
netload命令只显示关于当前流量负载的一份简短报告，并显示自程序启动以来所传输的总字节量。没有更多的功能特性。它是netdiag的一部分。

     netload eth0 
     sudo apt-get install netdiag 
16. ifstat
ifstat能够以批处理式模式显示网络带宽。输出采用的一种格式便于用户使用其他程序或实用工具来记入日志和分析。
     ifstat -t -i eth0 0.5 
Time           eth0 
HH:MM:SS   KB/s in  KB/s out 
09:59:21   　   2.62      2.80 
09:59:22   　   2.10      1.78 
09:59:22   　   2.67      1.84 
09:59:23    　  2.06      1.98 
09:59:23    　  1.73      1.79 
安装ifstat：Ubuntu、Debian和Fedora用户在默认软件库里面就有它。CentOS用户则需要从Repoforge获得它，因为Epel里面没有它。
# ubuntu, debian 
     sudo apt-get install ifstat 
# fedora, centos（Repoforge） 
     sudo yum install ifstat 
17. dstat
dstat是一款用途广泛的工具（用python语言编写），它可以监控系统的不同统计信息，并使用批处理模式来报告，或者将相关数据记入到CSV或类似的文件。这个例子显示了如何使用dstat来报告网络带宽。
安装dstat
     dstat -nt 
-net/total- ----system---- 
recv  send|     time 
0     0 |23-03 10:27:13 
1738B 1810B|23-03 10:27:14 
2937B 2610B|23-03 10:27:15 
2319B 2232B|23-03 10:27:16 
2738B 2508B|23-03 10:27:17 
18. collectl
collectl以一种类似dstat的格式报告系统的统计信息；与dstat一样，它也收集关于系统不同资源（如处理器、内存和网络等）的统计信息。这里给出的一个简单例子显示了如何使用collectl来报告网络使用/带宽。


     collectl -sn -oT -i0.5 
waiting for 0.5 second sample... 
#         <----------Network----------> 
#Time       KBIn  PktIn  KBOut  PktOut 
10:32:01      40     58     43      66 
10:32:01      27     58      3      32 
10:32:02       3     28      9      44 
10:32:02       5     42     96      96 
10:32:03       5     48      3      28 


安装collectl
# Ubuntu/Debian用户 
     sudo apt-get install collectl 
#Fedora 
     sudo yum install collectl 
