### **静态库、动态库的区别**

- 空间（资源利用）

  ​	静态库：复制包含在多个二进制文件中，每个二进制中均包含了静态库，浪费空间

  ​	动态库：又称共享库，多个二进制文件共用一个动态库，节省空间

  ​	动态库空间利用率高于静态库

- 时间（性能）

  ​	静态库：二进制链接完成后每次都是相当于调用自己程序中的函数，效率高
  
  ​	动态库：需要动态的去内存调用，效率低

### **静态库的生成、使用**



- **生成.o文件**

  gcc -c add.c -o add.o

  gcc -c sub.c -o sub.o

- **使用ar工具生成静态库**

  ar rcs libcalculator.a add.o sub.o

- **静态库的使用，编译静态库到可执行文件中**

  gcc main.c libcalculator.a -o a.out -I ../include

  注：-I指定所需头文件路径



### **动态库的生成、使用**

- **生成 .o 文件（生成与位置无关的代码 -fPIC）**

  gcc -c add.c -o add.o -fPIC

  gcc -c sub.c -o sub.o -fPIC

- **使用 gcc -shared 制作动态库**

   gcc -shared -o libcalculator.so add.o sub.o

- **编译可执行程序时，指定所使用的动态库**

  gcc main.c -o a.out -l calculator -L ./lib

  注： -l：指定库名(去掉lib前缀和.so后缀)，-L：指定库路径。

- **运行可执行程序前需要配置动态链接器**

  注：没有配置会出现该错误：./a.out: error while loading shared libraries: libsum.so: cannot open shared object file: No such file or directory

  配置方式1 - 临时生效

  ​	export LD_LIBRARY_PATH=动态库路径

  配置方式2 - 永久生效

  ​	1）vim ~/.bashrc

  ​	2）添加export LD_LIBRARY_PATH=动态库路径

  ​    3）生效：source ~/.bashrc

  配置方式3 - 不建议

  ​	拷贝动态库到系统文件夹/lib目录下面，二进制运行前会自动扫描该文件

  配置方式4 - 配置文件法

  ​	1）sudo vim /etc/ld.so.conf

  ​	2）文件中添加动态库所在路径

  ​	3）sudo ldconfig -v：使配置文件生效

  

  ### **问题**

  **1、动态库生成.o文件的时候为什么添加-fPIC，生成与位置无关的代码？**

  

  ​	二进制文件中，动态库函数的地址绑定要比普通函数要晚，动态库函数的地址是根据调用函数的地址进行的相对地址，如：main函数调用了sum函数、sub函数，而sub函数非动态库函数，sum函数是动态库中的函数，main函数分配地址后，假设地址为1000，sub的函数的地址将是一个明确的地址，如1100，而sum函数的地址为main+200，是相对于main函数变化的，因此生成.o文件时要加-fPIC，生成与位置无关的代码。

  

  **2、动态库的链接器和动态链接器**

  ​	链接器：工作于链接阶段，工作时需要 -l 和 -L即可满足

  ​	动态链接器：工作于程序运行阶段，工作时需要提供动态库所在目录位置。程序运行会先去固定的几个目录下去找。
