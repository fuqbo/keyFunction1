#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/ioctl.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h> /*文件控制*/
#include <sys/select.h>
#include <sys/time.h> /*时间方面的函数*/
#include <errno.h> /*有关错误方面的宏*/
#include <sys/poll.h> //poll()
#include <fcntl.h>
#include <string.h> //memset()
#include <linux/input.h> 


int main(void)
{
int fd,ret;
struct pollfd event;                                                         //创建一个struct pollfd结构体变量，存放文件描述符、要等待发生的事件
struct input_event key_value;


        while(1)
        { 
                fd=open("/dev/input/event1",O_RDWR); 
                if(fd<0)
                {
                    perror("open  error!\n");
                    exit(1);
                }
                                                                                                  //poll结束后struct pollfd结构体变量的内容被全部清零，需要再次设置
                memset(&event,0,sizeof(event));                       //memst函数对对象的内容设置为同一值
                event.fd=fd;                                                             //存放打开的文件描述符
                event.events=POLLIN;                                         //存放要等待发生的事件
                ret=poll((struct pollfd *)&event,1,5000);            //监测event，一个对象，等待5000毫秒后超时,-1为无限等待


                                                                                                //判断poll的返回值，负数是出错，0是设定的时间超时，整数表示等待的时间发生
               if(ret<0)
               {
                   printf("poll error!\n");
                   exit(1);
                }
               if(ret==0)
               {
                 //printf("Time out!\n");
                   continue;

               }


             if(event.revents&POLLERR)
             {                                                                                     //revents是由内核记录的实际发生的事件，events是进程等待的事件
                  printf("Device error!\n");
                  exit(1);
              }
            if(event.revents&POLLIN)
           {     //key_value=0;
                  ret=read(fd,&key_value,sizeof(key_value));

                 if(key_value.value==0)//此处判断按键是否松开
                 {
                       switch(key_value.code)//驱动input 上来的真正的按键值
                       {
                            case KEY_MENU:
                                      printf("press----KEY_MENU     mplayer\n");
                                      system("mount /dev/mmcblk0p1 /mnt/");//system 是系统调用函数，可直接执行文件系统中的任何其他应用程序。

                                      system("/opt/./mplayer -n /mnt/wenbie.mp4 &");//此处调用/opt/ 下的应用程序 ./mplayer  并播放视频文件wenbie.mp4 , '&' 表示在后台运行。
                                      break;

                           case KEY_UP:
                                     printf("press----KEY_UP     preview\n");
                                     system("mount /dev/mmcblk0p1 /mnt/");
                                     system("/opt/./preview");

                                     break;


                           case KEY_DOWN:
                                     printf("press----KEY_DOWN    audiorec\n");
                                     system("mount /dev/mmcblk0p1 /mnt/");
                                     system("/opt/./audiorec -p /mnt -a 2 -t 120");

                                      break;


                           default:
                                       break;
                       }
                }

           }


         close(fd);
      }
      }
