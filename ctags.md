# **ctags**

## **简述**

- linux给文件建立索引的工具
- 通过在目录文件下生成tags文件，组织目录内所有.c文件函数调用关系

## **安装**

- yum install ctags
- 查看是否安装成功
  - rpm -qa | grep ctags
  - ctags --help

## **生成索引**

- 在目录下运行：ctags ./* -R



## **相关命令**

- ctrl + ] ：跳转到函数定义的位置
- ctrl + t ： 返回到函数跳转之前的位置

