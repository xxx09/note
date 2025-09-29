# nvidia-driver
sudo apt install nvidia-driver-570-open
安装完之后记得重启电脑

# install issacgym in ubuntu22.04 5090
conda create -n isaacgym1 python=3.8
conda activate isaacgym1
pip install numpy==1.23.5
cd pytorch
export USE_CUDA=1
export TORCH_CUDA_ARCH_LIST="8.0;8.6;8.9;9.0;12.0"
export MAX_JOBS=$(nproc)
pip install pyyaml
pip install typing_extensions
python setup.py bdist_wheel

conda activate isaacgym
#安装其他包
pip install torchvision==0.18.1
pip install torchaudio==2.3.1
pip install numpy==1.23.5

cd /home/xxxxx/IsaacGym_Preview_4_Package/isaacgym/python
conda activate isaacgym
pip install -e .

cd python/examples
conda activate isaacgym
python 1080_balls_of_solitude.py

git clone https://github.com/leggedrobotics/rsl_rl
cd rsl_rl
git checkout v1.0.2
conda activate isaacgym
pip install -e .

conda activate isaacgym
pip install torch-***.whl

git clone https://github.com/unitreerobotics/unitree_rl_gym.git
conda activate isaacgym
pip install -e .

#一般这个时候之前安装的咱们魔改torch会被冲掉，重新安装一次
cd path/to/torch
pip install torch-***.whl
pip install numpy==1.23.5
 
cd legged_gym/scripts
python3  train.py --task=g1

https://blog.csdn.net/m0_56706433/article/details/148902144

问题汇总
- ImportError: libpython3.8.so.1.0: cannot open shared object file: No such file or directory 
- solution 1
- -  运行sudo find / -name 'libpython3.8*' 2>/dev/null，输出为/home/xxx/anaconda3/pkgs/python-3.8.20-he870216_0/lib/libpython3.8.so.1.0 
/home/xxx/anaconda3/pkgs/python-3.8.20-he870216_0/lib/libpython3.8.so
/home/xxx/anaconda3/envs/isaacgym/lib/libpython3.8.so.1.0
/home/xxx/anaconda3/envs/isaacgym/lib/libpython3.8.so
- - solution[deepseek]
在.bashrc中添加下述内容
export LD_LIBRARY_PATH=/home/xxx/anaconda3/envs/isaacgym/lib:$LD_LIBRARY_PATH

- solution 2
- - sudo ln -s /home/xxx/anaconda3/envs/isaacgym/lib/libpython3.8.so.1.0 /usr/lib/libpython3.8.so.1.0

# 新建其他isaacgym环境
```
    conda activate isaacgym
    pip freeze > requirements_isaacgym_frozen.txt
    cat requirements_isaacgym_frozen.txt【把某些内容删除，或者版本信息修改】

    conda create -n host python=3.8
    conda activate host
    pip install -r requirements_isaacgym_frozen.txt
```
- 遇见的问题
- - import torch时报错 ImportError: /home/xxx/anaconda3/envs/host/bin/../lib/libstdc++.so.6: version `GLIBCXX_3.4.30' not found (required by /home/xxx/anaconda3/envs/host/lib/python3.8/site-packages/torch/lib/libtorch_python.so)
- - 解决方法，将/home/xxx/anaconda3/envs/isaacgym/lib/libstdc++.so.6 复制到/home/xxx/anaconda3/envs/host/lib/ 下

    sudo cp -r /home/xxx/anaconda3/envs/isaacgym/lib/libstdc++.so.6 /home/xxx/anaconda3/envs/host/lib/

- 测试是否安装成功
- - cd workspace/rsl_rl, pip install -e .
- - cd workspace/unitree_rl_gym, pip install -e .
- - 可能需要重新安装torch; cd software/pytorch/dist, pip install torch-2.3.0a0+git63d5e92-cp38-cp38-linux_x86_64.whl
- - rewbuffer.extend(cur_reward_sum[new_ids][:, 0].cpu().numpy().tolist()) RuntimeError: Numpy is not available;
- - pip install numpy==1.23.5