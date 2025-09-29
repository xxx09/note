| 日期 | 超参 | 效果 |
|:------ | :------: | --------:|
| 250811 | 4096 dof25  | 起身几乎不成功，姿态很怪异；坐在地上没起来 |
| 250811 | 4096 dof25  | 训练效果几乎一模一样 |
| 250812 | 4096 dof25 hip_roll_joint[-0.5, 0.5]  | 腿折着站起来 |
| 250812 | 4096 dof25 hip_roll_joint[-0.5, 0.5] 新的mesh，腰部用完整mesh，屁股用5个点代替，大小腿用完整的mesh； | 先翻身再起来； |
| 250812 | 4096 dof25 base_link后加一个腰垫； | 右腿水平伸直，左腿水平伸直小腿内屈再站立起来，大部分蹲坐在原地 |
| 250812 | 4096 dof25 mirax的训练 roll_joint[-0.5, 1.5] | 先做起来，左腿屈起，再站立起来，相对姿态最正常，也有相当一部分机器人坐在地上 |
| 250813 | 4096 dof25 mirax1的训练 roll_joint[-0.5, 1.5] 移动mesh| play有问题，就蹲坐在原地 |
| 250813 | 8192 auxiliary_ankle dof25 mirax的训练 roll_joint[-0.5, 1.5] torso1_link | 先翻身，折腿再站起来|
| 250813 | 8192 auxiliary_ankle dof25 mirax1的训练 新的mesh roll_joint[-0.5, 1.5] torso1_link | 先翻身，折腿再站起来 |
| 250814 | 10240 修改关节角度的范围，主要为waist_yaw_joint和hip_pitch_joint, | 正身起来，撑臂撑腿挺腰坐起来，左腿屈起来，右腿伸直再站立起来； |
| 250818 | 12288 原始的urdf | 翻身折腿，再转回来，起身； seed=1未收敛，seed=42收敛；|
| 250818 | 12288 原始的urdf | 翻身折腿，再转回来，起身|
| 250819 | 10240 新的上半身mesh | 全身弓起来，往左边撑起身体，左腿折右腿直身体前压，左膝顶起来，|
| 250819 | 10240 新的上半身mesh，ground_prone | 先伸直，再把身体抬起来，再屈腿，翻踝再站起来|
| 250821 | 4096 新的上半身mesh，ground_prone | 抬腿打腿，挺身再站起来； sim2sim失败，mujoco中腿抬得很高，上半身挺不起来，一直往前倾倒； <br> 经过一系列调试，可通过降低torso_link的质量，或增加脚踝的质量，在mujoco中能起身|
| 250821 | 4096 mirax3接unitree的上半身 | 先跪地同时撑手，再拧膝盖站起来; 在mujoco和sim中一致|
| 250825 | 4096 mirax3接unitree的torsolink | sim2mujoco失败|
| 250825 | 4096 mirax3手臂末端接unitree的手臂 | sim2mujoco失败|
| 250826 | 4096 新的上半身mesh，ground_prone, friction 0.07 damping 0.02 | 撅屁股侧身，手撑地改为跪膝，再抬脚站起来； sim2mujoco测试成功|
| 250826 | 4096 新的上半身mesh，ground_prone, friction0.08，damping0.01 | ; 动作和friciton007一样，但跪在地上没站起来；|

【warning】
host_prone: no_orientation不能置为false；

【todo】
胸前加一个link， 如果触地则terminate; 
对orientation进行惩罚
将上半身的把手去除掉试下看
将手臂增加一下长度，看看是否能正身起来； 修改urdf和mesh；

总结：
1. 增加腿踝的质量，或者降低身体的质量，同一个模型能让机器人反身起来；
2. 

# 将下面每个变量从0开始排序
0. 'base_link'
1. 'left_hip_pitch_link'
2. 'left_hip_roll_link'
3. 'left_hip_yaw_link'
4. 'left_knee_link'
5. 'left_ankle_pitch_link'
6. 'auxiliary_left_ankle_pitch_link1'
7. 'auxiliary_left_ankle_pitch_link2'
8. 'auxiliary_left_ankle_pitch_link3'
9. 'auxiliary_left_ankle_pitch_link4'
10. 'left_ankle_roll_link'
11. 'right_hip_pitch_link'
12. 'right_hip_roll_link'
13. 'right_hip_yaw_link'
14. 'right_knee_link'
15. 'right_ankle_pitch_link'
16. 'auxiliary_right_ankle_pitch_link1'
17. 'auxiliary_right_ankle_pitch_link2'
18. 'auxiliary_right_ankle_pitch_link3'
19. 'auxiliary_right_ankle_pitch_link4'
20. 'right_ankle_roll_link'
21. 'torso_link'
22. 'keyframe_head_link'
23. 'keyframe_torso1_link'
24. 'keyframe_torso1_link2'
25. 'left_shoulder_pitch_link'
26. 'left_shoulder_roll_link'
27. 'left_shoulder_yaw_link'
28. 'left_elbow_pitch_link'
29. 'left_elbow_yaw_link'
30. 'right_shoulder_pitch_link'
31. 'right_shoulder_roll_link'
32. 'right_shoulder_yaw_link'
33. 'right_elbow_pitch_link'
34. 'right_elbow_yaw_link'
