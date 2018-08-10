# nodejs使用笔记

## 1.安装

1. 去nodeks中文网下载最新版后缀为.msi的文件

2. 安装,尽量安装到D盘

3. 如果是win10系统，以管理员身份运行cmd.exe(C:\Windows\System32\cmd.exe)

4. 执行以下初始化配置

   ```
   # 查看npm包管理工具版本
   npm -v
   
   # 改变原有环境变量
   # 我们要先配置npm的全局模块的存放路径以及cache的路径，例如我希望将以上两个文件夹放在NodeJS的主目录
   # 下，便在NodeJs下建立"node_global"及"node_cache"两个文件夹，输入以下命令改变npm配置
   
   npm config set prefix "D:\Program Files\nodejs\node_global"
   npm config set cache "D:\Program Files\nodejs\node_cache"
   
   
   ```

   5.安装淘宝npm（cnpm） 

   ```
   # 安装npm淘宝镜像
   npm install -g cnpm --registry=https://registry.npm.taobao.org
   
   # 输入cnpm -v输入是否正常，这里肯定会出错。
   cnpm -v
   
   # 添加系统变量path的内容
   
   因为cnpm会被安装到D:\Program Files\nodejs\node_global下，而系统变量path并未包含该路径。在系统变量path下添加该路径即可正常使用cnpm。
   ```

   

