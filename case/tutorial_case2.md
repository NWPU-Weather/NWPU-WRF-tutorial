# 在WRF-4.6.1上运行case2案例
### 下载静态地理数据
本案例使用与case1同一静态地理数据，故而不再赘述。
### 下载气象数据
WPS预处理的ungrib程序需要使用气象数据，创建DATA文件夹存放气象数据：
```
cd ~/Build_WRF/WRF/
mkdir DATA
```
这里下载的是ECMWF的再分析气象数据，下载地址：[https://cds.climate.copernicus.eu/datasets](https://cds.climate.copernicus.eu/datasets)
进入下载网址后需要分别下载地面单层数据ERA5 hourly data on single levels from 1940 to present数据和等压面多层数据ERA5 hourly data on pressure levels from 1940 to present数据
#### ERA5 hourly data on single levels from 1940 to present
Variable选择10m u-component of wind，10m v-component of wind，
2m dewpoint temperature，2m temperature，Mean sea level pressur,Surface pressure，Total precipitation，Skin temperature，其它可根据自身需求选择。

年月日这里选择的是2020.7.1，时间选择0：00-23：00，即一天

Geographical选择自己想要模拟地区的经纬度（注意经纬度最好稍大于所选地区）：
North:40&emsp;&emsp;Sourth:30&emsp;&emsp;West:105&emsp;&emsp;East:115

Data format:选择GRIB格式（因为在WRF4.6.1中有处理该格式的Vtable）

Download format都可以选择，我选择的是ZIP下载格式

这些配置完成后便可以通过申请的API下载，相应的代码也在页面给出；也可以通过在线提交申请（Submit form）等待片刻后下载。
#### ERA5 hourly data on pressure levels from 1940 to present
Variable选择Geopotential，Relative humidity，Specific humidity，Temperature，U-component of wind，V-component of wind，Vertical velocity，其它可根据自身需求选择。

年月日和时间与single level相同，选择的是2020.7.1，0：00-23：00，

Pressure level选择希望模型模拟的压强，这里选择了全部压强

Geographical选择自己想要模拟地区的经纬度（注意经纬度最好稍大于所选地区）：
North:40  Sourth:30  West:105  East:115

Data format:选择GRIB格式（因为在WRF4.6.1中有处理该格式的Vtable）

Download format都可以选择，我选择的是ZIP下载格式

这些配置完成后便可以通过申请的API下载，相应的代码也在页面给出；也可以通过在线提交申请（Submit form）等待片刻后下载。

**静态地理数据和气象数据到此全部准备完毕** 
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
 start_date = '2020-07-01_00:00:00',
 end_date   = '2020-07-01_23:00:00',
 interval_seconds = 3600                     ！ 是你数据的间隔时间，单位是s
 io_form_geogrid = 2,
/

&geogrid
 parent_id         =   1,   1,
 parent_grid_ratio =   1,   3,
 i_parent_start    =   1,  31,
 j_parent_start    =   1,  17,
 e_we              =  100, 
 e_sn              =  100,  
 geog_data_res = '5m','2m',
 dx = 500,
 dy = 500,
 map_proj = 'lambert',
 ref_lat   =  34.27,
 ref_lon   = 108.9,
 truelat1  =  30.0,
 truelat2  =  60.0,
 stand_lon = 108.9,
 geog_data_path = '~/Build_WRF/WRF/WPS_GEOG/' ! 这里选择静态地理数据存放文件夹
/

&ungrib
 out_format = 'WPS',
 prefix = 'ECMWFXian_500m',
/

&metgrid
 fg_name = 'ECMWFXian_500m'
 io_form_metgrid = 2, 
/
```
注意：这里只设置了一层（max_dom=1），start_date和end_date是所选择的气象数据的开始和结束时间。网格分辨率是500m，阈为500*100=50000m，即50km范围

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
./link_grib.csh ~/Build_WRF/WRF/DATA/data*(这里数据文件名自己命名)
```
链接Vtable文件：
```
ln -sf ungrib/Variable_Tables/Vtable.ECMWF Vtable
```
运行ungrib程序：
```
./ungrib.exe
```
出现Successful字样代表成功。
检查是否生成ECMWF*（处理好的气象数据）文件：
```
ls -l ECMWF*
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
 run_hours                           = 23,
 run_minutes                         = 0,
 run_seconds                         = 0,
 start_year                          = 2020,
 start_month                         = 07,   
 start_day                           = 01,   
 start_hour                          = 00,   
 end_year                            = 2020, 
 end_month                           = 07,   
 end_day                             = 01,   
 end_hour                            = 23,  
 interval_seconds                    = 3600
 input_from_file                     = .true.,.true.,.true.,
 history_interval                    = 60,  60,   60,
 frames_per_outfile                  = 24, 1000, 1000,
 restart                             = .false.,
 restart_interval                    = 720,
 io_form_history                     = 2
 io_form_restart                     = 2
 io_form_input                       = 2
 io_form_boundary                    = 2
 /

 &domains
 time_step                           = 3,
 time_step_fract_num                 = 0,
 time_step_fract_den                 = 1,
 max_dom                             = 1,
 e_we                                = 100,   
 e_sn                                = 100,   
 e_vert                              = 38,    
 p_top_requested                     = 5000,
 num_metgrid_levels                  = 38,
 num_metgrid_soil_levels             = 0,
 dx                                  = 500, 
 dy                                  = 500, 
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
 mp_physics                          = 5,    -1,    -1,
 cu_physics                          = 0,    -1,     0,
 ra_lw_physics                       = 4,    -1,    -1,
 ra_sw_physics                       = 4,    -1,    -1,
 bl_pbl_physics                      = 1,    -1,    -1,
 sf_sfclay_physics                   = 1,    -1,    -1,
 sf_surface_physics                  = 1,    -1,    -1,
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
 relax_zone                          = 4
 nested                              = .false.
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
nohup mpirun -np 8 ./wrf.exe > run.log 2>&1 &
```
np后面的数字代表多核运行，这里是8核运行。
这里的命令可以挂起在后台运行，以防系统关闭。或者使用screen也行。
最终生成wrfout*（一个或多个）文件。

以上便是case2教程的全部内容！
