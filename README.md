# schtask-bypass

免杀计划任务进行权限维持，过国内主流杀软。 A schtask tool bypass anti-virus

**请勿使用于任何非法用途，由此产生的后果自行承担。**

**开机自启动、登陆自启动、定时执行任务三种权限维持的免杀方法。**

**需要以管理员权限执行dll，计划任务会以SYSTEM权限进行执行，也算是变相的提权了。**

**经过师傅的测试，cs在执行rundll32的时侯rundll32可以不必指定目录默认调用，但如ghost在终端执行的时侯就必须将rundll32文件放到和dll同一个目录下面才能执行成功。**

思路很简单，参考微软官方的文档写的dll，之前尝试过exe不免杀，但是dll免杀，直接编译即可。

若想修改计划任务的名字，请修改源码中wszTaskName，这里为了减少输入项暂时未让用户输入计划任务名字。

**师傅们也可以修改为反射型dll，然后不落地注入。**

**也可以参考[该项目](https://github.com/killeven/DllToShellCode)将dll转为shellcode并注入实现不落地执行。**

通过AttachConsole将输出转到父进程控制台，进而获取到dll的输出。

## Dll_task_boot

开机启动使用Dll_task_boot，以SYSTEM权限执行计划任务，filename为计划任务的启动程序的绝对路径：

rundll32 Dll_task_boot,DllMain filename

![1667714567689](https://user-images.githubusercontent.com/48757788/200157286-7e00113a-486f-4942-affb-d4ad574fbaf8.jpg)

## Dll_task_login

登陆启动使用Dll_task_login，需要输入用户名、权限、计划任务的启动程序三个参数：

以管理员用户为例

rundll32 Dll_task_login,DllMain Administrator S-1-5-32-544 filename

SID可以从whoami /all中当前所在组的别名中来找到

![1667727254525](https://user-images.githubusercontent.com/48757788/200163656-fad69c20-b056-41e2-aa30-2d2f0e000aa9.jpg)

## Dll_task_time

定时执行计划任务使用Dll_task_time，以SYSTEM权限执行计划任务，需要输入间隔的分钟数以及计划任务的启动程序：

以每隔一分钟执行一次弹出计算器为例，同时请注意只有当启动程序退出之后才会再次执行计划任务，不会每隔一分钟就有一个新的会话上线服务器

rundll32 Dll_task_time,DllMain 1 filename

![1667734784929](https://user-images.githubusercontent.com/48757788/200168733-4d4bc542-6058-453f-9ab9-7ae6d1208e99.jpg)









