# 在WRF-4.6.1上运行case1案例
### 下载静态地理数据
WPS预处理的geogrid程序需要使用静态地理数据，创建WPS_GEOG文件夹存放静态地理数据：
```
cd ~/Build_WRF/WRF/
mkdir WPS_GEOG
```
静态地理数据下载地址：[https://www2.mmm.ucar.edu/wrf/users/download/get_sources_wps_geog.html](https://www2.mmm.ucar.edu/wrf/users/download/get_sources_wps_geog.html)
其中主要是下载如下这个表格里的压缩包，建议全部下载，下载完成后解压到刚创建的WPS_GEOG文件夹里，注意解压时不要嵌套相同名称的文件夹。
![地理数据网页](https://raw.githubusercontent.com/NWPU-Weather/NWPU-WRF-tutorial/main/case_image/tutorial_case1_image/WPS_GEOG.png)
有时在后面WPS预处理运行程序时会报错并且显示缺少的地理信息包，这时只需根据缺少的地理信息包去静态地理数据网址下载对应的包并解压即可。
### 下载气象数据
WPS预处理的ungrib程序需要使用气象数据，创建DATA文件夹存放气象数据：
```
cd ~/Build_WRF/WRF/
mkdir DATA
```
这里下载的是FNL的气象数据，下载地址：[https://rda.ucar.edu/datasets/d083002/ajax/#access](https://rda.ucar.edu/datasets/d083002/ajax/#access)
进入下载网址后在右上角搜索FNL，然后选择 NCEP FNL Operational Model Global Tropospheric Analyses, continuing from July 1999 (d083002)这个数据集。
![气象数据网页](https://raw.githubusercontent.com/NWPU-Weather/NWPU-WRF-tutorial/main/case_image/tutorial_case1_image/气象数据_1.png)
进入grib2格式的数据集下载网页
![气象数据网页](https://raw.githubusercontent.com/NWPU-Weather/NWPU-WRF-tutorial/main/case_image/tutorial_case1_image/气象数据_2.png)
![气象数据网页](https://raw.githubusercontent.com/NWPU-Weather/NWPU-WRF-tutorial/main/case_image/tutorial_case1_image/气象数据_3.png)
然后选择想要模拟的时间所对应的气象数据并下载解压到DATA文件夹。这里选择的是2022_01_02:00:00到2022_01_02:18:00的气象数据，时间间隔为6小时。
### WPS预处理
进入到WPS文件夹下
```
cd ~/Build_WRF/WRF/WPS-4.6.0/
```
修改namelist.wps文件
```
&share
 wrf_core = 'ARW',
 max_dom = 1,
 start_date = '2022-01-02_00:00:00','2006-08-16_12:00:00',
 end_date   = '2022-01-02_18:00:00','2006-08-16_12:00:00',
 interval_seconds = 21600
 io_form_geogrid = 2,
/

&geogrid
 parent_id         =   1,   1,
 parent_grid_ratio =   1,   3,
 i_parent_start    =   1,  31,
 j_parent_start    =   1,  17,
 e_we              =  74, 112,
 e_sn              =  61,  97,
 geog_data_res = '5m','2m',
 dx = 30000,
 dy = 30000,
 map_proj = 'lambert',
 ref_lat   =  40.1,
 ref_lon   = 117.88,
 truelat1  =  30.0,
 truelat2  =  60.0,
 stand_lon = 117.88,
 geog_data_path = '~/Build_WRF/WRF/WPS_GEOG/'
/

&ungrib
 out_format = 'WPS',
 prefix = 'FILE',
/

&metgrid
 fg_name = 'FILE'
 io_form_metgrid = 2, 
/
```
注意：这里只设置了一层（max_dom=1），start_date和end_date是所选择的气象数据的开始和结束时间。geog_data_path指的是存放静态地理数据的路径。
**生成地理数据**
运行geogrid程序：
```
./geogrid.exe
```
出现Successful字样代表成功。
检查是否生成geo_em*地理信息文件：
```
ls -l geo_em*
```
**处理气象数据**
将之前下载的气象数据链接到当前WPS目录下：
```
./link_grib.csh ~/Build_WRF/WRF/DATA/fnl
```
链接Vtable文件：
```
ln -sf ungrib/Variable_Tables/Vtable.GFS Vtable
```
运行ungrib程序：
```
./ungrib.exe
```
出现Successful字样代表成功。
检查是否生成FILE*（处理好的气象数据）文件：
```
ls -l FILE*
```
**合并地理数据和气象数据**
运行metgrid程序：
```
./metgrid.exe
```
出现Successful字样代表成功。
检查是否生成met_em*（WPS预处理的最终文件）文件：
```
ls -l met_em*
```
### WRF模型
进入到WRF的test/em_real/文件夹下：
```
cd ~/Build_WRF/WRF/WRF/test/em_real/
```
修改namelist.input文件：
```
&time_control
 run_days                            = 0,
 run_hours                           = 18,
 run_minutes                         = 0,
 run_seconds                         = 0,
 start_year                          = 2022, 2000, 2000,
 start_month                         = 01,   01,   01,
 start_day                           = 02,   24,   24,
 start_hour                          = 00,   12,   12,
 end_year                            = 2022, 2000, 2000,
 end_month                           = 01,   01,   01,
 end_day                             = 02,   25,   25,
 end_hour                            = 18,   12,   12,
 interval_seconds                    = 21600
 input_from_file                     = .true.,.true.,.true.,
 history_interval                    = 60,  60,   60,
 frames_per_outfile                  = 1000, 1000, 1000,
 restart                             = .false.,
 restart_interval                    = 7200,
 io_form_history                     = 2
 io_form_restart                     = 2
 io_form_input                       = 2
 io_form_boundary                    = 2
 /

 &domains
 time_step                           = 180,
 time_step_fract_num                 = 0,
 time_step_fract_den                 = 1,
 max_dom                             = 1,
 e_we                                = 74,    112,   94,
 e_sn                                = 61,    97,    91,
 e_vert                              = 33,    33,    33,
 p_top_requested                     = 5000,
 num_metgrid_levels                  = 34,
 num_metgrid_soil_levels             = 4,
 dx                                  = 30000, 10000,  3333.33,
 dy                                  = 30000, 10000,  3333.33,
 grid_id                             = 1,     2,     3,
 parent_id                           = 0,     1,     2,
 i_parent_start                      = 1,     31,    30,
 j_parent_start                      = 1,     17,    30,
 parent_grid_ratio                   = 1,     3,     3,
 parent_time_step_ratio              = 1,     3,     3,
 feedback                            = 1,
 smooth_option                       = 0
 /

 &physics
 physics_suite                       = 'CONUS'
 mp_physics                          = -1,    -1,    -1,
 cu_physics                          = -1,    -1,     0,
 ra_lw_physics                       = -1,    -1,    -1,
 ra_sw_physics                       = -1,    -1,    -1,
 bl_pbl_physics                      = -1,    -1,    -1,
 sf_sfclay_physics                   = -1,    -1,    -1,
 sf_surface_physics                  = -1,    -1,    -1,
 radt                                = 30,    30,    30,
 bldt                                = 0,     0,     0,
 cudt                                = 5,     5,     5,
 icloud                              = 1,
 num_land_cat                        = 21,
 sf_urban_physics                    = 0,     0,     0,
 /

 &fdda
 /

 &dynamics
 hybrid_opt                          = 2, 
 w_damping                           = 0,
 diff_opt                            = 1,      1,      1,
 km_opt                              = 4,      4,      4,
 diff_6th_opt                        = 0,      0,      0,
 diff_6th_factor                     = 0.12,   0.12,   0.12,
 base_temp                           = 290.
 damp_opt                            = 3,
 zdamp                               = 5000.,  5000.,  5000.,
 dampcoef                            = 0.2,    0.2,    0.2
 khdif                               = 0,      0,      0,
 kvdif                               = 0,      0,      0,
 non_hydrostatic                     = .true., .true., .true.,
 moist_adv_opt                       = 1,      1,      1,     
 scalar_adv_opt                      = 1,      1,      1,     
 gwd_opt                             = 1,
 /

 &bdy_control
 spec_bdy_width                      = 5,
 specified                           = .true.
 /

 &grib2
 /

 &namelist_quilt
 nio_tasks_per_group = 0,
 nio_groups = 1,
 /
```
注意，time_control里的起止时间要与namelist.wps里设置的一样，也就是模拟的气象数据的起止时间。
将WPS预处理生成的最终文件链接到当前目录：
```
ln -sf ~/Build_WRF/WRF/WPS-4.6.0/met_em*  .
```
**运行real程序**
```
mpirun -np 2 ./real.exe
```
np后面的数字代表多核运行，这里是2核运行。
生成wrfinput*(初始条件文件)和wrfbdy*(边界条件)这两个文件代表成功。
**运行wrf程序**
```
mpirun -np 8 ./wrf.exe
```
np后面的数字代表多核运行，这里是8核运行。
最终生成wrfout*（一个或多个）文件。

以上便是case1教程的全部内容！
