# rl_tips
# 1. terrain
- 地形的变化条件，不能过于简化，要稍微严苛一些；比如站立到位就进入下一阶段；


# 2. 域随机化
- 随机外力的添加，会导致训练变困难；

# 3. sim2sim
- 训练mirax3起身时，sim2sim部署遇到问题，isaacgym中能起身，mujoco中则无法实现；
- - 仔细比对mujoco和isaacgym中参数的差异；发现是hip_pitch关节的特性不一致，导致isaacgym中能起身，mujoco中则不行；修改isaacgym中aramature，damping和friction【最重要】；增大关节friction会导致isaacgym中与mujoco相似，但并非完全一致；