---
title: 关于socket部分功能的使用
date: 2022-06-11 10:00:03
categories:
  - [基础知识, socket]
tags:
	- socket
	- python
	- java
---

## 前言

都是复制的以前写过的东西

## python UDP 客户端+服务端

封装进了一个类

``` python
import socket
import threading
from typing import Tuple

class SR:
    def __init__(self, ip = '0.0.0.0', port = 27013, sz = 2048) -> None:
        self.__socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
        self.__socket.bind((ip, port))
        self.__sz = sz
        self.__lock = threading.Lock()
        self.__datapool = []
        

    def __t_receive(self) -> None:
        '''接收消息, 应该放到单独的线程中'''
        while True:
            try:
                data, address = self.__socket.recvfrom(self.__sz)
                self.__lock.acquire()
                data = data.decode('UTF-8')
                self.__datapool.append((data, address))
                self.__lock.release()
            except:
                break

    def receive(self) -> None:
        '''开启消息接收的方法'''
        t = threading.Thread(target = self.__t_receive, name = 'receiver')
        t.start()

    def send(self, data, address) -> None:
        '''发送消息'''
        self.__socket.sendto(data.encode('UTF-8'), address)

    def getData(self) -> Tuple[str, Tuple[str, int]]:
        '''
        从消息队列中提取一条最早的数据
        return : (msg, (ip, port))
        '''
        #self.__lock.acquire()
        if len(self.__datapool) == 0: return None
        data = self.__datapool.pop(0)
        #self.__lock.release()
        return data
    
    def inactive(self):
        self.__socket.close()

```

## python TCP 服务端

一个摆烂的服务端脚本删减

``` python
# from MemoSqliteFunctions import *
import socket
import threading

HOST = '0.0.0.0'
PORT = 22222
BUFF_SIZE = 4096

def service_run():
    # 建立Socket连接, AF_INEF说明使用IPv4地址, SOCK_STREAM指明TCP协议
    serverSocket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    serverSocket.bind((HOST, PORT))
    serverSocket.listen(3)# 监听 

    print(f'Run at {HOST}:{PORT}')

    while True:
        # 接收TCP连接, 并返回新的Socket对象
        sk, addr = serverSocket.accept()
        print(f"客户端: {addr} 链接")

        task = threading.Thread(target = TCP_task, args=(sk, ), name = 'tcp_task')
        task.start()

    else:
        conn.close()


def TCP_task(sk : socket.socket) -> None:
	'''线程任务'''
	
    # ......
    
    while True:
        try:
            # 接收客户端发送的数据
            data = sk.recv(BUFF_SIZE)
            data = data.decode('utf-8')
            # ......
		except:
            # ......
            pass


if __name__ == '__main__':
    service_run()
```

## java(Android开发) UDP 客户端

``` java
   private void uploadUDP(String ip, String port){
       new Thread() {
           //线程运行的代码
           public void run() {
               try {
                   // Toast弹窗必要条件1
                   Looper.prepare();
                   
                   InetAddress address = InetAddress.getByName(ip);
                   DatagramSocket socket = new DatagramSocket();

                   String data = "xxxxxxx";	
					
                   // 发消息
                   byte[] data1 = data.getBytes();
                   DatagramPacket packet = new DatagramPacket(data1, data1.length, address, Integer.parseInt(port));
                   socket.send(packet);

                   //收消息
                   byte[] data2 = new byte[1 << 5];
                   DatagramPacket packet2 = new DatagramPacket(data2, data2.length);
                   socket.receive(packet2);
                   
                   String reply = new String(data2, 0, packet2.getLength());
                   
                   Toast.makeText(CloudActivity.this, "receive : "+reply, Toast.LENGTH_SHORT).show();

                   // Toast弹窗必要条件1
                   Looper.loop();
               } catch (Exception e) {
                   e.printStackTrace();
               }
           }
       }.start();//启动线程
   }
```

## java(Android开发) TCP 客户端

``` java
private void upload(String ip, String port){
        new Thread() {
            //线程运行的代码
            public void run() {
                boolean know_error = false;
                
                // Toast弹窗必要条件1
                Looper.prepare();
                try {
					// ......
                    
                    InetAddress serverip= InetAddress.getByName(ip);;//定义保存服务器地址的对象
                    Socket client=new Socket(serverip, Integer.parseInt(port));//定义创建客户端对象的Socket
                    
                    OutputStream socketOut=client.getOutputStream(); //定义发送信息的输出流对象
                    InputStream socketIn=client.getInputStream(); //定义接收数据的输入流

                    byte receive[] = new byte[buff_size]; //定义保存客户端发送来的数据的字节数组

                    // ......
                    
                    //发消息
                    String firstData = "xxxxxxx";
                    socketOut.write(firstData.getBytes("utf-8"));
                    
                    //接收数据保存在字节数组中, 然后转String字符串
                    int len=socketIn.read(receive);
                    String rev=new String(receive,0,len);

                    if (! rev.equals("ok")){
                        know_error = true;
                        Toast.makeText(CloudActivity.this, "拒绝访问", Toast.LENGTH_SHORT).show();
                        throw new Exception("access deny");
                    }

                    for(int row=0; row<rowSize ; row++) {

                        String data = Integer.toString(row);

                        // 发消息
                        socketOut.write(data.getBytes("utf-8"));

                        //接收数据保存在字节数组中, 然后转String字符串
                        len=socketIn.read(receive);
                        rev=new String(receive,0,len);
                        
                        if (! rev.equals("ok")){
                            know_error = true;
                            Toast.makeText(CloudActivity.this, "传输中遇到错误", Toast.LENGTH_SHORT).show();
                            throw new Exception("error at half road");
                        }


                    }
                    socketOut.close();
                    socketIn.close();
                    client.close();

                    Toast.makeText(CloudActivity.this, "同步完成", Toast.LENGTH_SHORT).show();
                }catch(ConnectException e){
                    Toast.makeText(CloudActivity.this, "未能成功连接服务器", Toast.LENGTH_SHORT).show();
                }
                catch(Exception e){
                    Log.i("tcp-error", e.getMessage() + e.getStackTrace() + e.getClass());
                    if(! know_error){
                        Toast.makeText(CloudActivity.this, "发生错误", Toast.LENGTH_SHORT).show();
                    }
                }
                
                // Toast弹窗必要条件2
                Looper.loop();
            }
        }.start();//启动线程
    }
```

