注意：后面带有 (max_dom) 的变量表示当 max_dom > 1 时，该变量需要为嵌套网格定义。  
&time_control
run_days                            = 0,    ! 运行时间（天）  
run_hours                           = 0,    ! 运行时间（小时）  
                              注意：如果运行时间超过 1 天，可以同时使用 run_days 和 run_hours，也可以仅使用 run_hours。例如，如果总运行时间为 36 小时，可以设置 run_days = 1, run_hours = 12，或 run_days = 0, run_hours = 36。  
run_minutes                         = 0,    ! 运行时间（分钟）  
run_seconds                         = 0,    ! 运行时间（秒）  
start_year (max_dom)                = 2001, ! 起始时间的四位数年份    
start_month (max_dom)               = 06,   ! 起始时间的两位数月份    
start_day (max_dom)                 = 11,   ! 起始时间的两位数日期    
start_hour (max_dom)                = 12,   ! 起始时间的两位数小时    
start_minute (max_dom)              = 00,   ! 起始时间的两位数分钟    
start_second (max_dom)              = 00,   ! 起始时间的两位数秒     
                              注意：起始时间用于命名第一个 wrfout 文件。  
                              它还控制嵌套网格的起始时间，以及重启的时间。  
end_year (max_dom)                  = 2001, ! 结束时间的四位数年份    
end_month (max_dom)                 = 06,   ! 结束时间的两位数月份    
end_day (max_dom)                   = 12,   ! 结束时间的两位数日期  
end_hour (max_dom)                  = 12,   ! 结束时间的两位数小时  
end_minute (max_dom)                = 00,   ! 结束时间的两位数分钟  
end_second (max_dom)                = 00,   ! 结束时间的两位数秒  
                              该变量还控制嵌套网格的积分何时结束。 所有的起始和结束时间都由 real.exe 使用。需要注意的是，可以使用 run_days/run_hours 等变量，也可以使用 end_year/month/day/hour 等变量来控制模型积分的时间长度。但 run_days/run_hours的优先级高于结束时间。  
                              real.exe 仅使用起始和结束时间。

interval_seconds                    = 10800,! 真实数据的输入时间间隔（秒），同时也是侧边界条件文件的时间间隔  
input_from_file (max_dom)           = T,    ! 是否为除主网格（域 1）以外的嵌套网格提供输入文件  
fine_input_stream (max_dom)         = 0,    ! 用于初始化嵌套网格的输入场选择  
                                            0: 使用所有字段  
                                            2: 仅使用静态和随时间变化的被掩蔽的陆地表面字段  
                                                    需要使用 io_form_auxinput2  
history_interval (max_dom)          = 60,  	! 生成历史输出文件的时间间隔（分钟）  
frames_per_outfile (max_dom)        = 1, 	  ! 每个历史输出文件包含的时间帧数  
                                                  该选项用于将输出拆分成多个小文件  
restart                             = F,	  ! 是否为重启运行  
cycling                             = F,    ! 是否为循环运行，如果是，则仅为 Thompson 方案初始化查找表  
restart_interval		     = 1440,	          ! 生成重启文件的时间间隔（分钟）  
reset_simulation_start              = F,    ! 是否用预报起始时间覆盖模拟起始时间  
io_form_history                     = 2,    ! 2 = netCDF 格式  
io_form_restart                     = 2,    ! 2 = netCDF 格式  
io_form_input                       = 2,    ! 2 = netCDF 格式  
io_form_boundary                    = 2,    ! 侧边界文件格式：  
                                     = 4,     ! PHD5 格式  
                                     = 5,     ! GRIB1 格式  
                                     = 10,    ! GRIB2 格式  
                                     = 11,    ! pnetCDF 格式  
ncd_nofill                          = .true. 
                                            ! 只执行一次写入操作，而不是写入/读取/写入的序列  
debug_level                         = 0, 	  ! 调试信息打印级别（50,100,200,300 依次增加打印信息）  
diag_print                          = 0, 	  ! 是否打印模型诊断的时间序列数据  
                                            0 = 不打印  
                                            1 = 标准输出文件中输出 3 小时平均的静力学地表气压变化率 (Dpsfc/Dt) 以及干静力学柱气压变化率 (Dmu/Dt)  
                                            2 = 除以上内容外，还包括区域平均降水量、地表蒸发量、 以及显热和潜热通量的输出  
all_ic_times                        = .false., ! 是否为所有处理时间写出 wrfinput 文件  
adjust_output_times                 = .false., ! 是否将输出时间调整到最近的整点  
override_restart_timers             = .false., ! 是否修改重启计时器的预设报警值  
write_hist_at_0h_rst                = .false., ! 是否在重启运行的起始时间输出历史文件  
write_restart_at_0h                 = .false., ! 是否在重启运行的起始时间输出重启文件  
output_ready_flag		        = .true.,       ! 生成一个空文件 `wrfoutReady_d<domain>_<date>`，  
                                            该文件可用于生产运行中，供后处理代码检查文件是否完整  
force_use_old_data                  = .false., 
                                            ! 如果设置为 .true.，允许使用 WRF 版本 3 的输入数据  
                                            = .false. 时，如果 WRF 模型检测到版本 3 数据，则终止运行  

选择 EM 核心的输入方式（来自 SI 或 WPS）：  
auxinput1_inname                    = "met_em.d<domain>.<date>"             
                                            ! 默认使用 WPS 生成的 met_em 文件（WRF 3.0 及以上版本）  
                                    = "wrf_real_input_em.d<domain>.<date>"  
                                            ! 使用 SI 生成的 wrf_real_input_em 文件  

其他输出选项： **注意**：所有 auxhist[1-24] 和 auxinput[2-24] 变量均可针对不同网格域单独设置  

auxhist9_outname                    = "rainfall" ! 额外的输出文件名称，若未指定，则默认为  
                                                    `auxhist9_d<domain>_<date>`  
                                                    需要输出历史文件之外的变量时，必须修改 Registry.EM 文件  
auxhist9_interval (max_dom)         = 10,   ! 以分钟为单位的输出时间间隔  
io_form_auxhist9                    = 2,    ! 采用 netCDF 格式输出  
frames_per_auxhist9                 = 1000, ! 每个 auxhist9 文件包含的时间步数  

**SST（海表温度）更新（仅在 sst_update=1 时使用）：**  
auxinput4_inname                    = "wrflowinp_d<domain>"  
auxinput4_interval (max_dom)        = 360      
                                            ! 时间间隔（分钟），通常与 interval_seconds 相匹配  
io_form_auxinput4                   = 2     ! IO 格式  

nwp_diagnostics                     = 0     ! 设置为 1 时，会增加 7 个历史时间步的最大诊断字段  

**用于区域气候表面场的额外诊断：**  
output_diagnostics                  = 0     ! 设置为 1 时，增加 36 个表面诊断数据集（最大值/最小值/均值/标准差）  
auxhist3_outname                    = 'wrfxtrm_d<domain>_<date>' ! 额外诊断数据的文件名  
io_form_auxhist3                    = 2     ! 使用 netCDF 格式  
auxhist3_interval (max_dom)         = 1440  ! 诊断数据的输出时间间隔（分钟，1440 表示每日最大/最小值）  
frames_per_auxhist3                 = 1     ! 每个文件包含的时间步数  
                                                  **注意**：仅在 auxhist3_interval 的整数倍时才进行重启  

**观测数据调整（仅在使用观测数据时启用）：**  
auxinput11_interval                 = 10    ! 观测数据输入的时间间隔（分钟），应至少与 obs_ionf 设定的间隔一致  
auxinput11_end_h                    = 6     ! 观测数据的结束时间（小时）  

**运行时 IO 选项：**  
iofields_filename (max_dom)         = "my_iofields_list.txt"  
                                       (示例： `+:h:21:rainc, rainnc, rthcuten`)  
ignore_iofields_warning             = .true.,  ! 遇到用户指定文件的错误时如何处理  
                                      .false., ! 发生错误时终止运行  

nocolons                            = .false., ! 是否在文件日期格式中使用冒号  
                                      .true.,  ! 例如：2020-05-20_12_00_00  

**WRFVAR（WRF 变分同化）相关设置：**  
write_input                         = t,    ! 以输入格式写出数据  
inputout_interval (max_dom)         = 180,  ! 以分钟为单位的输入格式数据写出间隔  
input_outname                       = 'wrfinput_d<domain>_<date>' ! 输出文件名，可自定义  
inputout_begin_y (max_dom)          = 0  
inputout_begin_m                    = 0  
inputout_begin_d (max_dom)          = 0  
inputout_begin_h (max_dom)          = 3  
inputout_begin_m (max_dom)          = 0  
inputout_begin_s (max_dom)          = 0  
inputout_end_y (max_dom)            = 0  
inputout_end_m                      = 0  
inputout_end_d (max_dom)            = 0  
inputout_end_h (max_dom)            = 12  
inputout_end_m (max_dom)            = 0  
inputout_end_s (max_dom)            = 0  
                                                  以上设置表示，输入格式数据将在 3 小时到 12 小时之间，以 180 分钟的间隔输出  

**自动移动嵌套网格（需特殊输入数据，并在编译时设置环境变量 TERRAIN_AND_LANDUSE）：**  
input_from_hires (max_dom)          = .true.,  
rsmas_data_path                     = "path-to-terrain-and-landuse-dataset"  

&domains
 time_step                           = 60,	! 积分时间步长（以整数秒为单位）
                                                  对于典型的真实数据情况，推荐 6*dx（单位：km）
 time_step_fract_num                 = 0,	  ! 分数时间步长的分子
 time_step_fract_den                 = 1,	  ! 分数时间步长的分母
                                                  例如，如果希望使用 60.3 秒作为时间步长，
                                                  设 time_step = 60, time_step_fract_num = 3,
                                                  time_step_fract_den = 10
 time_step_dfi                       = 60,	! DFI（数字滤波初始化）的时间步长，可与常规时间步长不同
 reasonable_time_step_ratio          = 6.,	! 如果 d01（第一层域）在真实数据模式下时间步长比大于此值，运行将被终止。
                                                  除非有特殊情况（如使用 IEVA），否则此值不应超过 6（默认值）。
 max_dom                             = 1,	  ! 计算域的数量，如果使用嵌套模式，设为大于 1
 s_we (max_dom)                      = 1,	  ! x 方向（西-东）起始索引（保持默认）
 e_we (max_dom)                      = 91,	! x 方向（西-东）结束索引（交错维度）
 s_sn (max_dom)                      = 1,	  ! y 方向（南-北）起始索引（保持默认）
 e_sn (max_dom)                      = 82,	! y 方向（南-北）结束索引（交错维度）
 s_vert (max_dom)                    = 1,	  ! z 方向（垂直方向）起始索引（保持默认）
 e_vert (max_dom)                    = 30,	! z 方向（垂直方向）结束索引（交错维度）
                                                  注意：该值包括地表和模型顶部的完整层数
                                                  垂直方向的层数必须在所有嵌套网格中保持一致
                                                  备注：大多数变量是非交错的（= 交错维度 - 1）
 dx (max_dom)                        = 10000,	! x 方向网格间距，单位：米
 dy (max_dom)                        = 10000,	! y 方向网格间距，单位：米
 ztop (max_dom)                      = 19000.	! 质量模式下用于理想化情况的最高高度
 grid_id (max_dom)                   = 1,	  ! 域标识符
 parent_id (max_dom)                 = 0,   ! 父域的 ID
 i_parent_start (max_dom)            = 0,   ! 在父域中的起始 x 索引
 j_parent_start (max_dom)            = 0,   ! 在父域中的起始 y 索引
 parent_grid_ratio (max_dom)         = 1,   ! 父域与子域的网格大小比
                                              对于真实数据模式，该比率必须是奇数
                                              对于理想化情况，如果 feedback 设为 0，则比率可以是偶数
 parent_time_step_ratio (max_dom)    = 1,   ! 父域与子域的时间步长比
                                              该值可以不同于 parent_grid_ratio
 feedback                            = 1,   ! 是否从子域反馈到父域，0 = 无反馈
 smooth_option                       = 2    ! 父域平滑选项，仅当 feedback 选项开启时使用
                                              0：无平滑
                                              1：1-2-1 平滑
                                              2：平滑-去平滑（默认）

WPS（气象前处理系统）输入文件中特定的 namelist 变量（用于真实数据模式）：  

 num_metgrid_soil_levels             = 4    ! metgrid（WPS）输入的土壤垂直层数
 num_metgrid_levels                  = 27   ! metgrid（WPS）输入的 3D 气象场垂直层数
 interp_type                         = 2    ! 垂直插值方法
                                              1 = 按压力线性插值
                                              2 = 按对数压力线性插值
 extrap_type                         = 2    ! 非温度变量的垂直外推
                                              1 = 使用最低两层外推
                                              2 = 低于地面时保持最低层为常数
 t_extrap_type                       = 2    ! 位温的垂直外推方法
                                              1 = 绝热
                                              2 = -6.5 K/km 温度递减率
                                              3 = 常数位温
 use_levels_below_ground             = .true.  ! 在垂直插值时是否使用低于输入地表的等压面数据
 use_surface                         = .true.  ! 是否在垂直插值和外推时使用输入的地表数据
 lagrange_order                      = 2    ! 垂直插值的阶数
                                              1 = 线性
                                              2 = 二次
                                              9 = 三次样条
zap_close_levels = 500            ! 如果等压面高度的气压差（Pa）小于 zap_close_levels，则忽略该等压面  
lowest_lev_from_sfc = .false.     ! 是否将地表值作为最低 eta 层  
                                  T = 使用地表值作为最低 eta 层（u, v, t, q）  
                                  F = 使用传统插值方法  
force_sfc_in_vinterp = 1          ! 在插值过程中，使用地表层作为下边界的 eta 层数  
                                  0 = 进行传统的截取插值  
                                  n = 前 n 个 eta 层直接使用地表层值  
maxw_horiz_pres_diff = 5000       ! 压力阈值（Pa）。在使用最大风速高度时，如果相邻值之间的气压差超过此最大值，  
                                  则该变量不会插入到用于垂直插值的列中。  
trop_horiz_pres_diff = 5000       ! 压力阈值（Pa）。在使用对流层顶高度时，如果相邻值之间的气压差超过此最大值，  
                                  则该变量不会插入到用于垂直插值的列中。  
maxw_above_this_level = 30000     ! 允许使用最大风速高度的最低气压（单位：Pa）。如果设定为 300 hPa，  
                                  则 500 hPa 处的最大风速值将被忽略。  
use_maxw_level                    ! 0 = 在 real 程序的垂直插值中不使用最大风速高度  
                                  1 = 使用该高度  
use_trop_level                    ! 与上面类似，适用于对流层顶高度数据  
sfcp_to_sfcp = .false.            ! 当输入数据仅包含地表气压和地形高度，而没有海平面气压（SLP）时，  
                                  计算模式地表气压的一种可选方法  
smooth_cg_topo = .false.          ! 平滑第 1 层网格区域的外层行列的地形，使其与输入数据更匹配  
use_tavg_for_tsk = .false.        ! 是否使用日平均地表温度作为皮肤温度。  
                                  日平均地表温度可通过 WPS 工具 avg_tsfc.exe 计算。  
                                  当 SKINTEMP 不存在时，可以使用此选项。  
aggregate_lu = .false.            ! 是否在主导土地利用类型中合并草地、灌木和树木；默认为 false。  
rh2qv_wrt_liquid = .true.         ! 是否计算相对于水（true）或冰（false）的相对湿度（RH）  
rh2qv_method = 1                  ! 计算相对湿度转换为混合比（qv）的方法：  
                                  1 = 旧 MM5 方法（默认）  
                                  2 = WMO 推荐方法（WMO-No. 49, 勘误表，2000 年 8 月）  
use_sh_qv = .false.               ! 是否使用输入数据中的比湿（specific humidity）或混合比（mixing ratio）  
                                  如果输入数据具有高垂直分辨率，建议使用此选项  
interp_theta = .false.            ! 如果设置为 .false.，则会对温度进行垂直插值，而不是位温（potential temperature），  
                                  这可能会减少与输入数据的偏差  
hypsometric_opt = 2               ! = 1: 默认方法  
                                  = 2: 使用另一种方法计算高度（与输入数据相比偏差更小）  
wif_input_opt = 0                 ! = 1: 处理 metgrid 中的水冰友好气溶胶（Water Ice Friendly Aerosol）输入数据，  
                                       用于 mp_physics=28  
                                  = 2: 自 WRF V4.4 起，使用黑碳气溶胶类别（mp_physics=28），  
                                       以及其辐射效应。必须在 WPS 过程中包含 QNWFA_QNIFA_QNBCA_SIGMA_MONTHLY.dat 文件。  
num_wif_levels = 30               ! Thompson 水冰友好气溶胶的层数（mp_physics=28）  
p_top_requested = 5000            ! 在模式中使用的 p_top（Pa）  
vert_refine_fact = 1              ! ndown 使用的垂直细化因子，不用于并行垂直网格细化  
vert_refine_method (max_dom) = 0  ! 垂直细化方法  
                                  0 = 不进行垂直细化  
                                  1 = 整数倍垂直细化（必须设置 rebalance=1）  
                                  2 = 使用指定或计算的 eta 层进行垂直细化（必须设置 rebalance=1）  

 ts_buf_size                         = 200      ! 时间序列缓冲区大小
 max_ts_locs                         = 5        ! 最大时间序列位置数
 max_ts_level                        = 15       ! 时间序列输出的最高模型层数
 tslist_unstagger_winds              = .false.  ! 是否插值 3D 风速 u、v 到网格中心


用户可以显式定义完整的 eta 层。给定的是 28 层和 35 层的两种分布。层数必须与分配的 eta 面数量（e_vert）一致。用户也可以仅请求层数（通过 e_vert），实际程序将计算值。有两种方法，通过 auto_levels_opt = 1（旧方法）或 2（新方法）选择。旧方法假设已知前几层，然后在模型顶部生成等高度的层。新方法具有表面和上层的拉伸因子（dz_stretch_s 和 dz_stretch_u），根据对数压力拉伸层，直到达到最大厚度（max_dz），并从厚度 dzbot 开始。拉伸从 dzstretch_s 过渡到 dzstretch_u，直到厚度达到 max_dz/2。

 max_dz                              = 1000.     ! 允许的最大层厚度（米）
 auto_levels_opt                     = 1         ! 旧方法
                                     = 2         ! 新方法（默认，还需设置 dzstretch_s, dzstretch_u, dzbot, max_dz）
 dzbot                               = 50.       ! 最低层的厚度（米），用于 auto_levels_opt=2
 dzstretch_s                         = 1.3       ! 表面拉伸因子，用于 auto_levels_opt=2
 dzstretch_u                         = 1.1       ! 上层拉伸因子，用于 auto_levels_opt=2

用户可以定义 eta 层，例如：
 eta_levels                          = 1.000, 0.990, 0.978, 0.964, 0.946,
                                       0.922, 0.894, 0.860, 0.817, 0.766,
                                       0.707, 0.644, 0.576, 0.507, 0.444,
                                       0.380, 0.324, 0.273, 0.228, 0.188,
                                       0.152, 0.121, 0.093, 0.069, 0.048,
                                       0.029, 0.014, 0.000,
 eta_levels                          = 1.000, 0.993, 0.983, 0.970, 0.954,
                                       0.934, 0.909, 0.880, 0.845, 0.807,
                                       0.765, 0.719, 0.672, 0.622, 0.571,
                                       0.520, 0.468, 0.420, 0.376, 0.335,
                                       0.298, 0.263, 0.231, 0.202, 0.175,
                                       0.150, 0.127, 0.106, 0.088, 0.070,
                                       0.055, 0.040, 0.026, 0.013, 0.000
                                       这允许在嵌套域中进行垂直嵌套请注意，对于垂直嵌套，只能使用RRTM和RRTMG辐射物理学

垂直嵌套的示例（在程序 real 中定义）：
 e_vert                              = 35,    45,
 eta_levels(1:35)                    = 1., 0.993, 0.983, 0.97, 0.954, 0.934, 0.909, 0.88, 0.8406663, 0.8013327,
                                       0.761999, 0.7226653, 0.6525755, 0.5877361, 0.5278192, 0.472514,
                                       0.4215262, 0.3745775, 0.3314044, 0.2917579, 0.2554026, 0.2221162,
                                       0.1916888, 0.1639222, 0.1386297, 0.1156351, 0.09525016, 0.07733481,
                                       0.06158983, 0.04775231, 0.03559115, 0.02490328, 0.0155102, 0.007255059, 0.
 eta_levels(36:81)                   = 1.0000, 0.9946, 0.9875, 0.9789, 0.9685, 0.9562, 0.9413, 0.9238, 0.9037, 0.8813, 0.8514,
                                       0.8210, 0.7906, 0.7602, 0.7298, 0.6812, 0.6290, 0.5796, 0.5333, 0.4901, 0.4493, 0.4109,
                                       0.3746, 0.3412, 0.3098, 0.2802, 0.2524, 0.2267, 0.2028, 0.1803, 0.1593, 0.1398, 0.1219,
                                       0.1054, 0.0904, 0.0766, 0.0645, 0.0534, 0.0433, 0.0341, 0.0259, 0.0185, 0.0118, 0.0056, 0.


理想化初始化方法：
 ideal_init_method                   = 1, 从 phb 计算 alb（默认）；= 2, 从 t_init 计算 alb


水平插值选项，粗网格到细网格。默认使用 Smolarkiewicz 的 "SINT" 方法。然而，已知在 WRF 中实现时，对于大的细化比例（如 15:1）会失效。对于这些极端情况（非常罕见），可以使用其他方案。对于选项 1、3、4 和 12，FG 侧边界使用相同的水平方案进行侧边界计算。
 interp_method_type                  = 1 ! 双线性插值
                                     = 2 ! SINT（默认）
                                     = 3 ! 最近邻插值 - 仅用于测试
                                     = 4 ! 重叠二次插值
                                     =12 ! 再次用于测试，使用 SINT 水平插值，并使用相同方案计算 FG 侧边界

3D 海洋初始化的变量，使用单一剖面。设置海洋物理选项为 #2。指定层数。对于每一层，提供表面以下的深度（米）。在每个深度提供温度（K）和盐度（ppt）。默认不使用 3D 海洋。即使激活了 3D 海洋，用户也必须指定合理的海洋。目前，这是运行 3D 海洋选项的唯一方法。
 &physics
 sf_ocean_physics                    = 激活海洋模型（0=否，1=1D 混合层；2=3D PWP，无地形）

 &domains
 ocean_levels                        = 30,
 ocean_z                             ; 海洋层深度的垂直剖面（米），例如：
                                     =       5.,       15.,       25.,       35.,       45.,       55.,
                                            65.,       75.,       85.,       95.,      105.,      115.,
                                           125.,      135.,      145.,      155.,      165.,      175.,
                                           185.,      195.,      210.,      230.,      250.,      270.,
                                           290.,      310.,      330.,      350.,      370.,      390.
 ocean_t                             ; 海洋温度的垂直剖面，例如：
                                     = 302.3493,  302.3493,  302.3493,  302.1055,  301.9763,  301.6818,
                                       301.2220,  300.7531,  300.1200,  299.4778,  298.7443,  297.9194,
                                       297.0883,  296.1443,  295.1941,  294.1979,  293.1558,  292.1136,
                                       291.0714,  290.0293,  288.7377,  287.1967,  285.6557,  284.8503,
                                       284.0450,  283.4316,  283.0102,  282.5888,  282.1674,  281.7461
 ocean_s                             ; 盐度的垂直剖面，例如：
                                     =  34.0127,   34.0127,   34.0127,   34.3217,   34.2624,   34.2632,
                                        34.3240,   34.3824,   34.3980,   34.4113,   34.4220,   34.4303,
                                        34.6173,   34.6409,   34.6535,   34.6550,   34.6565,   34.6527,
                                        34.6490,   34.6446,   34.6396,   34.6347,   34.6297,   34.6247,
                                        34.6490,   34.6446,   34.6396,   34.6347,   34.6297,   34.6247
                                

控制指定移动嵌套的 namelist 变量：
请注意，此移动嵌套选项需要在编译时通过添加-DMVE_NESTS来激活
到拱门。最大移动次数max_moves设置为50
但可以在源代码文件frame/module_drive_constants.F.中修改
 num_moves                           = 0                 ! 总移动次数
 move_id(max_moves)                  = 2,2,2,2,          ! 每次移动的嵌套域 ID 列表
 move_interval(max_moves)            = 60,120,150,180,   ! 从该域开始后的移动时间（分钟）
 move_cd_x(max_moves)                = 1,1,0,-1,         ! 在 i 方向上移动的父域网格单元数
 move_cd_y(max_moves)                = 1,0,-1,1,         ! 在 j 方向上移动的父域网格单元数
                                                           正数表示向 i 和 j 方向增加，负数表示向 i 和 j 方向减少。
                                                           0 表示不移动。当前限制是每次移动只能移动 1 个网格单元。

控制自动移动嵌套的 namelist 变量：
请注意，此移动嵌套选项需要在编译时通过添加-DMVE_NESTS来激活
以及将DVORTEX_CENTER转换为ARCHFLAGS。此选项使用中级涡流跟踪算法
确定巢穴移动。这个选项是实验性的。
vortex_interval(max_dom) = 15         ! 计算新涡旋位置的时间间隔（分钟）  
max_vortex_speed(max_dom) = 40        ! 用于计算新涡旋位置的搜索半径（单位：m/s）  
corral_dist(max_dom) = 8              ! 移动嵌套网格允许接近母网格边界的最小粗网格单元数  
track_level = 50000                   ! 追踪涡旋的气压层（单位：Pa）  
time_to_move(max_dom) = 0.            ! 允许移动嵌套网格的开始时间（单位：分钟）  

tile_sz_x = 0,                        ! x 方向上的网格块数  
tile_sz_y = 0,                        ! y 方向上的网格块数  
                                      ! 这些值可以自动确定  
numtiles = 1,                         ! 每个计算单元（patch）上的网格块数（可替代上面两个变量）  
nproc_x = -1,                         ! x 方向上的处理器数量（用于网格分解）  
nproc_y = -1,                         ! y 方向上的处理器数量（用于网格分解）  
                                      ! -1：代码会自动分解网格  
                                      ! >1：指定的处理器数量会用于分解网格  

自适应时间步长（adaptive time step）控制的 namelist 变量：
use_adaptive_time_step = .false.      ! 是否启用自适应时间步长（T=启用，F=禁用）  
step_to_output_time = .true.          ! 如果启用自适应时间步长，是否调整时间步长以精确匹配历史输出时间  
target_cfl(max_dom) = 1.2,1.2         ! 目标 CFL 数（垂直和水平），如果 max(垂直 CFL, 水平 CFL) ≤ 该值，则无需减少时间步长，可适当增加  
target_hcfl(max_dom) = .84,.84        ! 目标水平 CFL 数，如果水平 CFL ≤ 该值，则无需减少时间步长  
max_step_increase_pct(max_dom) = 5,51 ! 允许的最大时间步长增长百分比  
                                      ! 如果 max(垂直 CFL, 水平 CFL) ≤ target_cfl，则时间步长按此比例增加  
                                      ! 对于嵌套网格，建议使用较大的值（如 51%）  
starting_time_step(max_dom) = -1,-1   ! 初始时间步长  
                                      ! -1：使用默认的 4×dx（在 start_em 中定义）  
                                      ! 100：粗网格的初始时间步长为 100 秒  
max_time_step(max_dom) = -1,-1        ! 最大时间步长  
                                      ! -1：默认最大时间步长为 8×dx  
                                      ! 100：时间步长不会超过 100 秒  
min_time_step(max_dom) = -1,-1        ! 最小时间步长  
                                      ! -1：默认最小时间步长为 3×dx  
                                      ! 100：时间步长不会小于 100 秒  
adaptation_domain = 1                 ! 自适应时间步长的控制域  
                                      ! 1 = 默认设置，所有细网格的自适应时间步长由粗网格驱动  
                                      ! 2 = 细网格域 #2 决定整个模拟的自适应时间步长  

数字滤波初始化（DFI）控制
 &dfi_control
 dfi_opt                             = 0        ! 使用哪种 DFI 选项（推荐使用 3）
                                                    0 = 无数字滤波初始化
                                                    1 = 数字滤波启动（DFL）
                                                    2 = 非绝热 DFI（DDFI）
                                                    3 = 两次 DFI（TDFI）
 dfi_nfilter                         = 7        ! 使用的数字滤波器类型（推荐使用 7）
                                                    0 = 均匀
                                                    1 = Lanczos
                                                    2 = Hamming
                                                    3 = Blackman
                                                    4 = Kaiser
                                                    5 = Potter
                                                    6 = Dolph 窗口
                                                    7 = Dolph
                                                    8 = 递归高阶
 dfi_write_filtered_input            = .true.   ! 是否在开始预报前写入过滤后的 wrfinput 文件
 dfi_write_dfi_history               = .false.  ! 是否在滤波积分期间写入 wrfout 文件
 dfi_cutoff_seconds                  = 3600     ! 滤波器的截止周期（秒）
 dfi_time_dim                        = 1000     ! 滤波期间的最大时间步数
 dfi_bckstop_year                    = 2004     ! 反向 DFI 积分的停止年份
 dfi_bckstop_month                   = 03       ! 反向 DFI 积分的停止月份
 dfi_bckstop_day                     = 14       ! 反向 DFI 积分的停止日
 dfi_bckstop_hour                    = 12       ! 反向 DFI 积分的停止小时
 dfi_bckstop_minute                  = 00       ! 反向 DFI 积分的停止分钟
 dfi_bckstop_second                  = 00       ! 反向 DFI 积分的停止秒
 dfi_fwdstop_year                    = 2004     ! 正向 DFI 积分的停止年份
 dfi_fwdstop_month                   = 03       ! 正向 DFI 积分的停止月份
 dfi_fwdstop_day                     = 13       ! 正向 DFI 积分的停止日
 dfi_fwdstop_hour                    = 12       ! 正向 DFI 积分的停止小时
 dfi_fwdstop_minute                  = 00       ! 正向 DFI 积分的停止分钟
 dfi_fwdstop_second                  = 00       ! 正向 DFI 积分的停止秒
 dfi_savehydmeteors                  = 0        ! 雷达数据同化选项：0：在 DFI 前将水凝物设为 0 并让其在 DFI 中自旋；1：在 DFI 中保持不变


物理选项：
注意：即使物理选项在不同的嵌套域中也可能不同，
必须谨慎使用哪些选项是明智的
 &physics
 chem_opt (max_dom)                  = 0,       ! 化学选项 - 使用 WRF-Chem
 mp_physics (max_dom)                = 微物理选项
                                     = 0, 无微物理
                                     = 1, Kessler 方案
                                     = 2, Lin 等人方案
                                     = 3, WSM 3 类简单冰方案
                                     = 4, WSM 5 类方案
                                     = 5, Ferrier（新 Eta）微物理，操作高分辨率窗口版本
                                     = 6, WSM 6 类霰方案
                                     = 7, Goddard 4 冰方案
                                     = 8, Thompson 方案
                                     = 9, Milbrandt-Yau 2 矩方案
                                     = 10, Morrison（2 矩）
                                     = 11, CAM 5.1 微物理
                                     = 13, SBU_YLIN 方案
                                     = 14, WDM 5 类方案
                                     = 16, WDM 6 类方案
                                     = 18, NSSL 2 矩 4 冰方案，预测（未激活）CCN（或激活 CCN）
                                          要更改全局 CCN 值，请使用
                                           nssl_cccn  = 0.7e9   ; NSSL 方案（18）或 nssl_ccn_on=1 的海平面压力下的 CCN（#/m^3）
                                          同时设置 mp_physics=18 的 ccn_conc
                                      对于 NSSL 1 矩方案，可以设置雪、霰、雹和雨的截距和粒子密度。对于 1 矩和 2 矩方案，可以设置霰和雹的形状参数。
                                      请参阅 doc/README.NSSLmp 以获取影响 NSSL 方案的选项
                                     = 17, 19, 21, 22: 旧版 NSSL-MP 选项：请参阅 README.NSSLmp 以获取与 18 等效的设置
                                     = 24, WSM 7 类方案（单独的雹和霰类别）
                                     = 26, WDM 7 类方案（单独的雹和霰类别）
                                     = 28, 具有水和冰友好气溶胶气候学的 Thompson 方案
                                       此选项有两个气候气溶胶输入选项：
                                       use_aero_icbc = .F. : 使用常数值
                                       use_aero_icbc = .T. : 使用来自 WPS 的气候气溶胶输入
 use_rap_aero_icbc                   = .false.    ! 设置为 .true. 以摄取包含气溶胶的实时数据（4.4 新增）

nssl_ccn_on=1  
                                           该选项同时为 mp_physics=18 设置 ccn_conc（云凝结核浓度）。  
                                       对于 NSSL 1-矩（1-moment）方案，可以设置雪、霰、冰雹和雨滴的截距和粒子密度。  
                                       对于 1- 和 2-矩方案，可以设置霰和冰雹的形状参数。  
                                       请查看 doc/README.NSSLmp 以获取影响 NSSL 方案的选项。  
                                     = 17, 19, 21, 22: 传统的 NSSL 微物理选项，详情请见 README.NSSLmp，  
                                       这些选项在 mp_physics=18 中有等效设置。  
                                     = 24, WSM 7 类微物理方案（冰雹和霰类别分开）。  
                                     = 26, WDM 7 类微物理方案（冰雹和霰类别分开）。  
                                     = 28, 具有水相和冰相气溶胶气候学的气溶胶感知 Thompson 方案。  
                                       该选项有两种气溶胶气候输入方式：  
                                       use_aero_icbc = .F. ：使用固定值。  
                                       use_aero_icbc = .T. ：使用 WPS 提供的气溶胶气候输入数据。  
 use_rap_aero_icbc                   = .false.    ! 设置为 .true. 以摄入包含气溶胶的实时数据（WRF 4.4 版本新增）。  
 qna_update                          = 0          ! 设置为 1 时，可从气候数据或实时数据更新随时间变化的地表气溶胶排放，  
                                                  适用于 mp_physics = 28。  
                                                  需要输入文件 'wrfqnainp_d0*'，  
                                                  还需设置 auxinput17_interval 和 io_form_auxinput17（WRF 4.4 版本新增）。  
 wif_fire_emit                       = .false.    ! 设置为 .true. 以在 mp_physics=28 时纳入生物质燃烧生成的有机和黑碳气溶胶（WRF 4.4 版本新增）。  
 wif_fire_inj                        = 1          ! （默认值）在 mp_physics=28 中，垂直分布生物质燃烧排放（WRF 4.4 版本新增）。  
                                     = 30, HUJI（以色列耶路撒冷希伯来大学）光谱微物理方案，快速版本。  
                                     = 32, HUJI 光谱微物理方案，完整版本。  
                                     = 38, 具有 2-矩霰/冰雹的 Thompson 方案（WRF 4.5 版本新增）。  
                                     = 40, Morrison 2-矩方案，并考虑 CESM-NCSU RCP4.5 气溶胶气候数据。  
                                     = 50, P3 1 类冰相，1-矩云水。  
                                     = 51, P3 1 类冰相 + 2-矩云水。  
                                     = 52, P3 2 类冰相 + 2-矩云水。  
                                     = 53, P3 1 类冰相 + 3-矩冰相 + 2-矩云水。  
                                     = 55, Jensen-ISHMAEL（Ice-Spheroids Habit Model with Aspect-ratio Evolution）  
                                           方案，预测 qc、qr 以及 3 类冰相，每种冰相具有 4 个预测方程。  
                                     = 56, NTU 多矩方案（ntu3m）。  
 ccnty                                 = 1        ! ntu3m 的气溶胶背景类型，1 = 海洋环境。  
                                         2        ! 2 = 大陆清洁环境（默认）。  
                                         3        ! 3 = 大陆平均环境。  
                                         4        ! 4 = 大陆城市环境。  

                                     = 95, Ferrier（旧 Eta）微物理方案。  
                                     = 96, Madwrf 微物理方案。  
                                     = 97, Goddard GCE 方案（同时使用 gsfcgce_hail 和 gsfcgce_2ice 选项）。  

对于非零 mp_physics 选项，为了保持 Qv（气态水混合比）≥ 0，并确保其他水分变量小于临界值时设为零。


mp_zero_out = 0,            ! 无操作，不调整任何湿度字段  
             = 1,           ! 除 Qv 之外，如果其他湿度数组低于临界值，则设为零（仅适用于‘湿度’数组）  
             = 2,           ! Qv 保持 ≥ 0，其他湿度数组低于临界值时设为零（仅适用于‘湿度’数组）  

mp_zero_out_thresh = 1.e-8  ! 湿度数组阈值，低于此值的湿度数组（除 Qv 外）设为零（单位 kg/kg）  
mp_zero_out_all = 0,        ! 若设为 1 且 mp_zero_out > 0，则恢复旧行为，
                            并将阈值应用于标量、化学和示踪数组  

gsfcgce_hail    = 0         ! 运行 gsfcgce 微物理方案，使用霰  
                = 1         ! 运行 gsfcgce 微物理方案，使用冰雹（默认值 = 0）  

gsfcgce_2ice    = 0         ! 运行时包括雪、冰和霰/冰雹  
                = 1         ! 仅使用冰和雪  
                = 2         ! 仅使用冰和霰（仅用于极端情况）（默认值 = 0）  
                            ! 若 gsfcgce_2ice 设为 1 或 2，则忽略 gsfcgce_hail  

hail_opt = 0                ! WSM6 和 WDM6 方案中的冰雹开关：0 - 关闭，1 - 开启  
morr_rimed_ice = 1          ! Morrison 方案（mp_physics=10 和 40）
                            中的冰雹开关：0 - 关闭，1 - 开启（4.0 版新增）  

clean_atm_diag = 0          ! 若设为 1，则开启清洁大气诊断（用于化学）；默认值为 0（关闭）  
acc_phy_tend = 0            ! 设为 1 以输出 16 种累积物理趋势，包括位温、水汽混合比及 U/V 风分量；
                            默认值为 0（关闭）（4.4 版新增）  

progn (max_dom) = 0         ! 是否使用混合激活方案（仅适用于 Morrison、WDM6、WDM5 和 NSSL_2MOM）  
ccn_conc = 1.E8             ! 云凝结核（CCN）浓度，WDM 方案使用（NSSL_2MOM 方案会自动设置 nssl_cccn）  

no_mp_heating = 0           ! 正常  
                 = 1        ! 关闭微物理方案中的潜热释放  

use_mp_re = 1               ! 是否在 RRTMG 方案中使用微物理方案计算的有效半径  
                            0：不使用；1：使用  
                            ! 适用于以下微物理方案：3,4,6,7,8,10,14,16,18,24,26,28,50-53,55  

force_read_thompson = .false.       ! 是否读取 mp_physics = 8,28 的查找表  
write_thompson_tables = .true.      ! 是否读取或计算 mp_physics = 8,28 的查找表  
write_thompson_mp38table = .false.  ! 是否读取 mp_physics = 38 的查找表（qr_acr_qg_mp38V1.dat）  

ra_lw_physics (max_dom)             长波辐射选项
                                     = 0, 无长波辐射
                                     = 1, RRTM 方案
                                       （WRF 4.2 版本的默认温室气体（GHG）值：
                                         co2vmr = (280. + 90.*exp(0.02*(yr-2000)))*1.e-6
                                         n2ovmr = 319.e-9, ch4vmr = 1774.e-9
                                         之前版本使用的值：
                                         co2vmr = 330.e-6, n2ovmr = 0., ch4vmr = 0.）
                                     = 3, CAM 方案
                                          还需要设置 levsiz、paerlev、cam_abs_dim1/2（详见下方）
                                     = 4, RRTMG 方案
                                       （WRF 4.2 版本的默认温室气体（GHG）值：
                                         co2vmr = (280. + 90.*exp(0.02*(yr-2000)))*1.e-6
                                         n2ovmr = 319.e-9, ch4vmr = 1774.e-9,
                                         cfc11 = 0.251e-9, cfc12 = 0.538e-9）
                                     = 14, RRTMG-K 方案（来自 KIAPS）
                                     = 24, GPU 和 MIC 加速的快速 RRTMG 方案（自 WRF 3.7 版本起）
                                       （WRF 4.2 版本的默认温室气体（GHG）值：
                                         co2vmr = (280. + 90.*exp(0.02*(yr-2000)))*1.e-6
                                         n2ovmr = 319.e-9, ch4vmr = 1774.e-9,
                                         cfc11 = 0.251e-9, cfc12 = 0.538e-9）
                                     = 5, Goddard 长波辐射方案
                                     = 7, FLG（UCLA）方案
                                     = 31, Earth Held-Suarez 强迫方案
                                     = 99, GFDL（Eta）长波辐射方案（半支持）
                                          若使用 ARW，必须同时设置 co2tf = 1

ra_sw_physics (max_dom)             短波辐射选项
                                     = 0, 无短波辐射
                                     = 1, Dudhia 方案
                                     = 2, Goddard 短波辐射方案
                                     = 3, CAM 方案
                                          还需要设置 levsiz、paerlev、cam_abs_dim1/2（详见下方）
                                     = 4, RRTMG 方案
                                     = 14, RRTMG-K 方案（来自 KIAPS）
                                     = 24, GPU 和 MIC 加速的快速 RRTMG 方案（自 WRF 3.7 版本起）
                                     = 5, Goddard 短波辐射方案
                                     = 7, FLG（UCLA）方案
                                     = 99, GFDL（Eta）短波辐射方案（半支持）
                                          若使用 ARW，必须同时设置 co2tf = 1

radt (max_dom)                      = 30,  ! 辐射物理过程调用的时间间隔（分钟）
                                              建议时间间隔约为 dx（网格间距）的 1 倍（例如 10 km 时设为 10 min）；
                                              所有嵌套网格应使用相同的值。

cldovrlp                            = 2,   ! 仅适用于 RRTMG 的云重叠选项
                                              1 = 随机重叠
                                              2 = 最大-随机重叠（默认）
                                              3 = 最大重叠
                                              4 = 指数重叠
                                              5 = 指数-随机重叠

idcor                               = 0,   ! 适用于 cldovrlp=4 或 5 的去相关长度标志
                                              0 = 固定去相关长度 2500 m
                                              1 = 随纬度变化的去相关长度

ra_sw_eclipse                       = 0,   ! 是否考虑日食对短波辐射的影响
                                              0 = 关闭
                                              1 = 开启
                                              适用于 RRTMG、Goddard、旧 Goddard 和 Dudhia 方案

ghg_input                           = 1,   ! 读取 CAMtr_volume_mixing_ratio 文件以获取温室气体数据的选项
                                              WRF 4.4 版本的默认值为 SSP 2 与 RCP 4.5 结合，即 SSP245
                                              适用于 CAM LW 和 SW、RRTM、RRTMG LW 和 SW、RRTMG_fast LW 和 SW
                                              0 = 不读取年度数据
                                              1 = 读取 CO2、N2O、CH4、CFC11、CFC12 的时间变化数据


co2tf             ! 仅适用于 GFDL 辐射的 CO2 传输函数选项  
                  = 0,  ! 从预生成文件读取 CO2 数据  
                  = 1,  ! 在模拟过程中内部生成 CO2 数据  

ra_call_offset    ! 辐射计算的偏移量  
                  = 0   ! 无偏移  
                  = -1  ! 旧版偏移  

swint_opt         ! 基于太阳天顶角的短波辐射插值选项  
                  = 0,  ! 无插值  
                  = 1,  ! 进行插值  
                  = 2,  ! 每个时间步调用 FAST All-Sky Radiation Model for Solar（FARMS）  

couple_farms = .false.  ! 是否使用 FARMS 短波辐射驱动地面过程（True: 用 FARMS；False: 用 rad_sw_physics 方案）  
cam_abs_freq_s                      = 21600   ! 默认的CAM晴空长波吸收计算频率 (建议的最小值以加速方案)
levsiz                              = 59      ! 用于CAM辐射输入的臭氧层级，自动设置
paerlev                             = 29      ! 用于CAM辐射输入的气溶胶层级，自动设置 
cam_abs_dim1                        = 4       ! 用于CAM吸收保存数组，自动设置 
cam_abs_dim2                        = 对CAM第二吸收保存数组的e_vert值，自动设置

o3input                             = 辐射的臭氧输入选项（目前仅限rrtmg）
                                      = 0,    ! 使用代码内部的剖面
                                      = 2,    ! 使用CAM臭氧数据（ozone.formatted）
aer_opt                             = 辐射的气溶胶输入选项（目前仅限rrtmg）
                                      = 0,    ! 无
                                      = 1,    ! 使用Tegen (1997)数据, 
                                      = 2,    ! 使用J. A. Ruiz-Arias方法（参见其他aer_*选项） 
                                      = 3,    ! 使用G. Thompson的水/冰友好的气候学气溶胶 
alevsiz                             = 12      ! 用于Tegen气溶胶输入层级，自动设置 
no_src_types                        = 6       ! 用于Tegen气溶胶：有机碳和黑碳、海盐、硫酸盐、尘土，和 stratospheric aerosol（火山灰 - 当前为0），自动设置

以下气溶胶选项允许RRTMG和新的Goddard辐射方案看到，但气溶胶在模型集成过程中是常数。
aer_aod550_opt (max_dom)            = [1,2] 
                                       1 = 从namelist中输入550nm处的AOD常数值。
                                           在这种情况下，值从aer_aod550_val读取；
                                       2 = 从辅助输入15中输入值。这是一个随时间变化的2D网格，采用netcdf格式
                                           wrf兼容。默认值是aer_aod550_opt=1和aer_aod550_val=0.12
aer_aod550_val (max_dom)            = 0.12
aer_angexp_opt (max_dom)            = [1,2,3] :
                                       1 = 从namelist中输入常数的Angstrom指数值。 在这种情况下， 
                                           值从aer_angexp_val读取；
                                       2 = 从辅助输入15中输入值，类似于aer_aod550_opt；
                                       3 = 根据aer_type定义的气溶胶类型估算Angstrom指数值，并与
                                           WRF中的RH调制。默认操作是aer_angexp_opt = 1，aer_angexp_val=1.3。
aer_angexp_val (max_dom)            = 1.3   
aer_ssa_opt (max_dom)               = [1,2,3] 类似于aer_angexp_opt。
aer_ssa_val (max_dom)               = 0.85
aer_asy_opt (max_dom)               = [1,2,3] 类似于aer_angexp_opt。
aer_asy_val (max_dom)               = 0.9
aer_type (max_dom)                  = [1,2,3] : 1 表示乡村，2 表示城市，3 表示海洋。

sf_sfclay_physics (max_dom)         地面层选项（旧的bl_sfclay_physics选项）
                                     = 0, 无地面层
                                     = 1, 修订的MM5 Monin-Obukhov方案（Jimenez，在v3.6中重命名）
                                     = 2, Monin-Obukhov (Janjic)方案
                                     = 3, NCEP全球预报系统方案 
                                     = 4, QNSE地面层
                                     = 5, MYNN地面层
                                     = 7, Pleim-Xiu地面层
                                     = 10, TEMF地面层
                                     = 91, 旧的MM5方案（以前是选项1）

sf_surface_physics (max_dom)        土地表面选项（旧的bl_surface_physics选项）
                                     = 0, 无表面温度预测
                                     = 1, 热扩散方案
                                     = 2, 统一的Noah陆地表面模型
                                     = 3, RUC陆地表面模型
                                     = 4, Noah-MP陆地表面模型（参见额外的&noah_mp namelist）
                                     = 5, 社区陆地模型版本4（CLM4），改编自CAM
                                     = 6, 社区陆地系统模型（CTSM）。有关如何运行此选项的信息，请参见doc/README.CTSM。
                                     = 7, Pleim-Xiu LSM
                                     = 8, 简化的简易生物圈模型（SSiB） 
                                          - 可以与Dudhia/RRTM、CAM或RRTMG辐射选项一起使用

sf_urban_physics(max_dom)           = 0, ! 激活城市冠层模型（仅在Noah和Noah-MP LSM中）
                                     = 0: 无
                                     = 1: 单层，UCM 
                                     = 2: 多层，建筑环境参数化（BEP）方案 
                                          （仅与YSU, MYJ和BouLac PBL工作）
                                     = 3: 多层，建筑环境模型（BEM）方案 
                                          （仅与YSU, MYJ和BouLac PBL工作）

num_urban_ndm                       = 1! (=  2如果BEP或BEM激活) 最大街道维度数（BEP或BEM头文件中的ndm）
num_urban_ng                        = 1! (= 10如果BEP或BEM激活) 地面网格层数（BEP或BEM头文件中的ng_u）
num_urban_nwr                       = 1! (= 10如果BEP或BEM激活) 墙面或屋顶的网格层数（BEP或BEM头文件中的nwr_u）
num_urban_nz                        = 1! (= 18如果BEP或BEM激活) 城市网格的最大垂直层数（BEP或BEM头文件中的nz_um）
num_urban_ngb                       = 1! (= 10如果BEM激活)        建筑物下方的地面网格层数（BEM头文件中的ngb_u）
num_urban_nf                        = 1! (= 10如果BEM激活)        楼层的网格层数（BEM头文件中的nf_u）
num_urban_nbui                      = 1! (= 15如果BEM激活)        每个城市类别的最大建筑物类型数（BEM头文件中的nbui_max）

sf_surf_irr_scheme (max_dom)        = 0: 无灌溉方案
                                     = 1: 渠道方案（仅在Noah中）
                                     = 2: 滴灌方案
                                     = 3: 喷灌方案
 
irr_daily_amount (max_dom)          = 每日等效灌溉水量，单位为毫米（浮动值，例如5.7）
irr_start_hour (max_dom)            = 灌溉的UTC开始小时（整数，例如5）
irr_num_hours (max_dom)             = 灌溉的连续小时数（整数，例如3）
irr_start_julianday (max_dom)       = 灌溉开始的儒略日（整数，例如135）
irr_end_julianday (max_dom)         = 灌溉结束的儒略日（整数，例如255）
irr_ph (max_dom)                    = 0: 激活所有静态灌溉场
                                     = 1: 假随机激活场：确保在不同编译器中的重复性
                                     = 2: 随机激活场：结果可能依赖于编译器的fortran RANDOM_SEED
irr_freq (max_dom)                  = 灌溉频率（天数）（整数，例如3）

bl_pbl_physics (max_dom)            边界层选项
                                     = 0, 无边界层 
                                     = 1, YSU方案
                                     = 2, Mellor-Yamada-Janjic TKE方案
                                     = 3, 混合EDMF GFS方案 
                                     = 4, Eddy-diffusivity质量通量，准正态尺度消除边界层
                                     = 5, MYNN 2.5级TKE方案，适用于
                                          sf_sfclay_physics=1或2以及5
                                          （选项6在4.5中被删除，已由bl_mynn_closure选项替代）
bl_mynn_closure                          = 2.5: 等级2.5
                                            2.6: 等级2.6
                                            3.0: 等级3.0                                         
                                     = 7, ACM2 (Pleim) PBL
                                     = 8, Bougeault 和 Lacarrere (BouLac) PBL
                                     = 9, 来自 CAM5 (CESM 1_0_1) 的 UW 边界层方案
                                     = 10, TEMF (总能量质量流) 方案
                                          sf_sfclay_physics=10
                                     = 11, Shin-Hong ‘尺度感知’ PBL 方案
                                     = 12, Grenier-Bretherton-McCaa 方案
                                     = 16, TKE+TKE 消散率 (epsilon) 方案
                                           适用于表面层选项 1,91,2,5.
                                     = 17, TKE+TKE 消散率+TPE (温度方差) 方案
                                     = 99, MRF 方案

 bldt (max_dom)                      = 0,       ! 边界层物理调用的分钟间隔

 grav_settling (max_dom)             云雾/云滴的重力沉降 (现在适用于任何 PBL 方案)
 				     = 0, 不沉降云滴
				     = 1, 来自 Dyunkerke 1991（在大气和地面上）
				     = 2, Fogdes（依赖于植被和风速；Katata 等 2008）在地面上 
                                          和 Dyunkerke 在大气中。
 ysu_topdown_pblmix                  = 0,       ! 是否启用自顶向下的辐射驱动混合（1=是）
 mfshconv (max_dom)                  = 1,       ! 是否启用新白天 EDMF QNSE（0=否）
 topo_wind (max_dom)                 = 0,       ! 关闭，
                                     = 1,    启用 Jimenez 的地形表面风修正 
                                             （仅限 YSU PBL，并需要来自 geogrid 的额外输入）
                                     = 2,    启用 Mass 的地形表面风修正（仅限 YSU PBL）

 tke_budget (max_dom)                = 0,       ! 默认关闭；= 1 启用 MYNN tke 预算项输出（4.5 版本中新功能）
                                                  （替换以前版本中的 bl_mynn_tkebudget）
 bl_mynn_tkeadvect (max_dom)         = .false., ! 默认关闭；= .true. 启用 MYNN tke 对流输送
 icloud_bl                                        选项用于将 PBL 方案中的亚网格云（仅 MYNN）耦合到辐射方案
                                                  0: 不耦合；1: 启用耦合到辐射（默认）
 bl_mynn_cloudmix (max_dom)          = 0        ! 默认关闭；=1 启用 MYNN 中 qc 和 qi 的混合
                                     0: 不混合 qc 和 qi；1: 启用混合（默认）。 
                                        当 scalar_pblmix=1 时，qnc 和 qni 会被混合。
 bl_mynn_mixlength                   选项用于更改 MYNN 中的混合长度公式
                                     0: 原始公式，参照 Nakanishi 和 Niino 2009， 
                                     1: RAP/HRRR（包括自由大气中的 BouLac）， 
                                     2: 实验性（默认；包括云特定混合长度和尺度感知混合长度，参考 Ito 等 2015，BLM）。选项 2 已通过测试 
                                        与 EDMF 选项结合使用。
 bl_mynn_cloudpdf                    选项用于切换不同的云 PDF 来表示亚网格云
                                     0: 原始（Sommeria 和 Deardorf 1977）； 
                                     1: Kuwano 等 2010，与选项 0 相似，但使用解析尺度梯度 
                                        代替高阶矩； 
                                     2: 来自 Chaboureau 和 Bechtold（2002, JAS，修改版，默认） 
 bl_mynn_edmf (max_dom)              = 1    ! 启用 MYNN 中的质量流选项，0: 关闭质量流选项
                                              相关（隐藏）选项： 
 bl_mynn_edmf_mom (max_dom)          = 1    ! - 启用 MYNN 质量流方案中的动量传输 
                                            （假设 bl_mynn_edmf > 0）； 0=关闭（默认）
                                       0      不启用动量传输；1: 启用动量传输（默认）
 bl_mynn_edmf_tke (max_dom)          = 0,   ! 默认；= 1 启用 MYNN 质量流方案中的 TKE 传输 
                                              （假设 bl_mynn_edmf > 0）
                                       0      不启用 TKE 传输（默认）；1: 启用 TKE 传输
 bl_mynn_output (max_dom)            = 0,   ! 不输出额外的数组
                                       1      分配并输出 MYNN PBL 的额外 3D 数组
 bl_mynn_mixscalars (max_dom)        = 0    ! 关闭，1: 启用标量混合
 bl_mynn_mixqt (max_dom)             = 0    ! 分别混合湿度物种，1: 混合总水量

 scalar_pblmix (max_dom)             = 1    ! 按照 PBL 选项（exch_h）一致地混合标量场
 tracer_pblmix (max_dom)             = 1    ! 按照 PBL 选项（exch_h）一致地混合示踪场
 shinhong_tke_diag (max_dom)         = 0    ! 来自 Shin-Hong PBL 的诊断 TKE 和混合长度
 opt_thcnd                                    选项用于在 Noah LSM 中处理热导率
                                     = 1, 原始（默认）
                                     = 2, McCumber 和 Pielke 用于粘土和沙质壤土
 sf_surface_mosaic                    选项用于在 Noah LSM 中马赛克化土地使用类别            
                                     = 0    ! 默认；仅使用主导类别
                                     = 1    ! 使用马赛克土地使用类别
 mosaic_cat                          = 3    ! 一个网格单元中土地使用类别的数量
 mosaic_lu                           = 0,   ! 默认 ;  设置为 = 1 以在 RUC 中使用马赛克土地使用类别
 mosaic_soil                         = 0,   ! 默认 ;  设置为 = 1 以在 RUC 中使用马赛克土壤类别
 flag_sm_adj                         = 0,   ! 默认 ;  设置为 = 1 在 real.exe 中调整 RUC 的土壤水分 
                                            （例如，当 RUC 从 GFS LSM 数据初始化时） 

 cu_physics (max_dom)                云物理方案
                                     = 0, 不计算对流
                                     = 1, Kain-Fritsch（新 Eta）方案
                                     = 2, Betts-Miller-Janjic 方案
                                     = 3, Grell-Freitas 集合方案
                                     = 4, 尺度感知 GFS 简化 Arakawa-Schubert（SAS）方案 
                                     = 5, Grell 3D 集合方案
                                     = 6, 修改版 Tiedtke 方案
                                     = 7, 来自 CAM5 (CESM 1_0_1) 的 Zhang-McFarlane 方案
                                     = 10, 基于 PDF 的触发函数修改版 Kain-Fritsch 方案
                                     = 11, 多尺度 Kain-Fritsch 方案
                                     = 14, KIM 简化 Arakawa-Schubert 方案（KSAS），适用于灰区分辨率
                                     = 16, 新版 Tiedtke 方案
                                     = 94, 2015 GFS 简化 Arakawa-Schubert 方案（来自 HWRF，请小心使用） 
                                     = 95, 以前的 GFS 简化 Arakawa-Schubert 方案（来自 HWRF，请小心使用） 
                                     = 96, 以前新版 GFS 简化 Arakawa-Schubert 方案来自 YSU
                                     = 93, Grell-Devenyi 集合方案
                                     = 99, 以前的 Kain-Fritsch 方案

shcu_physics (max_dom)              独立的浅对流选项（不与深对流绑定）
                                     = 0, 无独立的浅对流
                                     = 1, Grell 3D 集合方案（需与 cu_physics=93 或 5 一起使用）
                                          （占位符：此开关尚未实现——请使用 ishallow）
                                     = 2, Park 和 Bretherton 提出的 CAM5（CESM 1_0_1）浅对流方案
                                     = 3, GRIMS 浅对流方案（由 YSU 研究组提供）
                                     = 4, NSAS 浅对流方案。必须与 KIAPS SAS 积云方案（cu_physics = 14）一起使用
                                     = 5, Deng 积云方案（包括浅对流和深对流），
                                        由 PSU 和   WRF-Solar 提供。
                                        但该方案的深对流部分不够活跃，不建议单独用于深对流情况。
                                        在 3-9 km 的网格尺度下可能表现良好。
                                        此方案仅适用于 MYJ PBL 方案，且不应与 icloud=3 组合使用。
                                        该方案也可与 MYNN PBL 方案配合使用，但需关闭 EDMF（bl_mynn_edmf=0）。


ishallow                            = 0,        !  = 1 启用浅对流，用于Grell 3D集成方案
                                                  （cu_physics = 3 或 5）
clos_choice                         = 0,        ! 闭合选择（仅作为占位符）
cu_diag (max_dom)                   = 0,        ! 为cu物理（cu_phys = 3, 5, 10 和 93 仅适用）
                                                  提供附加的时间平均值
kf_edrates (max_dom)                = 0,        ! 为基于KF的积云方案添加卷吸/排气率和对流时间尺度
                                                  输出变量（仅适用于cu_phys = 1, 11 和 99）
                                     = 0,       ! 无输出；= 1, 添加输出
convtrans_avglen_m                  = 30,       ! 用于对流传输变量的平均时间（仅适用于cu_phys=3,5 和93）（单位：分钟）
cu_rad_feedback (max_dom)           = .false.   ! 计算子格云效应对辐射的影响
                                                  目前仅适用于GF, G3, GD 和KF方案
                                                  还需要设置cu_diag = 1以启用GF, G3, KF-CuP 和 GD方案
bmj_rad_feedback (max_dom)          = .false.   ! BMJ积云降水派生的子格云效应对辐射的影响
cudt (max_dom)                      = 0,        ! 每次积云物理调用之间的时间间隔（单位：分钟）
kfeta_trigger                       KF触发选项（仅适用于cu_physics=1）：
                                     = 1, 默认选项
                                     = 2, 基于湿度输送的触发（Ma 和 Tan [2009]）
                                     = 3, RH依赖的附加扰动选项1（JMA）
cugd_avedx                          ; 用于扩展沉降的网格点数。
                                     = 1, 默认，用于大网格距离
                                     = 3, 用于小网格距离（DX < 5 km）
nsas_dx_factor                      = 0,        ! 默认选项
                                     = 1,   NSAS网格距离依赖选项
 对于KF-CuP方案：建议与cu_rad_feedback一起使用
shallowcu_forced_ra(max_dom)        强迫浅对流的辐射影响，使用预设的最大云量
                                     = .false., 关闭选项，默认
                                     = .true., 强迫浅对流的辐射影响，云量为0.36
numbins(max_dom)                    潜热和混合比扰动的扰动次数，
                                     应为奇数（推荐值21）
thBinSize(max_dom)                  潜热扰动增量的桶大小（0.01 K）
rBinSize(max_dom)                   混合比扰动增量的桶大小（1.0e-4 kg/kg）
minDeepFreq(max_dom)                在允许深对流之前的最小频率（0.333）
minShallowFreq(max_dom)             在允许浅对流之前的最小频率（1.0e-2）
shcu_aerosols_opt(max_dom)          浅对流中的气溶胶选项：0=无，2=预测（与WRF-Chem一起运行）

对于MSKF方案：
aercu_opt                           控制MSKF和Morrison微物理学中气溶胶相互作用的选项（mp_physics=40）
                                     = 0: 无气溶胶相互作用
                                     = 1: 仅与MSKF进行气溶胶相互作用
                                     = 2: 与MSKF和Morrison均进行气溶胶相互作用
                                     ! aercu_opt = 1和2都需要“CESM_RCP4.5_Aerosol_Data.dat”静态运行时数据文件。
                                       可以从以下地址下载：
                                            http://www2.mmm.ucar.edu/wrf/src/wrf_files/CESM_RCP4.5_Aerosol_Data.tar.gz
                                       解压后将其中一个文件复制到运行目录中的CESM_RCP4.5_Aerosol_Data.dat
aercu_fct                           与气溶胶量相乘的因子
                                     = 1.       ! 默认值
no_src_types_cu                     = 1         ! 全球气溶胶数据中气溶胶种类的数量：对于CESM输入，默认为10
                                                  自动设置
alevsiz_cu                          = 1         ! 全球气溶胶数据中的层数：对于CESM输入默认为30
                                                  自动设置
isfflx                              = 1,	      ! 来自地面的热和湿度通量
                                                  （仅适用于sf_sfclay_physics = 1,5,7,11）
                                                  1 = 从地面获取通量
                                                  0 = 无地面通量
                                                      当bl_pbl_physics=0时，使用tke_drag_coefficient
                                                      和tke_heat_flux进行垂直扩散
                                                  2 = 使用sf_sfclay_physics中的拖曳和tke_heat_flux中的热通量，当bl_pbl_physics=0时
ideal_xland                         = 1,        ! 设置XLAND（1=陆地，2=水域）用于理想案例，无需输入土地利用
                                                      wrf.exe物理初始化的运行时开关（默认值为1）
ifsnow                               = 1,	      ! 雪覆盖效应
                                                  （仅适用于sf_surface_physics = 1）
                                                  1 = 启用雪覆盖效应
                                                  0 = 不启用雪覆盖效应
icloud                              = 1,	      ! 云对辐射的影响
                                                  （仅适用于ra_sw_physics = 1,4 和ra_lw_physics = 1,4）
                                                  从3.6版本起，这也控制云量选项
                                                  1 = 启用云效应，并使用云量选项1
                                                      （Xu-Randall方法）
                                                  0 = 不启用云效应
                                                  2 = 启用云效应，并使用云量选项2（基于阈值的0/1方法）
                                                  3 = 启用云效应，并使用云量选项3（基于Sundqvist等人，1989）
insert_init_cloud                   = .false.,  ! 默认
                                     = .true.,  ! 用于冷启动（零云场）时，使用icloud=3方案生成初始云水和云冰场，以及确保初始水蒸气场与饱和匹配，保留云量
swrad_scat                          = 1.        ! 散射调节参数（默认值为1，单位为1.e-5 m2/kg）
                                                  （仅适用于ra_sw_physics = 1选项）
surface_input_source                = 3,	      ! 土地使用和土壤分类数据来源：
                                                  1 = WPS/geogrid，但重新计算了主导类别
                                                  2 = 来自其他模型的GRIB数据（仅在
                                                      met_em文件中来自WPS时有效）
                                                  3 = 使用来自WPS/geogrid的主导土地和土壤类别（自3.8版本以来为默认）
num_soil_layers                     = 5,	      ! 地面模型中的土壤层数
                                                  = 5：热扩散方案
                                                  = 4：Noah地面模型
                                                  = 6或9：RUC地面模型
                                                  = 10：CLM4地面模型
                                                  = 2：Pleim-Xu地面模型
                                                  = 3：SSiB地面模型
num_land_cat                        = 21,       ! 输入数据中的土地类别数
                                                  24 - 对于USGS（默认）；20对于MODIS
                                                  28 - 对于USGS，如果包括湖泊类别
                                                  21 - 对于MODIS，如果包括湖泊类别（自3.8版本以来默认）
						  40 - 对于NCLD
use_wudapt_lcz                      = 0,        ! 是否使用WUDAPT LCZ城市土地利用类别，以及标准
						  城市类别（31-33）
                                                  = 0：使用传统的33土地利用类别（城市地区为31-33）
                                                  = 1：使用WUDAPT LCZ城市土地利用类别
							* 注意：如果输入文件中的城市类别数与namelist选项不一致，
							  将会出现错误消息。
							  创建LCZ数据的方法可以参考： http://www.wudapt.org/

slucm_distributed_drag              = .false.   ! 是否使用空间变化的二维城市零平面位移、粗糙度长度（动量）、前面面积指数
                                                ! 目前不支持LCZ，仅支持单层城市物理（urban_physics=1）
                                                ! 需要在wrfinput文件中提供上述三个输入变量
distributed_ahe_opt                 = 0,        ! 处理人类活动表面热通量的选项（需要在wrfinput文件中提供额外输入）
                                                  = 0: 不从输入数据中获取人类活动表面热通量
                                                  = 1: 将其添加到第一级温度趋向
                                                  = 2: 将其添加到地表显热通量
num_soil_cat                        = 16,       ! 输入数据中的土壤类别数

pxlsm_smois_init(max_dom)           = 1         ! PXLSM土壤水分初始化选项 
                                                   0 - 从分析数据中获取，1 - 从土壤水分可用性或SLMO在LANDUSE.TBL中获取
pxlsm_modis_veg                     = 1,        ! PX LSM LAI和VEGFRA 
                                                   0 - 使用旧的PX方法，从PX土地利用查找表中获取
                                                   1 - 使用wrflowinp_d0*文件中的VEGFRA和LAI
                                                   注意：输出中使用的值称为VEGF_PX和LAI_PX。

maxiens                             = 1,        ! Grell-Devenyi方法仅适用
maxens                              = 3,        ! G-D方法仅适用
maxens2                             = 3,        ! G-D方法仅适用
maxens3                             = 16        ! G-D方法仅适用
ensdim                              = 144       ! G-D方法仅适用
                                                  这些是推荐的数字。如果要使用其他数字，请查阅代码，确保自己了解使用方法。
seaice_threshold                    = 100.      ! 若 tsk < seaice_threshold，且为水点：
                                                  - 在 5 层板层方案下，将其设置为陆地点和永久冰；
                                                  - 在 Noah 方案下，将其设置为陆地点、永久冰，并从 2m 高度至地表设定温度，
                                                    以及设置土壤含水量（smois）和土壤液态水（sh2o）。
                                                  该默认值在 v3.5.1 版本中已从 271 K 更改为 100 K，以避免与分数海冰输入混淆。
                                                  适用于陆面模式选项 1、2、3、4 和 8。

 sst_update                          = 0        ! 是否使用时间变化的海表温度 (SST)（0=否，1=是）。
                                                  若选择 1（是），则 real 程序会将 SST、XICE、ALBEDO 和 VEGFRA 
                                                  放入 wrflowinp_d01 文件，并且 WRF 会按照与边界文件相同的时间间隔
                                                  从该文件中更新这些变量。
                                                  还需在 &time_control 中设置以下参数：
                                                  - auxinput4_interval
                                                  - auxinput4_end_h
                                                  - auxinput4_inname = "wrflowinp_d<domain>"
                                                  - io_form_auxinput4

 usemonalb                           = .false.  ! 是否使用月平均反照率地图，而非表格值（建议在 sst_update=1 时使用）。

 rdmaxalb                            = .true.   ! 是否使用 geogrid 提供的雪反照率；
                                                  若为 false，则使用表格中的值。

 rdlai2d                             = .false.  ! 是否从输入文件中读取 LAI（叶面积指数）；
                                                  若为 false，则使用表格值。
                                                  如果 sst_update=1，LAI 也会包含在 wrflowinp 文件中。

 dust_emis                           = 0        ! 是否启用地表扬尘排放方案（0=否，1=是），
                                                  以向 mp_physics=28 方案提供 QNIFA（冰核友好气溶胶变量）。

erosion_dim                         = 3         ! 配合dust_emis=1时，值只能设为3（侵蚀性信息）
bucket_mm                           = -1.       ! 水积累的水桶重置值（单位：mm，-1=不激活）
bucket_J                            = -1.       ! 能量积累的水桶重置值（单位：J，-1=不激活）
tmn_update                          = 0         ! 更新深层土壤温度（1=是，0=否）
lagday                              = 150       ! 计算tmn时使用的天数，基于皮肤温度
sst_skin                            = 0         ! 计算皮肤海面温度
slope_rad (max_dom)                 = 0         ! 太阳辐射的坡度效应（1=开启，0=关闭）
topo_shading (max_dom)              = 0         ! 邻近点阴影效应（1=开启，0=关闭）
shadlen                             = 25000.    ! 最大阴影长度（单位：米）当topo_shading=1时
sf_ocean_physics                    = 0         ! 激活海洋模型（0=否，1=1D混合层；2=3D PWP，无海底地形）
oml_hml0                            = 50        ! OML模型可以在任何地方初始化一个常数深度（米）
                                                  < 0，OML使用实时海洋混合层深度初始化
                                                  = 0，OML使用气候学海洋混合层深度初始化
oml_gamma                           = 0.14      ! OML深层水温递减率（K/m）
oml_relaxation_time                 = 0.        ! 海洋混合层模型的松弛时间（秒），返回原始值
                                                  （例如值为259200秒（三天））
omdt                                = 1.        ! 3D PWP时间步长（分钟）。它可以设置与WRF时间步长相同，但omdt不应小于1分钟。
ocean_levels                        = 30        ! 3D PWP中的垂直层数。注意，每层的深度在wrfinput_d01中的OM_DEPTH中指定
traj_opt                            = 0         ! 正向轨迹计算（Lee和Chen 2013）
num_traj                            = 1000      ! 要释放的轨迹数量
isftcflx                            = 0         ! 热带风暴应用的替代Ck、Cd公式 
                                                  sf_sfclay=1和11
                                                  0=默认
                                                  1=Donelan Cd + 常数z0q 
                                                  2=Donelan Cd + Garratt
                                                  sf_sfclay=5
                                                  （默认）=0：z0、zt和zq来自COARE3.0 （Fairall等，2003）
                                                        =1：z0来自Davis等（2008），zt和zq来自COARE3.0
                                                        =2：z0来自Davis等（2008），zt和zq来自Garratt（1992）
fractional_seaice                   = 0         ! 将海冰视为分数场（1）还是冰/非冰标志（0）
                                                  适用于sf_sfclay_physics=1,2,3,4,5,7和91 
                                                  如果fractional_seaice=1，还需要设置seaice_threshold=0。
seaice_albedo_opt                   = 0         ! 设置海冰上反照率的选项
                                                ! 0 = 海冰反照率是来自namelist选项seaice_albedo_default的常数值
                                                ! 1 = 根据Mills（2011）为北极海洋计算的Tair、Tskin、雪的函数
                                                ! 2 = 从输入变量ALBSI中读取海冰反照率
seaice_albedo_default               =  0.65     ! seaice_albedo_opt=0时海冰反照率的默认值
seaice_snowdepth_opt                = 0         ! 处理海冰雪深度的方法
                                                ! 0 = 海冰上的雪深度由seaice_snowdepth_min和seaice_snowdepth_max限定
                                                ! 1 = 从输入数组SNOWSI中读取海冰上的雪深度（由seaice_snowdepth_min和seaice_snowdepth_max限定）
seaice_snowdepth_max                = 1.E10     ! 海冰上允许的最大雪积累深度（米）
seaice_snowdepth_min                = 0.001     ! 海冰上允许的最小雪积累深度（米）
seaice_thickness_opt                = 0         ! 处理海冰厚度的选项
                                                ! 0 = 海冰厚度是来自namelist变量seaice_thickness_default的常数值
                                                ! 1 = 从输入变量ICEDEPTH中读取海冰厚度
seaice_thickness_default            = 3.0       ! seaice_thickness_opt=0时海冰厚度的默认值
tice2tsk_if2cold                    = .false.   ! 如果海冰温度过低，则将Tice设置为Tsk，以避免不切实际的低海冰温度
iz0tlnd                             = 0         ! sfclay的热粗糙度长度（0 = 旧方法，1 = 植被相关的Chen-Zhang Czil，
                                                      2 = Zilitinkevitch（czil=0.1））
                                                      对于mynn表面（0=Zilitinkevitch（默认），1=Chen-Zhang，2=mod Yang，3=常数zt）
mp_tend_lim                         = 10.,      ! 限制来自雷达数据同化的潜热影响的温度趋向
prec_acc_dt (max_dom)               = 0.,       ! 降水桶中的分钟数 - 将添加三个新的二维输出字段：prec_acc_c、prec_acc_nc和snow_acc_nc
topo_wind (max_dom)                 = 0,        ! 1 = 改善地形对地面风的影响。
ua_phys                             = .false.   ! 激活UA Noah变化的选项：一种不同的雪覆盖物理方法
                                                  Noah模型，特别是改善与植被冠层相关的雪处理。还使用VEGPARM.TBL中添加的新列
do_radar_ref                        = 0,        ! 1 = 允许使用特定于MP方案的参数计算雷达反射率。当前仅适用于mp_physics = 2,4,6,7,8,10,14,16,24,26,28
                                                  注意，当nwp_diagnostics=1时，反射率始终会被计算出来
hailcast_opt (max_dom)              = 0, 	      ! 1 = 1-D冰雹增长模型，预测第1-5阶排序冰雹直径、冰雹平均直径和标准差（Adams-Selin和Ziegler，MWR 2016年12月）。
haildt (max_dom)                    = 0.,       ! WRF-HAILCAST调用之间的时间间隔（秒）

湖泊模块的Namelist变量：
sf_lake_physics(max_dom)            = 1,        ! 湖泊模型开关
lakedepth_default(max_dom)          = 50,       ! 默认湖泊深度（如果输入数据中没有湖泊深度信息，则默认湖泊深度为50米）
lake_min_elev(max_dom)              = 5,        ! 湖泊的最低海拔。如果没有湖泊类别信息，可以用此值来判断水点是否为湖泊。如果土地利用类型包括“湖泊”（例如Modis_lake和USGS_LAKE），则此变量无效。
use_lakedepth(max_dom)             = 1,         ! 是否使用湖泊深度数据。湖泊深度数据可通过3.6版的geogrid程序获得。如果未处理湖泊深度数据，但此开关设置为1，程序将停止并提示返回geogrid程序。
                                                = 0, 不使用湖泊深度数据。
shalwater_z0                        = 0,        ! 浅水域粗糙度方案开关（1为开启，0为关闭）。仅与sf_sfclay_physics = 1兼容。
                                                  来自 GEBCO Compilation Group 的海底地形数据集。
                                                  在演示和出版物中请注明以下信息：
                                                  GEBCO Compilation Group (2021) GEBCO 2021 Grid 
                                                  (doi:10.5285/c6612cbe-50b3-0cff-e053-6c86abc09f8f)。

 shalwater_depth                     = 0.0,     ! 设定浅水粗糙度方案的固定深度 [m]（必须为正值），
                                                  仅在无海底地形数据时使用。
                                                  该方案适用于深度在 10.0 到 100.0 m 之间的情况，
                                                  任何超出此范围的深度值都将四舍五入到最近的限制值。

 lightning_option (max_dom)                      闪电参数化选项，允许在无化学过程的情况下预测闪电频率。
                                     = 0        ! 关闭闪电参数化
                                     = 1        ! PR92 方法，基于最大上升气流 w，重新分配闪电至 dBZ > 20 的区域
                                                  （适用于解析对流的模拟，必须同时使用：
                                                  do_radar_ref = 1，且 mp_physics = 2,4,6,7,8,10,14,16）。
                                     = 2        ! PR92 方法，基于 20 dBZ 顶部高度，重新分配闪电至 dBZ > 20 的区域
                                                  （适用于解析对流的模拟，必须同时使用：
                                                  do_radar_ref = 1，且 mp_physics = 2,4,6,7,8,10,14,16）。
                                     = 3        ! 预测闪电活动的潜在可能性
                                                  （基于 Yair et al., 2010, J. Geophys. Res., 115, D04205, 
                                                  doi:10.1029/2008JD010868）。
                                     = 11       ! PR92 方法，基于对流参数化的中性浮力层高度（适用于 10 < dx < 50 km 的
                                                  网格尺度，必须同时使用 cu_physics = 5 或 93）。

 lightning_dt  (max_dom)             = 0.       ! 闪电参数化调用的时间间隔（秒）。默认使用模型时间步长。

 lightning_start_seconds (max_dom)   = 0.       ! 启动闪电参数化的时间（秒）。建议至少预留 10 分钟的自旋时间（spin-up）。

 flashrate_factor  (max_dom)         = 1.0      ! 调整预测闪电次数的因子。
                                                  对于 lightning_option = 11，且 dx 在 10 到 50 km 之间时，
                                                  建议使用 1.0。对于所有其他选项，应单独手动调整每个嵌套网格的参数。

cellcount_method (max_dom)                      ! 计算风暴单元的方法。由CRM选项（lightning_options=1,2）使用。
                                                = 0,       ! 模型自动确定使用的方法
                                                = 1,       ! 面状范围内，适用于大范围领域
                                                = 2,       ! 域范围内，适用于单风暴领域
cldtop_adjustment (max_dom)         = 0.        ! 从LNB（单位：公里）进行的调整。由lightning_option=11使用。默认值为0，但建议调整为2公里。
iccg_method (max_dom)                           ! IC:CG分区方法（IC:云内；CG:云到地）
                                                = 0        ! 默认方法，当前所有选项都使用iccg_method=2
                                                = 1        ! 恒定全域设置，使用namelist选项iccg_prescribed（num|den）#，默认值为0./1.（全部为CG）。
                                                = 2        ! 基于1995-1999 NLDN/OTD气候数据的粗略规定（参见Boccippio等，2001年）
                                                = 3        ! 基于Price和Rind（1993年）参数化的冷云深度
                                                = 4        ! 通过wrfinput中的数组iccg_in_(num|den)提供的网格化输入，用于每月映射比率。具有0/0值的点使用由iccg_prescribed_(num|den)定义的比率。
iccg_prescribed_num (max_dom)       = 0.        ! 用户指定的IC:CG比例的分子
iccg_prescribed_den (max_dom)       = 1.        ! 用户指定的IC:CG比例的分母
ltng_temp_upper                     = -45.      ! LNOx垂直分布的上峰温度（摄氏度），适用于IC闪电（由&chem lnox_opt=2使用）。
ltng_temp_lower                     = -15.      ! LNOx垂直分布的下峰温度（摄氏度），适用于IC和CG闪电（由&chem lnox_opt=2使用）。

用于MAD-WRF的选项 - 请参见doc/README.madwrf以了解使用信息：
madwrf_opt = 0         ! MAD-WRF模型：0）关闭，1）平流/扩散被动水汽（qcloud，qice，qsnow），2）将活跃微物理水汽调整为被动水汽
madwrf_dt_relax = 60.0 ! 水汽调整松弛时间[s]
madwrf_dt_nudge = 60.0 ! 水汽调整的时间间隔[分钟]
madwrf_cldinit = 0     ! 增强云初始化：0：关闭，1：开启

风电场拖曳参数化选项：

windfarm_opt (max_dom)             = 0          ! 1 = 模拟风力涡轮机对大气演化的影响，激活Fitch等人（2012）提出的风电场参数化
                                                2 = 启用Ma等人（2022）提出的新的风电场方案（mav方案）。与选项1类似，但也考虑了子网格尺度的风力涡轮机尾迹效应
windfarm_ij                        = 0          ! 是否使用经纬度坐标或i-j坐标作为风电涡轮机的位置
                                                0 = 风力涡轮机的位置通过经纬度定义
                                                1 = 风力涡轮机的位置通过网格点定义
                                                2 = 仅在windfarm_opt=2时有效。风力涡轮机的位置通过经纬度定义，文件名为'windturbines-ll.txt'
windfarm_wake_model                = 2          ! 子网格尺度风力涡轮机尾迹模型，仅在windfarm_opt=2时有效，默认值为2
                                                1 = Jensen模型
                                                2 = XA模型
                                                3 = GM模型（windfarm_method未使用）
                                                4 = Jensen和XA的集合
                                                5 = Jensen、XA和GM的集合
windfarm_overlap_method            = 4          ! Jensen和XA风力涡轮机尾迹模型的尾迹叠加方法，仅在windfarm_opt=2时有效，默认值为4
                                                1 = 线性叠加
                                                2 = 平方叠加
                                                3 = 修改平方叠加
                                                4 = 以hub高度风速叠加（Ma等人，2022年）
windfarm_deg                       = 0.         ! 风电场布局的旋转角度。仅在'windfarm_opt=2'和'windfarm_ij=1'时有效
windfarm_tke_factor                = 0.25       ! 应用于TKE系数的修正因子（默认值为0.25，参见Archer等人，2020年）

随机参数化方案:
; 随机扰动场 (rand_perturb=1)
rand_perturb (max_dom)               = 1        ! 生成用户定义使用的随机扰动数组，1: 开启
gridpt_stddev_rand_pert (max_dom)    = 0.03     ! 每个网格点的随机扰动场的标准差。决定随机扰动的幅度
lengthscale_rand_pert (max_dom)      = 500000.0 ! 扰动长度尺度（单位：米）。
timescale_rand_pert (max_dom)        = 21600.0  ! 随机场的时间去相关（单位：秒）。
stddev_cutoff_rand_pert (max_dom)    = 3.0      ! 扰动模式的标准差超过此阈值时的裁剪。
rand_pert_vertstruc                  = 0        ! 随机扰动场的垂直结构：0=常数；1=随机相位并倾斜
iseed_rand_pert                       ! 随机数流的种子，用于rand_perturb。将与种子nens（代表集合成员编号）和初始起始时间结合，以确保来自不同初始时间和不同集合成员的预报使用不同的随机数流。

; 随机扰动物理趋势（SPPT）（sppt=1）
sppt (max_dom)                       = 0        ! 随机扰动物理趋势（SPPT），0: 关闭，1: 开启
gridpt_stddev_sppt (max_dom)         = 0.5      ! 每个网格点的随机扰动场的标准差。决定随机扰动的幅度
lengthscale_sppt (max_dom)           = 150000.0 ! 扰动长度尺度（单位：米）。
timescale_sppt (max_dom)             = 21600.0  ! 随机场的时间去相关（单位：秒）。
sppt_vertstruc                       = 0.       ! sppt的垂直结构，0：常数，1：随机相位。
stddev_cutoff_sppt (max_dom)         = 2.0      ! 扰动模式的标准差超过此阈值时的裁剪。
iseed_sppt                            ! 随机数流的种子，用于sppt。将与种子nens（代表集合成员编号）和初始起始时间结合，以确保来自不同初始时间和不同集合成员的预报使用不同的随机数流。

; 随机动能反向散射方案（SKEBS）（skebs=1）：
skebs (max_dom)                    = 0         ! 随机动能反向散射方案，0：关闭，1：开启
tot_backscat_psi (max_dom)         = 1.0E-05   ! 控制旋转风扰动的幅度
tot_backscat_t (max_dom)           = 1.0E-06   ! 控制潜在温度扰动的幅度
ztau_psi                           = 10800.0   ! psi扰动的噪声去相关时间
ztau_t                             = 10800.0   ! theta扰动的噪声去相关时间
rexponent_psi                      = -1.83     ! psi强迫的谱斜率
rexponent_t                        = -1.83     ! theta强迫的谱斜率
zsigma2_eps                        = 0.0833    ! psi扰动的噪声方差
zsigma2_eta                        = 0.0833    ! theta扰动的噪声方差
kminforc                           = 1         ! psi扰动的最小强迫波数（经度方向）
lminforc                           = 1         ! psi扰动的最小强迫波数（纬度方向）
kminforct                          = 1         ! theta扰动的最小强迫波数（经度方向）
lminforct                          = 1         ! theta扰动的最小强迫波数（纬度方向）
kmaxforc                           = 1000000   ! psi扰动的最大强迫波数（经度方向）
lmaxforc                           = 1000000   ! psi扰动的最大强迫波数（纬度方向）
kmaxforct                          = 1000000   ! theta扰动的最大强迫波数（经度方向）
lmaxforct                          = 1000000   ! theta扰动的最大强迫波数（纬度方向）
skebs_vertstruc                    = 0         ! 随机扰动场的垂直结构：0=常数；1=随机相位并倾斜

随机扰动参数方案（SPP）（spp=1）
sppt (max_dom)                      = 0        ! 随机扰动参数（SPP）方案用于
                                                ; GF对流方案、MYNN边界层方案和RUC陆面模型，0：关闭，1：开启
spp_conv (max_dom)                  = 0        ! 仅扰动GF对流方案的参数
gridpt_stddev_spp_conv (max_dom)    = 0.3      ! 每个网格点的随机扰动场的标准差。决定随机扰动的幅度
lengthscale_spp_conv (max_dom)      = 150000.0 ! 扰动长度尺度（单位：米）。
timescale_spp_conv (max_dom)        = 21600.0  ! 随机场的时间去相关（单位：秒）。
stddev_cutoff_spp_conv (max_dom)    = 3.0      ! 扰动模式的标准差超过此阈值时的裁剪。
iseed_spp_conv                       ! 随机数流的种子，用于spp_conv。将与种子nens（代表集合成员编号）和初始起始时间结合，以确保来自不同初始时间和不同集合成员的预报使用不同的随机数流。
spp_pbl (max_dom)                   = 0        ! 仅扰动MYNN PBL方案的参数
gridpt_stddev_spp_pbl (max_dom)     = 0.15     ! 每个网格点的随机扰动场的标准差。决定随机扰动的幅度
lengthscale_spp_pbl (max_dom)       = 70000.0  ! 扰动长度尺度（单位：米）。
timescale_spp_pbl (max_dom)         = 21600.0  ! 随机场的时间去相关（单位：秒）。
stddev_cutoff_spp_pbl (max_dom)     = 2.0      ! 扰动模式的标准差超过此阈值时的裁剪。
iseed_spp_pbl                        ! 随机数流的种子，用于spp_pbl。将与种子nens（代表集合成员编号）和初始起始时间结合，以确保来自不同初始时间和不同集合成员的预报使用不同的随机数流。
spp_lsm (max_dom)                   = 0        ! 扰动RUC陆面模型（LSM）的参数
gridpt_stddev_spp_lsm (max_dom)     = 0.3      ! 每个网格点的随机扰动场的标准差。决定随机扰动的幅度
lengthscale_spp_lsm (max_dom)       = 50000.0  ! 扰动长度尺度（单位：米）。
timescale_spp_lsm (max_dom)         = 86400.0  ! 随机场的时间去相关（单位：秒）。
stddev_cutoff_spp_lsm (max_dom)     = 3.0      ! 扰动模式的标准差超过此阈值时的裁剪。
iseed_spp_lsm                        ! 随机数流的种子，用于spp_lsm。将与种子nens（代表集合成员编号）和初始起始时间结合，以确保来自不同初始时间和不同集合成员的预报使用不同的随机数流。

WRF太阳能EPS的多重扰动（multi_perturb=1）
multi_perturb         = 0        ! 1) WRF-Solar EPS: 开启针对太阳能应用的随机扰动
spdt                  = -1.0     ! 更新随机扰动的频率（分钟）。负值表示每个时间步更新
pert_farms            = .false.  ! 是否对FARMS参数化方案进行扰动
pert_farms_albedo     = 0.0      ! 1.0) 扰动地表反照率, 0.0) 无扰动。下方其他选项类似
pert_farms_aod        = 0.0
pert_farms_angexp     = 0.0
pert_farms_aerasy     = 0.0
pert_farms_qv         = 0.0
pert_farms_qc         = 0.0
pert_farms_qs         = 0.0
pert_deng             = .false.  ! 是否对Deng的浅对流（shcu）参数化方案进行扰动
pert_deng_qv          = 0.0
pert_deng_qc          = 0.0
pert_deng_t           = 0.0
pert_deng_w           = 0.0
pert_mynn             = .false.  ! 是否对MYNN PBL（边界层）参数化方案进行扰动
pert_mynn_qv          = 0.0
pert_mynn_qc          = 0.0
pert_mynn_t           = 0.0
pert_mynn_qke         = 0.0
pert_noah             = .false.  ! 是否对Noah LSM（土壤湿度）进行扰动
pert_noah_qv          = 0.0
pert_noah_t           = 0.0
pert_noah_smois       = 0.0
pert_noah_tslb        = 0.0
pert_thom             = .false.  ! 是否对Thompson微物理方案进行扰动
pert_thom_qv          = 0.0
pert_thom_qc          = 0.0
pert_thom_qi          = 0.0
pert_thom_qs          = 0.0
pert_thom_ni          = 0.0
pert_cld3             = .false.  ! 是否对icloud = 3生成的云进行扰动
pert_cld3_qv          = 0.0
pert_cld3_t           = 0.0

num_pert_3d           = 15       ! STOCHPERT.TBL中条目数+1（无需修改）

; 对边界条件进行随机扰动 (perturb_bdy)
perturb_bdy                        = 0          ! 0: 不进行边界扰动
                                      1         使用SKEBS模式进行边界扰动
                                      2         使用其他用户提供的模式进行边界扰动

; 对WRF-CHEM中的边界趋势进行随机扰动 (perturb_chem_bdy)
perturb_chem_bdy                                 ! 对化学变量的侧边界进行扰动：
                                                  0 = 关闭; 1 = 使用RAND_PERTURB模式进行扰动

; 适用于所有随机方案的公共参数
nens                                = 1         ! 随机数流的种子。对于集合预报，每个成员都需要不同的种子。
                                                  该种子与初始开始时间有关，以确保不同初始时间的预测具有不同的随机数流。
                                                  修改此种子将更改所有已启用的随机参数化方案的随机数流。

与Noah MP陆面模型（sf_Surface_physics=4）一起使用的选项：

&noah_mp
dveg = 4,        ! Noah-MP 动态植被选项：
                      1 = 关闭（LAI 来自表格；FVEG = shdfac）
                      2 = 开启（LAI 预测；FVEG 计算）
                      3 = 关闭（LAI 来自表格；FVEG 计算）
                      4 = 关闭（LAI 来自表格；FVEG = 最大植被覆盖率）
                      5 = 开启（LAI 预测；FVEG = 最大植被覆盖率）
                      6 = 开启（使用输入的 FVEG = SHDFAC）
                      7 = 关闭（使用输入的 LAI；使用输入的 FVEG = SHDFAC）
                      8 = 关闭（使用输入的 LAI；计算 FVEG）
                      9 = 关闭（使用输入的 LAI；使用最大植被覆盖率）

opt_crs = 1,     ! Noah-MP 气孔阻力选项：
                      1 = Ball-Berry 模型
                      2 = Jarvis 模型

opt_sfc = 1      ! Noah-MP 表面层阻力系数计算方法：
                      1 = Monin-Obukhov
                      2 = 原始 Noah（Chen97）

opt_btr = 1,     ! Noah-MP 土壤水分因子用于气孔阻力：
                      1 = Noah
                      2 = CLM
                      3 = SSiB

opt_run = 3,     ! Noah-MP 地表径流和地下水选项：
                      1 = 具有地下水的 TOPMODEL
                      2 = 具有平衡水位的 TOPMODEL
                      3 = 原始地表和地下径流（自由排水）- 默认
                      4 = BATS 地表和地下径流（自由排水）
                      5 = Miguez-Macho & Fan 地下水方案（Miguez-Macho 等，2007 JGR；Fan 等，2007 JGR）
                            需要使用 GEOGRID.TBL.ARW.noahmp 运行 geogrid，谨慎使用
                      6 = 变渗透容量模型（VIC）地表径流方案（Wood 等，1992，JGR）
                      7 = Xiananjiang 入渗和地表径流方案（Jayawardena 和 Zhou，2000）
                      8 = 动态 VIC 地表径流方案（Liang 和 Xie，2001）

opt_infdv = 0,   ! Noah-MP 在动态 VIC 径流方案中的入渗选项（仅适用于 opt_run=8）
                      1 = Philip 方案
                      2 = Green-Ampt 方案
                      3 = Smith-Parlange 方案

opt_frz = 1,     ! Noah-MP 过冷液态水选项：
                      1 = 无迭代
                      2 = Koren 迭代

opt_inf = 1,     ! Noah-MP 土壤渗透性选项：
                      1 = 线性效应，更具渗透性
                      2 = 非线性效应，渗透性较低

opt_rad = 3,     ! Noah-MP 辐射传输选项：
                      1 = 修改后的双流模型（当植被覆盖率较小时可能存在问题）
                      2 = 双流模型应用于网格单元
                      3 = 双流模型应用于植被覆盖部分

opt_alb = 2,     ! Noah-MP 地表反照率选项：
                      1 = BATS
                      2 = CLASS

opt_snf = 1,     ! Noah-MP 降水雪雨分配选项：
                      1 = Jordan (1991)
                      2 = BATS：当 SFCTMP < TFRZ+2.2 时为雪
                      3 = 当 SFCTMP < TFRZ 时为雪
                      4 = 使用 WRF 降水分配
                      5 = 使用湿球温度（Wang 等，2019 GRL）

opt_tbot = 2,    ! Noah-MP 土壤温度下边界条件：
                      1 = 零热通量
                      2 = 来自输入文件的 8m 处 TBOT 温度

opt_stc = 1,     ! Noah-MP 雪/土壤温度时间方案：
                      1 = 半隐式
                      2 = 全隐式
                      3 = 半隐式（其中 Ts 使用雪覆盖分数）

opt_gla = 1,     ! Noah-MP 冰川处理选项：
                      1 = 包含相变
                      2 = 块状冰（Noah）

opt_rsf = 1,     ! Noah-MP 地表蒸发阻力选项：
                      1 = Sakaguchi 和 Zeng（2009）
                      2 = Sellers（1992）
                      3 = 调整后的 Sellers 以减少湿土壤的表面阻力（RSURF）
                      4 = 对于非雪地使用选项 1；对于积雪，rsurf = rsurf_snow（在 MPTABLE 中设置）

opt_soil = 1,    ! Noah-MP 土壤属性定义选项：
                      需要使用 GEOGRID.TBL.ARW.noahmp 运行 geogrid，谨慎使用
                      1 = 使用输入的主要土壤纹理
                      2 = 使用随深度变化的输入土壤纹理
                      3 = 使用土壤成分（沙子、粘土、有机物）和土壤转移函数（OPT_PEDO）
                      4 = 使用输入土壤属性（BEXP_3D, SMCMAX_3D 等）（在 WRF 中无效）

opt_pedo = 1,    ! Noah-MP 土壤转移函数选项（当 OPT_SOIL = 3 时使用）
                      需要使用 GEOGRID.TBL.ARW.noahmp 运行 geogrid，谨慎使用
                      1 = Saxton 和 Rawls（2006）

opt_crop = 0,    ! 作物模型选项：
                      需要使用 GEOGRID.TBL.ARW.noahmp 运行 geogrid，谨慎使用
                      0 = 无作物模型，将运行默认的动态植被
                      1 = Liu 等（2016）
                      2 = Gecros（基因型-环境相互作用作物生长模拟器），Yin 和 van Laar（2005）

opt_irr                            = 0,         !  灌溉方案选项  
                                                         geogrid 必须使用 GEOGRID.TBL.ARW.noahmp 运行，使用时需谨慎  
	                                             0 = 无灌溉  
                                                     1 = 开启灌溉  
                                                     2 = 根据作物生长季节（播种和收获日期）触发灌溉  
                                                     3 = 根据 LAI（叶面积指数）阈值触发灌溉  
 opt_irrm                           = 0,        !  灌溉方式选项（仅当 opt_irr > 0 时适用）  
                                                         geogrid 必须使用 GEOGRID.TBL.ARW.noahmp 运行，使用时需谨慎  
                                                     0 = 基于 geo_em 分数的方法（开启所有三种灌溉方式）  
                                                     1 = 喷灌  
                                                     2 = 微灌/滴灌  
                                                     3 = 地面漫灌  
 opt_tdrn                           = 0,        !  Noah-MP 瓦块排水选项（目前仅在 opt_run=3 时经过测试并有效）  
                                                         geogrid 必须使用 GEOGRID.TBL.ARW.noahmp 运行，使用时需谨慎  
                                                     0 = 无瓦块排水  
                                                     1 = 简单排水  
                                                     2 = 基于 Hooghoudt 方程的瓦块排水  
 soiltstep                          = 0.0,      !  Noah-MP 土壤过程时间步长（秒），用于求解土壤水分和温度  
                                                     0 = 默认，与 NoahMP 主模型时间步长相同  
                                                     N * dt_noahmp，通常为 15 分钟或 30 分钟  
 noahmp_output                      = 1,        !  Noah-MP 输出级别  
                                                     1 = 标准输出  
                                                     3 = 标准输出，附加水和能量收支项输出  
 noahmp_acc_dt                      = 0.0,      !  Noah-MP 累积输出的桶重置时间间隔（分钟）（仅在 noahmp_output=3 时有效）  
 /  

 &fdda  
 grid_fdda (max_dom)                 = 1        !  网格强制数据同化（FDDA）选项  
                                     = 2        !  频谱强制  
 gfdda_inname                        = "wrffdda_d<domain>" ! 在 real 预处理程序中定义的输入文件名称  
 gfdda_interval_m (max_dom)          = 360      !  分析时间之间的时间间隔（分钟）（必须使用分钟）  
 gfdda_end_h (max_dom)               = 6        !  从预报开始计算，停止强制数据同化的时间（小时）  
 io_form_gfdda                       = 2        !  分析数据的输入/输出格式（2 = netCDF）  
 fgdt (max_dom)                      = 0        !  网格强制数据同化的计算频率（分钟）（0=每步计算）  
 if_no_pbl_nudging_uv (max_dom)      = 0        !  1= 在行星边界层（PBL）不进行 u 和 v 方向风速的强制同化，0= 在 PBL 进行强制同化  
 if_no_pbl_nudging_t (max_dom)       = 0        !  1= 在 PBL 不进行温度强制同化，0= 在 PBL 进行强制同化  
 if_no_pbl_nudging_q (max_dom)       = 0        !  1= 在 PBL 不进行水汽混合比强制同化，0= 在 PBL 进行强制同化  
 if_zfac_uv (max_dom)                = 0        !  0= 在所有层进行 u 和 v 强制同化，1= 仅在 k_zfac_uv 以上的层进行强制同化  
  k_zfac_uv (max_dom)                = 10       !  在该模式层以下停止对 u 和 v 的强制同化  
 if_zfac_t (max_dom)                 = 0        !  0= 在所有层进行温度强制同化，1= 仅在 k_zfac_t 以上的层进行强制同化  
  k_zfac_t (max_dom)                 = 10       !  在该模式层以下停止对温度的强制同化  
 if_zfac_q (max_dom)                 = 0        !  0= 在所有层进行水汽混合比强制同化，1= 仅在 k_zfac_q 以上的层进行强制同化  
  k_zfac_q (max_dom)                 = 10       !  在该模式层以下停止对水汽混合比的强制同化  
 guv (max_dom)                       = 0.0003   !  u 和 v 风速的强制同化系数（秒⁻¹）  
 gt (max_dom)                        = 0.0003   !  温度的强制同化系数（秒⁻¹）  
 gq (max_dom)                        = 0.00001  !  水汽混合比的强制同化系数（秒⁻¹）  
 if_ramping                          = 0        !  0= 强制同化在设定时间结束后立即停止，1= 在结束阶段逐渐降低强制同化强度  
 dtramp_min                          = 0        !  逐步降低强制同化强度的时间（分钟）  
 grid_sfdda (max_dom)                = 0        !  地面强制数据同化开关  
                                                  0: 关闭  
                                                  1: 仅对选定的地面变量进行强制同化  
                                                  2: FASDAS（通量调整地面数据同化系统）  
 sgfdda_inname                       = "wrfsfdda_d<domain>" ! 地面强制数据同化的输入文件名称（来自 obsgrid 程序）  
 sgfdda_end_h (max_dom)              = 6        !  从预报开始计算，停止地面强制同化的时间（小时）  
 sgfdda_interval_m (max_dom)         = 180      !  地面分析时间之间的时间间隔（分钟）（必须使用分钟）  
 io_form_sgfdda                      = 2        !  地面分析数据的输入/输出格式（2 = netCDF）  
 guv_sfc (max_dom)                   = 0.0003   !  地面 u 和 v 风速的强制同化系数（秒⁻¹）  
 gt_sfc (max_dom)                    = 0.0003   !  地面温度的强制同化系数（秒⁻¹）  
 gq_sfc (max_dom)                    = 0.00001  !  地面水汽混合比的强制同化系数（秒⁻¹）  
 rinblw (max_dom)                    = 0.       !  用于确定观测数据对分析的置信度（或权重）的影响半径  
                                                  计算基于网格点到最近观测点的距离  
                                                  若没有附近观测点，使用较低权重的分析数据  
 pxlsm_soil_nudge(max_dom)           = 1        !  PXLSM 土壤强制同化选项（需要 wrfsfdda 文件）  

以下内容用于光谱微调：
fgdtzero (max_dom)                  = 0,       ! 1= 在FDDA调用之间将微调趋势设为零
if_no_pbl_nudging_uv (max_dom)      = 0,       ! 1= 在行星边界层（PBL）中不对风速 (uv) 进行微调，0= 在PBL中进行微调
if_no_pbl_nudging_t  (max_dom)      = 0,       ! 1= 在PBL中不对温度 (t) 进行微调，0= 在PBL中进行微调
if_no_pbl_nudging_ph (max_dom)      = 0,       ! 1= 在PBL中不对位势高度 (ph) 进行微调，0= 在PBL中进行微调
if_no_pbl_nudging_q  (max_dom)      = 0,       ! 1= 在PBL中不对水汽混合比 (q) 进行微调，0= 在PBL中进行微调
if_zfac_uv (max_dom)                = 0,       ! 0= 在所有层中微调风速 (uv)，1= 仅对 k_zfac_uv 以上的层进行微调
k_zfac_uv (max_dom)                 = 0,       ! 0= 在此模型层以下关闭风速 (uv) 微调
dk_zfac_uv (max_dom)                = 1,       ! 在 k_zfac_X 到 dk_zfac_X 之间的深度，微调强度线性增加
if_zfac_t  (max_dom)                = 0,       ! 0= 在所有层中微调温度 (t)，1= 仅对 k_zfac_t 以上的层进行微调
k_zfac_t  (max_dom)                 = 0,       ! 0= 在此模型层以下关闭温度 (t) 微调
dk_zfac_t  (max_dom)                = 1,       ! 在 k_zfac_X 到 dk_zfac_X 之间的深度，微调强度线性增加
if_zfac_ph (max_dom)                = 0,       ! 0= 在所有层中微调位势高度 (ph)，1= 仅对 k_zfac_ph 以上的层进行微调
k_zfac_ph (max_dom)                 = 0,       ! 0= 在此模型层以下关闭位势高度 (ph) 微调
dk_zfac_ph  (max_dom)               = 1,       ! 在 k_zfac_X 到 dk_zfac_X 之间的深度，微调强度线性增加
if_zfac_q  (max_dom)                = 0,       ! 0= 在所有层中微调水汽混合比 (q)，1= 仅对 k_zfac_q 以上的层进行微调
k_zfac_q  (max_dom)                 = 0,       ! 0= 在此模型层以下关闭水汽混合比 (q) 微调
dk_zfac_q  (max_dom)                = 1,       ! 在 k_zfac_X 到 dk_zfac_X 之间的深度，微调强度线性增加
gph (max_dom)                       = 0.0003,  ! 地理高度 (gph)
ktrop                               = 0,       ! 代表对流层顶的层，用于限制水汽 (q) 和温度 (t) 的微调
                                        ; 设置 ktrop = 0 允许微调扩展到大气的顶层
xwavenum (max_dom)                  = 3,       ! X 方向上微调的最高波数
ywavenum (max_dom)                  = 3,       ! Y 方向上微调的最高波数

以下是观测数据微调的参数：
obs_nudge_opt (max_dom)             = 1        ! 观测数据微调 FDDA 开启 (=0 关闭)
                                                ; 还需要在 time_control 名单文件中设置 auxinput11_interval 和 auxinput11_end_h
max_obs                             = 0        ! 在任意时间窗口内，某个区域所使用的最大观测数据数量
fdda_start (max_dom)                = 0        ! 观测数据微调的开始时间（分钟）
fdda_end (max_dom)                  = 0        ! 观测数据微调的结束时间（分钟）
obs_nudge_wind (max_dom)            = 1        ! 是否对风速进行微调（=0 关闭）
obs_coef_wind (max_dom)             = 0,       ! 风速微调系数，单位：s⁻¹
obs_nudge_temp (max_dom)            = 0,       ! 1= 进行温度微调（默认 = 0，关闭）
obs_coef_temp (max_dom)             = 0,       ! 温度微调系数，单位：s⁻¹
obs_nudge_mois (max_dom)            = 1        ! 是否对水汽混合比进行微调（=0 关闭）
obs_coef_mois (max_dom)             = 0,       ! 水汽混合比微调系数，单位：s⁻¹
obs_nudge_pstr (max_dom)            = 0        ! 是否对地面气压进行微调（未使用）
obs_coef_pstr (max_dom)             = 0.       ! 地面气压微调系数，单位：s⁻¹（未使用）
obs_rinxy (max_dom)                 = 0.       ! 水平影响半径（公里）
obs_rinsig                          = 0        ! 垂直影响半径（eta）
obs_twindo (max_dom)                = 0.66667  ! 观测数据用于微调的半周期时间窗口（小时）
obs_npfi                            = 0,       ! 粗网格时间步中进行诊断打印的频率
obs_ionf (max_dom)                  = 2        ! 观测数据输入和误差计算的频率（粗网格时间步）
obs_idynin                          = 0        ! 使用动态初始化，逐渐降低 FDDA 并转向纯预测（=1 开启）
obs_dtramp                          = 0        ! 微调逐渐减少至零的时间（分钟）
obs_prt_freq (max_dom)              = 10,      ! 诊断打印输出的观测数据索引频率
obs_prt_max                         = 1000,    ! 诊断打印允许的最大观测数据条目数
obs_ipf_in4dob                      = .true.   ! 输出观测数据输入诊断（=.false. 关闭）
obs_ipf_errob                       = .true.   ! 输出观测数据误差诊断（=.false. 关闭）
obs_ipf_nudob                       = .true.   ! 输出观测数据微调诊断（=.false. 关闭）
obs_ipf_init                        = .true.   ! 启用观测数据初始化警告信息

obs_no_pbl_nudge_uv (max_dom)       = 0        ! 1= 在PBL中不对风速进行微调
obs_no_pbl_nudge_t (max_dom)        = 0        ! 1= 在PBL中不对温度进行微调
obs_no_pbl_nudge_q (max_dom)        = 0        ! 1= 在PBL中不对水汽混合比进行微调
obs_sfc_scheme_horiz                = 0        ! 地表观测的水平扩展方案：
                                                ; 0=WRF 方案，1= 原始 MM5 方案
obs_sfc_scheme_vert                 = 0        ! 地表观测的垂直扩展方案：
                                                ; 0= 规则 VIF 方案，1= 原始简单方案
obs_max_sndng_gap                   = 20       ! 探空数据之间的最大气压间隔（cb）
obs_scl_neg_qv_innov                = 0        ! 1 = 防止微调到负的水汽混合比QV 
 /

 &scm
 scm_force                           = 1,       ! 单列模式（SCM）强迫开关（=0 关闭）
 scm_force_dx                        = 4000.    ! SCM 强迫的水平网格间距（单位：米）
 num_force_layers                    = 8        ! SCM 输入强迫层数
 scm_lu_index                        = 2        ! SCM 土地利用类型（2 代表旱地、农田和牧场）
 scm_isltyp                          = 4        ! SCM 土壤类型（4 代表壤土）
 scm_vegfra                          = 0.5      ! SCM 植被覆盖率
 scm_canwat                          = 0.0      ! SCM 冠层含水量
 scm_lat                             = 36.605   ! SCM 纬度
 scm_lon                             = -97.485  ! SCM 经度
 scm_th_adv                          = .true.   ! 开启 SCM 温位（theta）平流
 scm_wind_adv                        = .true.   ! 开启 SCM 风场平流
 scm_qv_adv                          = .true.   ! 开启 SCM 水汽（qv）平流
 scm_ql_adv                          = .true.   ! 开启 SCM 云液水（ql）平流
 scm_vert_adv                        = .true.   ! 开启 SCM 垂直平流
 num_force_soil_layers               = 5,       ! SCM 土壤强迫层数
 scm_soilT_force                     = .false.  ! 开启 SCM 土壤温度强迫
 scm_soilq_force                     = .false.  ! 开启 SCM 土壤湿度强迫
 scm_force_th_largescale             = .false.  ! 开启 SCM 大尺度温位（theta）强迫
 scm_force_qv_largescale             = .false.  ! 开启 SCM 大尺度水汽（qv）强迫
 scm_force_ql_largescale             = .false.  ! 开启 SCM 大尺度云水（ql）强迫
 scm_force_wind_largescale           = .false.  ! 开启 SCM 大尺度风场强迫

 &dynamics
 hybrid_opt                          = 2,       ! 默认值；Klemp 三次立方形式的 etac
                                                  0 = 原始 WRF 地形跟随坐标（适用于 V3 及之前的版本）
 etac                                = 0.2      ! 当 znw(k) < etac 时，eta 面为等压面，0.2 是推荐值（Pa/Pa）
 rk_ord                              = 3,       ! 时间积分方案选项：
                                                  2 = 二阶 Runge-Kutta 方法
                                                  3 = 三阶 Runge-Kutta 方法
 zadvect_implicit                    = 0,       ! [0/1] 选择是否开启 IEVA 方案，默认关闭
 w_crit_cfl                          = 1.2      ! 竖直方向库朗数的临界值，超出此值后开始对垂直速度进行阻尼
                                                !   当 zadvect_implicit 开启时，该值可设为 ~2.0
 zdamp (max_dom)                     = 5000.,   ! 从模式顶部开始的阻尼深度（单位：米）
 w_damping                           = 0,       ! 垂直速度阻尼开关（用于业务运行）：
                                                  0 = 关闭阻尼
                                                  1 = 开启阻尼
 diff_opt(max_dom)                   = 0,       ! 湍流和混合选项：
                                                  0 = 无湍流或显式空间数值滤波（km_opt 将被忽略）
                                                  1 = 在坐标面上计算二阶扩散项，
                                                      若 PBL 方案被使用，则垂直扩散使用 kvdif。
                                                      可与 km_opt = 1 或 4 结合使用（推荐用于真实数据情况）
                                                  2 = 在物理空间（x, y, z）计算混合项（应力形式），
                                                      可通过 km_opt 选择湍流参数化方案
 km_opt(max_dom)                     = 1,       ! 涡流系数选项：
                                                  1 = 常数（使用 khdif 和 kvdif）
                                                  2 = 1.5 阶 TKE 封闭（3D）
                                                  3 = Smagorinsky 一阶封闭（3D）
                                                      注意：选项 2 和 3 不推荐用于 DX > 2 km
                                                  4 = 水平 Smagorinsky 一阶封闭
                                                      （推荐用于真实数据情况）
                                                  5 = SMS-3DTKE 适应尺度 LES/PBL 方案，
                                                      必须与 diff_opt = 2 结合使用，
                                                      且 PBL 方案必须关闭（bl_pbl_physics=0）。
                                                      可与 sf_sfclay_physics = 1, 5, 91 结合使用
 damp_opt                            = 0,       ! 高层阻尼选项：
                                                  0 = 关闭阻尼
                                                  1 = 开启扩散阻尼（推荐用于真实数据情况，dampcoef 取值约 0.01-0.1）
                                                  2 = 开启 Rayleigh 阻尼（仅适用于理想化情况，dampcoef 取值为 1/s，例如 0.003）
                                                  3 = 开启 w-Rayleigh 阻尼（适用于真实数据情况，dampcoef 取值约 0.2）
 use_theta_m                         = 1        ! 1：使用 θ_m = θ (1 + 1.61 Qv)
                                                  0：使用干空气的位温 θ 进行计算
 use_q_diabatic                      = 0        ! 是否在平流过程中包含 Qv 和 Qc 的趋势项：
                                                  0 = 默认行为（不包含）
                                                  1 = 包含 Qv 和 Qc 的趋势项，这有助于在
                                                      理想化“湿对流基准”测试中得到正确解（Bryan, 2014）
                                                      但在真实数据测试中可能需要减少时间步长以维持稳定解

 c_s (max_dom)                       = 0.25     ! Smagorinsky 系数
 c_k (max_dom)                       = 0.15     ! TKE 系数
 diff_6th_opt (max_dom)              = 0,       ! 六阶数值扩散选项
                                                  0 = 关闭六阶扩散（默认）
                                                  1 = 启用六阶数值扩散（不推荐）
                                                  2 = 启用六阶数值扩散，但禁止上坡扩散
 diff_6th_factor (max_dom)           = 0.12,    ! 六阶数值扩散的无量纲速率（最大值 1.0
                                                      对应于在一个时间步内完全消除 2dx 波）
 diff_6th_slopeopt (max_dom)         = 0        ! 如果设置为 1，则打开六阶数值扩散 - 地形坡度调整。默认值为 0（关闭）
 diff_6th_thresh (max_dom)           = 0.10     ! 坡度阈值（m/m），超过该值时，陡峭地形将关闭六阶扩散
 dampcoef (max_dom)                  = 0.,      ! 阻尼系数（见上文）
 base_temp                           = 290.,    ! 真实数据模式，仅 em 模式，海平面基准温度（K）
 base_pres                           = 10^5     ! 真实数据模式，仅 em 模式，海平面基准气压（Pa），请勿更改
 base_lapse                          = 50.,     ! 真实数据模式，仅 em 模式，温度递减率（K），请勿更改
 iso_temp                            = 200.,    ! 真实数据模式，仅 em 模式，平流层参考温度，美国标准大气 216.5 K
 base_pres_strat                     = 0.       ! 真实数据模式，仅 em 模式，平流层底部的基准气压（Pa），
                                                  美国标准大气 55 hPa
 base_lapse_strat                    = -11.     ! 真实数据模式，仅 em 模式，平流层的基准温度递减率（ dT / d(lnP) ），
                                                  近似为美国标准大气 -12 K
 use_baseparam_fr_nml                = .f.,     ! 是否从 namelist 使用基准状态参数
 use_input_w                         = .f.,     ! 是否使用输入文件中的垂直风速
 khdif (max_dom)                     = 0,       ! 水平扩散系数（m²/s）。典型值应为 0.1 * DX（单位：米）
 kvdif (max_dom)                     = 0,       ! 垂直扩散系数（m²/s）。典型值应为 100。
 smdiv (max_dom)                     = 0.1,     ! 散度阻尼（0.1 为典型值）
 emdiv (max_dom)                     = 0.01,    ! 质量坐标模式的外模式滤波系数
                                                  （对于真实数据案例，0.01 是典型值）
 epssm (max_dom)                     = .1,      ! 垂直声波的时间非居中因子
 non_hydrostatic (max_dom)           = .true.,  ! 是否运行无静力假设模式
 pert_coriolis (max_dom)             = .false., ! 科氏力仅作用于风速扰动（理想化）
 top_lid (max_dom)                   = .false., ! 域顶部的垂直运动为零
 mix_full_fields(max_dom)            = .true.,  ! 用于 diff_opt = 2；推荐值为 ".true."，除非是高度理想化的数值测试；
                                                  如果选择 ".true."，则 damp_opt 不能为 1。
                                                  ".false." 表示在混合前减去 1D 基态剖面
 mix_isotropic(max_dom)              = 0        ! 0=各向异性垂直/水平扩散系数，1=各向同性
 mix_upper_bound(max_dom)            = 0.1      ! 扩散系数的无量纲上限
 tke_drag_coefficient(max_dom)       = 0.,      ! 用于 diff_opt=2 时的表面阻力系数（Cd，无量纲）
 tke_heat_flux(max_dom)              = 0.,      ! 用于 diff_opt=2 时的表面热通量（H/(ρ*cp)，K m/s）
 h_mom_adv_order (max_dom)           = 5,       ! 水平动量对流阶数（5=五阶，依此类推）
 v_mom_adv_order (max_dom)           = 3,       ! 垂直动量对流阶数
 h_sca_adv_order (max_dom)           = 5,       ! 水平标量对流阶数
 v_sca_adv_order (max_dom)           = 3,       ! 垂直标量对流阶数
 momentum_adv_opt(max_dom)           = 1,       ! 动量变量的对流选项：
                                                  1=原始方案，3=五阶 WENO
                                                  标量变量的对流选项：0=简单，1=正定，2=单调，3=五阶 WENO，
                                                  4=五阶 WENO + 正定滤波
 moist_adv_opt (max_dom)             = 1        ! 用于水汽
 moist_adv_dfi_opt (max_dom)         = 0        ! 正定 RK3 传输开关。默认值为 0（关闭）
 scalar_adv_opt (max_dom)            = 1        ! 用于标量
 chem_adv_opt (max_dom)              = 1        ! 用于化学变量
 tracer_adv_opt (max_dom)            = 1        ! 用于示踪变量（WRF-Chem 激活）
 tke_adv_opt (max_dom)               = 1        ! 用于 TKE
 phi_adv_z (max_dom)                 = 1        ! 地势势能的垂直对流选项：
                                                  1：原始（默认），2：避免 ω 的双重交错
 time_step_sound (max_dom)           = 4        ! 每个时间步的声波时间步数（0=自动设置）
                                                  （如果 time_step 远大于 6*dx（单位：km），
                                                  则应相应增加声波时间步数，建议使用偶数）
 do_coriolis (max_dom)               = .true.,  ! 是否进行科氏力计算（理想化）
 do_curvature (max_dom)              = .true.,  ! 是否进行曲率计算（理想化）
 do_gradp (max_dom)                  = .true.,  ! 是否计算水平压力梯度（理想化）
 fft_filter_lat                      = 91.      ! 高于该纬度时启用极地滤波（单位：度）- 45 度是一个合理的起始纬度
 coupled_filtering                   = .true.   ! 是否对 mu 耦合的标量数组进行极地滤波
 pos_def                             = .false.  ! 是否将标量数组中的负值设为零
 swap_pole_with_next_j               = .false.  ! 是否用 j=2（jds-2） 的值替换 j=1（jds-1）
 actual_distance_average             = .false.  ! 是否在 j 循环的每个 i 位置上，使用基于地图因子比率的网格点数量进行平均
 gwd_opt (max_dom)                   = 0        ! 运行时是否包含重力波拖曳
                                                  = 1  启用重力波拖曳（Choi 和 Hong 2015）
                                                  = 3  NOAA/GSL 重力波拖曳和湍流地形阻力拖曳
 gwd_diags                           = 0        ! 若 gwd_opt = 3，则设为 1 可输出诊断信息
 sfs_opt (max_dom)                   = 0        ! 非线性回散和各向异性（NBA）关闭
 m_opt (max_dom)                     = 0        ! 不额外输出
 tracer_opt(max_dom)                 = 0        ! 是否启用示踪变量
 rad_nudge                           = 1        ! 在理想化热带气旋案例中，将 T 向初始值调整

&bdy_control  
 spec_bdy_width                      = 5,       ! 指定边界值调整的总行数  
 spec_zone                           = 1,       ! 指定区域中的点数（用于指定边界条件）  
 relax_zone                          = 4,       ! 过渡区域中的点数（用于指定边界条件）  
 specified (max_dom)                 = .false., ! 指定边界条件（只能用于第一层网格）  
                                                  以上四个参数用于真实数据模拟  
 spec_exp                            = 0.       ! 过渡区域调整的指数乘子  
                                                  （0. = 线性调整默认值，例如 0.33 约等于 3*dx 的指数衰减系数）  
 constant_bc                         = .false.  ! 使用 DFI 时的恒定边界条件  

 periodic_x (max_dom)                = .false., ! x 方向上的周期性边界条件  
 symmetric_xs (max_dom)              = .false., ! x 起点（西侧）的对称边界条件  
 symmetric_xe (max_dom)              = .false., ! x 终点（东侧）的对称边界条件  
 open_xs (max_dom)                   = .false., ! x 起点（西侧）的开放边界条件  
 open_xe (max_dom)                   = .false., ! x 终点（东侧）的开放边界条件  
 periodic_y (max_dom)                = .false., ! y 方向上的周期性边界条件  
 symmetric_ys (max_dom)              = .false., ! y 起点（南侧）的对称边界条件  
 symmetric_ye (max_dom)              = .false., ! y 终点（北侧）的对称边界条件  
 open_ys (max_dom)                   = .false., ! y 起点（南侧）的开放边界条件  
 open_ye (max_dom)                   = .false., ! y 终点（北侧）的开放边界条件  
 nested (max_dom)                    = .false., ! 嵌套边界条件（用于嵌套网格时必须启用）  
 polar (max_dom)                     = .false., ! 极地边界条件  
                                                  （在极地最外侧 v 点处 v=0）  
 have_bcs_moist (max_dom)            = .false., ! 仅适用于 ndown 后的模拟：不在边界文件中使用微物理变量  
                                     = .true. , ! 在边界文件中使用微物理变量  
 have_bcs_scalar (max_dom)           = .false., ! 仅适用于 ndown 后的模拟：不在边界文件中使用标量变量  
                                     = .true. , ! 在边界文件中使用标量变量  
 multi_bdy_files                     = .false., ! F=默认；T=模型将使用拆分的单独 LBC 文件运行  
                                                  需要使用 `bdy_inname = "wrfbdy_d<domain>_<date>"`  
                                                  real 程序可以在一次运行中生成所有 LBC 文件  
                                                  每个单独的 LBC 文件仅包含一个时间段  

 &ideal  
 ideal_case                          = 0        ! 理想实验案例编号，选择 module_initialize_ideal.F 中的 CASE  
                                                  真实案例        ideal_case=0  
                                                  2D 山脊（x 方向） ideal_case=1  
                                                  四分之一对称涡流 ideal_case=2  
                                                  对流雷达案例    ideal_case=3  
                                                  2D 飑线（x 方向） ideal_case=4  
                                                  2D 飑线（y 方向） ideal_case=5  
                                                  2D 重力波（x 方向） ideal_case=6  
                                                  B 波             ideal_case=7  
                                                  2D 海风（x 方向） ideal_case=8  
                                                  湍流 LES        ideal_case=9  

 &tc  
                                                  *以下选项仅适用于 tc_em.exe，对 real、ndown 或 model 无影响*  

 insert_bogus_storm                  = .false.  ! 是否插入虚拟热带气旋（TC）  
 remove_storm                        = .false.  ! 是否仅移除原始热带气旋（TC）  
 num_storm                           = 1        ! 虚拟 TC 的数量  
 latc_loc                            = -999.    ! 虚拟 TC 的中心纬度  
 lonc_loc                            = -999.    ! 虚拟 TC 的中心经度  
 vmax_meters_per_second(max_bogus)   = -999.    ! 虚拟 TC 的最大风速（米/秒）  
 rmax                                = -999.    ! 从风暴中心向外的最大半径  
 vmax_ratio(max_bogus)               = -999.    ! 代表最大风速的比率，45 km 网格设为 0.75，15 km 网格设为 0.9  
 rankine_lid                         = -999.    ! TC 生成方案的顶部压力限制  

 &namelist_quilt  
                                                  *此部分用于控制 MPI 应用的异步 I/O*  

 nio_tasks_per_group                 = 0,       ! 默认值 0：无 quilting；>0 表示启用 quilting I/O  
 nio_groups                          = 1,       ! 默认 1，可设置为更高值以进行嵌套 IO 或历史/重启 IO  


&grib2:
 background_proc_id                  = 255,	    ! 背景生成进程标识符，通常由原始中心定义，用于识别创建
                                                  数据时使用的背景数据。这是 GRIB2 消息的第 4 节第 13 个字节。
 forecast_proc_id                    = 255,	    ! 分析或生成预报的进程标识符，通常由原始中心定义，用于
                                                  识别用于生成数据的预报过程。这是 GRIB2 消息的第 4 节第 14 个字节。
 production_status                   = 255,     ! GRIB2 消息中已处理数据的生产状态。请参考 GRIB2 
                                                手册中的代码表 1.3。这是 GRIB2 记录的第 1 节第 20 个字节。
 compression                         = 40,      !用于编码 GRIB2 输出消息的压缩方法。目前仅支持：
                                                40（JPEG2000）
                                                41（PNG）


默认情况下，压力和高度层数据分别存储在数据流 23 和 22 中。使用垂直插值选项时，用户需定义 io_form 和数据流的间隔。详情见 examples.namelist。

 &diags:
 p_lev_diags                         = 1,       ! 是否将诊断数据插值至压力层：
                                                  0=NO, 1=YES
 num_press_levels                    = 0,       ! 指定要插值的压力层数量，例如可设为 2
 press_levels                        = 0,       ! 指定插值的压力层（单位：Pa），例如 85000, 70000
 use_tot_or_hyd_p                    = 2        ! 选择使用的半层压力：
                                                1 = 总压力 (p+pb)
                                                2 = 静力学压力 (p_hyd)（默认，噪声较小）
                                                总压力方式与许多后处理软件的计算方法一致。
 z_lev_diags                         = 0,       ! 是否将诊断数据插值至高度层:
                                                  0=NO, 1=YES
 num_z_levels                        = 2,       ! 指定要插值的高度层数量。
 z_levels                            = 0,       ! 指定要插值的数据高度（单位：m）：
                                                正值：海平面以上的高度（例如飞行高度）
                                                负值：地面以上的高度

AFWA（空军天气局）诊断参数
&afwa
afwa_diag_opt (max_dom) = 0                     ! 是否启用 AFWA 诊断（1：开启）
afwa_ptype_opt (max_dom) = 0                    ! 是否启用降水类型计算（1：开启）
afwa_vil_opt (max_dom) = 0                      ! 是否启用垂直积分液态水计算（1：开启）
afwa_radar_opt (max_dom) = 0                    ! 是否启用雷达选项（1：开启）
afwa_severe_opt (max_dom) = 0                   ! 是否启用严重天气计算（1：开启）
afwa_icing_opt (max_dom) = 0                    ! 是否启用结冰计算（1：开启）
afwa_vis_opt (max_dom) = 0                      ! 是否启用能见度计算（1：开启）
afwa_cloud_opt (max_dom) = 0                    ! 是否启用云诊断（1：开启）
afwa_therm_opt (max_dom) = 0                    ! 是否启用热力指数计算（1：开启）
afwa_turb_opt (max_dom) = 0                     ! 是否启用湍流计算（1：开启）
afwa_buoy_opt (max_dom) = 0                     ! 是否启用浮力计算（1：开启）
afwa_ptype_ccn_tmp = 264.15                     ! 计算降水类型时使用的 CCN 温度
afwa_ptype_tot_melt = 50                        ! 降水类型计算的总融化能量



大气气溶胶数据集
在 real 程序处理中，额外的 3D 数据集可用于垂直插值，通常包括月气溶胶数据。该气溶胶数据的垂直坐标可与输入的气象数据不同。
如需引入新的数据集，需要在 Registry 和 module_initialize_real.F 中进行修改。

示例：

&domains
 num_wif_levels                      = 30    
 wif_input_opt                       = 1
/
 num_gca_levels                      = 13    
 gca_input_opt                       = 1     


物理方案配置
物理选项可通过 physics_suite 指定，支持两种方案：

 mp_physics
 cu_physics
 ra_lw_physics
 ra_sw_physics
 bl_pbl_physics
 sf_sfclay_physics
 sf_surface_physics

with a new namelist physics_suite = 'X'. Two suites are available:

 physics_suite = 'CONUS'

 or 

 physics_suite = 'tropical'

where 'CONUS' is equivalent to

 mp_physics         = 8,
 cu_physics         = 6,
 ra_lw_physics      = 4,
 ra_sw_physics      = 4,
 bl_pbl_physics     = 2,
 sf_sfclay_physics  = 2,
 sf_surface_physics = 2,

and 'tropical' is equivalent to

 mp_physics         = 6,
 cu_physics         = 16,
 ra_lw_physics      = 4,
 ra_sw_physics      = 4,
 bl_pbl_physics     = 1,
 sf_sfclay_physics  = 91,
 sf_surface_physics = 2,

One can use physics_suite, and overwrite one or more options by explicitly including the physics 
namelists.

To overwrite an option for a nest, one can have

 &physics
 physics_suite                       = 'tropical'
 cu_physics                          = -1,    -1,     0,
 ...

here '-1' means to use option specified by the suite, and '0' modifies the cumulus option from the 
suite option 16 to 0 (turning cumulus off).


Hybrid Vertical Coordinate (HVC) vs Terrain Following (TF)

1. To turn ON the HVC
&dynamics
 hybrid_opt         = 2,

1. To turn OFF the HVC
&dynamics
 hybrid_opt         = 0,



Traditional / full fields model output

The WRF model output may be modified at run-time to also include a stream that contains
more traditional fields: temperature, pressure, geopotential height, etc. The flag needs to 
be activated in the namelist (diag_nwp2 = 1). In the registry.trad_fields, the output stream 
is "h1", so that stream needs to be explicitly requested in the namelist.input file.

 &diags
 diag_nwp2 = 1
 /

 &time_control
 io_form_auxhist1                    = 2
 auxhist1_interval                   = 180,  30,   30,
 frames_per_auxhist1                 = 1,     1,    1,
 auxhist1_outname                    = "wrf_trad_fields_d<domain>_<date>"
 /



太阳能诊断输出
启用太阳能诊断后，WRF 将输出相关的 2D 变量（适用于太阳能预报）。如 tslist 文件存在，指定位置的时间序列数据也会包含这些变量。
启用该功能：

 &diags
 solar_diagnostics                   =  1
 /

 所有相关变量计算在 phys/module_diag_solar.F，并定义在 registry.solar_fields 中。
