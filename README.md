# DOL配置    

---

## Description
The distributed operation layer (DOL) is a framework that enables the (semi-) automatic mapping of applications onto the multiprocessor SHAPES architecture platform. 

The DOL consists of basically three parts:

 - DOL Application Programming Interface

The DOL defines a set of computation and
communication routines that enable the programming of distributed, parallel applications for the SHAPES platform. Using these routines, application programmers can write programs without having detailed knowledge about the underlying architecture. In fact, these routines are subject to further refinement in the hardware dependent software (HdS) layer.

 - DOL Functional Simulation

To provide programmers a possibility to test their applications, a functional simulation framework has been developed. Besides functional verification of applications, this framework is used to obtain performance parameters at the application level.

 - DOL Mapping Optimization:

The goal of the DOL mapping optimization is to compute a set of optimal mappings of an application onto the SHAPES architecture platform. In a first step, XML based specification formats have been defined that allow to describe the application and the architecture at an abstract level. Still, all the information necessary to obtain accurate performance estimates is contained.
  

## How to install
我安装的虚拟机的系统是Ubuntu15.4


安装步骤：

1. 配置Java环境  
    - 将jdk-8u40-linux-x64.gz安装包解压到  /usr/lib/java  

        ```
        cd /usr/lib
        sudo mkdir java
        #进入安装包下载所在路径(在终端打开)
        sudo tar zxvf ./jdk-8u40-linux-x64.gz -C /usr/lib/java
        #重命名为jdk8
        cd /usr/lib/java
        sudo mv jdk1.8.0_40/ jdk8
        ```  
    - 配置环境变量  
        ```
        gedit ~/.bashrc
        ```  

        打开文件后，在文件末尾添加JDK所在路径：

         ```           
         # enable jdk environment
         export JAVA_HOME=/usr/lib/java/jdk8
         export JRE_HOME=${JAVA_HOME}/jre
         export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
         export PATH=${JAVA_HOME}/bin:$PATH
         ```
     
        保存后退出。  
        终端中输入以下指令使它生效：
        ```
        source ~/.bashrc
        ```
    - 配置默认JDK  
     /usr/lib/java/jdk8/为JDK所在路径  

        ```
        sudo update-alternatives --install /usr/bin/java java /usr/lib/java/jdk8/bin/java 300  
        sudo update-alternatives --install /usr/bin/javac javac /usr/lib/java/jdk8/bin/javac 300  
        ```  
        
    - 验证配置是否成功  
    
        ```
        java -version	# 查看JDK版本
        java
        javac
        ```  
 
        输入java -version结果如下图  
        ![java -version][1]  
        输入java结果如下图  
        ![java][2]  
        输入javac结果如下图  
        ![javac][3]  

2. 配置Ant环境
```
sudo apt-get install ant
```  
    - 验证是否安装成功  
    
        ```
        ant
        ```  

        输入ant结果如下图  
        ![ant][4]  
  
  
3. 配置SystemC  
    - 安装与配置  
 
        ```
        # 解压到自定义安装路径下
        sudo mkdir /home/rainbow/apps/libs/
        sudo tar -zxvf systemc-2.3.1.tgz [-C /home/rainbow/apps/libs/]
        cd /home/rainbow/apps/libs/systemc-2.3.1
        sudo mkdir objdir
        cd objdir
        sudo ../configure	--disable-async-updates	
        sudo make
        sudo make check
        sudo make install
        ```  
        ../configure最后显示如下图  
        ![../configure][5]
    - 进行测试    
        a. 编写hello.h文件  
        （这里的路径是：/home/rainbow/Workspace/systemC_tb）
    
            #ifndef _HELLO_H
    	    #define _HELLO_H
    	    #include "systemc.h"
    	    #include <iostream>
    	    using namespace std;
    
    	    SC_MODULE(hello){
    		    SC_CTOR(hello){
    			    cout<<"Hello,SystemC!"<<endl;
    		    }
    	    };
    	    #endif
        b. 编写hello.cpp文件

            #if 1
    	    #include "hello.h"
    	    #else
    	    #include "systemc.h"
    
    	    class hello : public sc_module{
    	    public:
    		    hello(sc_module_name name) : sc_module(name){
    		            cout<<"Hello,SystemC!"<<endl;
    		    }
    	    };
    	    #endif
    
    	    int sc_main(int argc,char** argv){
    		    hello h("hello");
    		    return 0;
    	    }

      c. 编写Makefile  
      (下方的/home/rainbow/apps/libs/systemc-2.3.1为配置的SystemC路径) [LIB_DIR，INC_DIR都是上述make过程生成的东西]

            LIB_DIR=-L /home/rainbow/apps/libs/systemc-2.3.1/lib-linux64
    	    INC_DIR=-I /home/rainbow/apps/libs/systemc-2.3.1/include
    
    	    LIB=-l systemc
    
    	    APP=hello
    
    	    all:
    		    g++ -o $(APP) $(APP).cpp $(LIB_DIR) $(INC_DIR) $(LIB)
    
    	    clean:
    		    rm -rf $(APP)
        d. 上述文件放在同一个目录下, cd进入该目录 make之后即可通过 ./hello运行  

            cd /home/rainbow/Workspace/systemC_tb  
            sudo make
            ./hello
        出现如下信息表示成功配置：  
        ![./hello][6]

4. 配置DOL
    - 解压dol_ethz.zip  
        ```  
        #新建dol的文件夹 
        mkdir dol    
        
        #将dolethz.zip解压到 dol文件夹中
        unzip dol_ethz.zip -d dol      
        ```
    - 用gedit打开build_zip.xml：  
     找到下面这段话
         ```
        <property name="systemc.inc" value="YYY/include"/>
        <property name="systemc.lib" value="YYY/lib-linux/libsystemc.a"/>
        ```       
     把YYY改成安装SystemC的安装路径（/home/rainbow/apps/libs/systemc-2.3.1）  
     对于64位系统的机器，lib-linux要改成lib-linux64。   
    - 编译build_zip.xml
        ```
        ant -f build_zip.xml all
        ```
    编译成功会显示下图  
    ![build_zip.xml编译][7]
    - 测试示例
        ```
        cd build/bin/main
        sudo ant -f runexample.xml -Dnumber=1
        ```
    编译成功如下图  
    ![runexample.xml编译][8]


  [1]: http://ww2.sinaimg.cn/large/69347328gw1f8dy0qqoqjj20ie02mmyd.jpg
  [2]: http://ww1.sinaimg.cn/large/69347328gw1f8dy3r3883j20hv0b1adx.jpg
  [3]: http://ww3.sinaimg.cn/large/69347328gw1f8dy5it3dsj20k30c2n33.jpg
  [4]: http://ww2.sinaimg.cn/large/69347328gw1f8dykioshxj20hp023dgf.jpg
  [5]: http://ww3.sinaimg.cn/large/69347328gw1f8dywpzh7vj20h60bhadu.jpg
  [6]: http://ww1.sinaimg.cn/large/69347328gw1f8dzfv6n7wj20e3030752.jpg
  [7]: http://ww4.sinaimg.cn/large/69347328gw1f8e0n6s04mj205l01cglk.jpg
  [8]: http://ww3.sinaimg.cn/large/69347328gw1f8e0rfoy01j20870c2djh.jpg
  
  
  
## Experimental experience
由于不熟悉Linux操作系统的命令，配置过程中一直都要上网查询Linux命令的使用方法。在此记录下我认为比较有用但不熟悉的Linux命令：

1. [Tab]按键  
具有『命令补全』『档案补齐』的功能

2. chmod命令  
用来变更文件或目录的权限  
 - 语法    
    chmod(选项)(参数) 
 - 选项   
    <权限范围>+<权限设置>：开启权限范围的文件或目录的该选项权限设置；  
    <权限范围>-<权限设置>：关闭权限范围的文件或目录的该选项权限设置；  
    <权限范围>=<权限设置>：指定权限范围的文件或目录的该选项权限设置；   
    u User，即文件或目录的拥有者；   
    g Group，即文件或目录的所属群组；  
    o Other，除了文件或目录拥有者或所属群组之外，其他用户皆属于这个范围； 
    a All，即全部的用户，包含拥有者，所属群组以及其他用户；  
    r 读取权限，数字代号为“4”;  
    w 写入权限，数字代号为“2”；  
    x 执行或切换权限，数字代号为“1”；  
    不具任何权限，数字代号为“0”；  
    s 特殊功能说明：变更文件或目录的权限。 
 - 参数  
    权限模式（指定文件的权限模式）、 文件（要改变权限的文件）
 - 例子

            chmod u+x,g+w f01　　//为文件f01设置自己可以执行，组员可以写入的权限
            chmod u=rwx,g=rw,o=r f01 
            chmod 764 f01 
            chmod a+x f01　　//对文件f01的u,g,o都设置可执行属性

3. 解压

        tar –xvf file.tar //解压 tar包 
        tar -xzvf file.tar.gz //解压tar.gz 
        tar -xjvf file.tar.bz2 //解压 tar.bz2 
        tar –xZvf file.tar.Z //解压tar.Z 
        unrar e file.rar //解压rar 
        unzip file.zip //解压zip 

