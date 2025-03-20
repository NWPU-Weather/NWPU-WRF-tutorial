# 在WRF-4.6.1上运行case1案例
#### 下载静态地理数据
WPS预处理的geogrid程序需要使用静态地理数据，创建WPS_GEOG文件夹存放静态地理数据：
```
cd ~/Build_WRF/WRF/
mkdir WPS_GEOG
```
静态地理数据下载地址：[https://www2.mmm.ucar.edu/wrf/users/download/get_sources_wps_geog.html](https://www2.mmm.ucar.edu/wrf/users/download/get_sources_wps_geog.html)
其中主要是下载如下这个表格里的压缩包，建议全部下载，下载完成后解压到刚创建的WPS_GEOG文件夹里，注意解压时不要嵌套相同名称的文件夹。
![地理数据网页](https://raw.githubusercontent.com/NWPU-Weather/NWPU-WRF-tutorial/main/case_image/tutorial_case1_image/WPS_GEOG.png)
有时在后面WPS预处理运行程序时会报错并且显示缺少的地理信息包，这时只需根据缺少的地理信息包去静态地理数据网址下载对应的包并解压即可。
#### 下载气象数据
WPS预处理的ungrib程序需要使用气象数据，创建DATA文件夹存放气象数据：
```
cd ~/Build_WRF/WRF/
mkdir DATA
```
这里下载的是FNL的气象数据，下载地址：[https://rda.ucar.edu/datasets/d083002/ajax/#access](https://rda.ucar.edu/datasets/d083002/ajax/#access)
进入下载网址后在右上角输入FNL进行搜索，然后选择 NCEP FNL Operational Model Global Tropospheric Analyses, continuing from July 1999 (d083002)这个数据集。
![气象数据网页](https://raw.githubusercontent.com/NWPU-Weather/NWPU-WRF-tutorial/main/case_image/tutorial_case1_image/气象数据_1.png)
进入grib2格式的数据集下载网页
![气象数据网页](https://raw.githubusercontent.com/NWPU-Weather/NWPU-WRF-tutorial/main/case_image/tutorial_case1_image/气象数据_2.png)
![气象数据网页](https://raw.githubusercontent.com/NWPU-Weather/NWPU-WRF-tutorial/main/case_image/tutorial_case1_image/气象数据_3.png)
然后选择想要模拟的时间所对应的气象数据并下载解压到DATA文件夹。这里选择的是2022_01_02:00:00到2022_01_02:18:00的气象数据，时间间隔为6小时。
#### WPS预处理
进入到WPS文件夹下，修改