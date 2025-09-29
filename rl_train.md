# Mirax training
4096 一次用时1.6s, 8192一次用时2.2s；

| 日期 | 超参 | 效果 |
|:------ | :------: | --------:|
| 250707 | plane   | train5k->len2.3k; rw=200 |
| 250707 | plane + slope   | train15k->len1.9k; rw=100 |
| 250708 | 12288; ref_joint_pos rw_coef=0.2;  | sim2mujoco能走斜坡 |
| 250709 | 4096; ref_joint_pos rw_coef=0.2; default_joints[-0.3,0.6,-0.3]; <br> des_base_height 0.68  | --- |
| 250710 | lstm  ref_joint_pos rw_coef=0.02; default_joints[-0.3,0.6,-0.3]; <br>  no sym max_feet_height=0.12 | ---|
| 250711 | 12288 ref_joint_pos rw_coef=2.2; default_joints[-0.3,0.6,-0.3]; <br>  no sym max_feet_height=0.12 | return 较高，episode_len相对较长|
| 250711 | lstm  ref_joint_pos rw_coef=2.2; default_joints[-0.3,0.6,-0.3]; <br>  no sym max_feet_height=0.12 | infer在仿真中走斜坡的效果更好 |
| 250714 | 4096  ref_joint_pos rw_coef=2.2; default_joints[-0.3,0.6,-0.3]; <br>  max_feet_height=0.12 | ---  |
| 250715 | lstm  ref_joint_pos rw_coef=2.2; default_joints[-0.3,0.6,-0.3]; <br>  no sym max_feet_height=0.12 | --- |
| 250715 | 6144  ref_joint_pos rw_coef=2.2; default_joints[-0.3,0.6,-0.3]; <br>  max_feet_height=0.24 | 仔细分析reward，站立时，ref_pos和feet_height的reward较高，导致整体reward尚可； <br>实际角度跟踪很差，所以不抬脚  |
| 250716 | 4096  ref_joint_pos rw_coef=2.2; default_joints[-0.3,0.6,-0.3] <br> alive=0.2, stand prob 0.1, slope 0.15->0.35, enable_height_scan， base_height=0.72; <br>  max_feet_height=0.24 | 抬脚高度够，但脚尖朝下，容易摔倒-  |
| 250717 | 4096  ref_joint_pos rw_coef=0.02; default_joints[-0.3,0.6,-0.3] <br> terminate， default_joint_pos_rw, rw_feet_period_force\vel，  <br>  max_feet_height=0.16 | 脚尖仍旧朝下，同时甩腿严重，往外撇腿  |
| 250718 | 4096  ref_joint_pos rw_coef=0.02; default_joints[-0.3,0.6,-0.3] <br> 添加feet_orientation函数，feet_period_vel只用z方向速度，增大defalut_joint_pos系数，添加no_yaw_roll函数，  <br>  max_feet_height=0.16 | 脚尖仍旧朝下，行走时腿往左拐；feet_orientation、no_yaw_roll得分很低  |
| 250718 | 4096  ref_joint_pos rw_coef=1.2; default_joints[-0.3,0.6,-0.3] <br> 降低feet_orientation、no_yaw_roll函数中exp系数，降低feet_height的rw系数、加大其exp系数  <br>  max_feet_height=0.16 | 平地行走抬脚不高，但走台阶时会抬脚； 整体效果较好  |
| 250718 | 4096 LSTM ref_joint_pos rw_coef=1.2; default_joints[-0.3,0.6,-0.3] <br> 降低feet_orientation、no_yaw_roll函数中exp系数，降低feet_height的rw系数、加大其exp系数  <br>  max_feet_height=0.16 | 行走正常，但上楼梯效果不好，  |
| 250721 | 4096  obs中添加高度图，ref_joint_pos rw_coef=1.2; default_joints[-0.3,0.6,-0.3] <br> 降低feet_orientation、no_yaw_roll函数中exp系数，降低feet_height的rw系数、加大其exp系数  <br>  max_feet_height=0.16 | 走到台阶前不动；保持站立姿态  |
| 250721 | 4096  obs中添加高度图，Heading_command 设置为True，有高度信息，高度台阶不一样，观测值中含高度信息， 增加三级上台阶 | 下台阶走到台阶前不动；保持站立姿态  |
| 250722 | 4096  dwl, height加入critic每一帧，heightmap的resolution改为0.1，则维度为17*11，多种台阶 | 可以上12cm台阶，上三级台阶不是很连续；  |
| 250723 | 4096  ppo, height加入critic每一帧，heightmap的resolution改为0.1，则维度为17*11，多种台阶 | 可以上12cm台阶，上三级台阶也可以；相对dwl要好  |
| 250724 | 4096  Mirax1、Mirax2站立训练，效果不太好；针对base的vel和ang_vel做惩罚，可以将晃动抑制住；  | 但目前MiraX2使用的是全站立环境训练, 整体效果还可以 |
| 250725 | 4096  Mirax2站立训练；针对base的vel和ang_vel做惩罚；全站立环境  | ，可以将晃动抑制住，但目前MiraX2使用的是全站立环境训练, 整体效果还可以 |
| 250725 | 4096  Mirax1站立训练 针对base和left_shoulder的vel和ang_vel做惩罚， 0.3的站立环境  | 0.3的站立环境还可以，但不如全站立的稳；使用的1*norm(abs_vel) |
| 250725 | 4096  Mirax1站立训练 针对base和left_shoulder的vel和ang_vel做惩罚， 0.3的站立环境 + 全地形reward  | 效果不太好，站不了很稳； |
| 250725 | 4096  Mirax1站立训练 针对base和left_shoulder的vel和ang_vel做惩罚， 1的站立环境 + 老的reward  | 真机测试较稳，与仿真接近； |
| 250725 | 4096  Mirax1站立训练 针对base和left_shoulder的vel和ang_vel做惩罚， 0.3的站立环境  + 老的reward | 行走正常，站立有点晃； |
| 250725 | 4096  MiraX1全地形，多种台阶，reward加入了站立不晃的reward， | 晃动好一点，勉强能上12cm的台阶[速度要有停顿]，上台阶时容易右转，可以上0.3rad的斜坡；terrain最后到4 |
| 250728 | 4096  Mirax1站立训练 针对base和left_shoulder的vel和ang_vel做惩罚， 0.3的站立环境  + 老的reward + imu_offset_false | 站立时仍旧有点晃； |
| 250728 | 4096  Mirax1全地形， 增加torso_link, base_link 的mass和com的随机化； friction中make_consistent设置为True； privilege增加com_offset，增加之前的电机相关偏置信息； torso增加外扰力和力矩【暂未增加到本体信息内】；| 晃动没有变化，上台阶12cm上不了，terrain最后到3 |
| 250729 | 4096  Mirax1全地形， terrain_level由20改为10，com使用质心，抬腿高度设置为0.1;；| 整体效果更差 |
| 250729 | 4096  Mirax1全地形， terrain_level由20改为10，com使用质心，抬腿高度0.16; 去掉外力| 目前效果相对较好 |
| 250729 | 4096  Mirax1全地形， 抬腿高度0.16; 去掉body的mass scale ； 修改terrain变化条件| 崎岖地面基本不走，保持站立，terrain变化条件还不够合理，随机化导致容易进入下一个阶段；复杂地形需要更复杂的设计|
| 250730 | 4096  Mirax1全地形; 修改terrain变化条件;增加脚yaw角接近的奖励;| 训练出来了直膝行走，也能上楼梯|
| 250730 | 4096  Mirax1全地形; 修改terrain变化条件;增加脚yaw角接近的奖励; yaw角初始位置设置为+-0.15| 屈膝行走，能上16cm楼梯，行走姿态更好看； 右腿比左腿抬得高(原地转弯更明显)，直线行走会往左偏一点[当前真机部署使用的该模型]|
| 250730 | 4096  Mirax1全地形; dwl| terrain收敛很慢|
| 250731 | 4096  Mirax1全地形; 添加额外的力，间隔[5,8]| terrain收敛很慢，抬脚不高|
| 250731 | 4096  Mirax1全地形; 只有额外的力，不添加速度push| terrain到2，16cm楼梯ok，上斜坡不行，抬脚不高|
| 250801 | 4096  Mirax1全地形; feet_height和feet_vel的奖励函数 | terrain较好，抬脚高，上斜坡、走楼梯都可以，但不太稳；|
| 250801 | 16384  Mirax1全地形; feet_height和feet_vel的奖励函数 default 363 | terrain到3.5, 抬脚高，走各种地形都较好，30k时直走有点往左偏，25k时直走较直；可上16cm，0.3rad斜坡，且从斜坡可直接走下来；站立时晃动大一些；|
| 250801 | 16384  Mirax1全地形; feet_height和feet_vel的奖励函数 default 121 | terrain到3.5，站立时笔直，走路时会屈膝【怀疑是base_height奖励导致】，但整体稳定性稍差【站立时晃动更大， 斜坡不太能站立住， episode_len稍差】；|
| 250815 | 24576  Mirax1全地形; feet_height和feet_vel的奖励函数 default 363 |30k直走时有点往左偏；26k右偏，27k直行，28k左偏；站立时晃动变小，身体有点前倾斜；  |
| 250815 | 24576  Mirax1全地形; feet_height和feet_vel的奖励函数 default 363 standenvs02 | 27k时可以执行，站立时晃动更小|
| 250826 | 8192  Mirax3全地形; standenvs015， damping0.01， friction 0.006 | 走路时拖着脚走路|
| 250829 | 24576  Mirax3全地形; standenvs01，  | 上大斜坡不稳定，下楼梯两个脚容易打架，容易摔倒|

| 250906 | 24576  Mirax3全地形; standenvs01 | 站立时晃动厉害，走路时右腿内撇|
| 250906 | 24576  Mirax3全地形; standenvs02 | 走路时右腿内撇， 下楼梯不稳，容易摔倒|