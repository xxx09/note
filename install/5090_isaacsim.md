# 5090D + Isaacsim + IsaacLab
# 
```
conda create -n env_isaaclab python=3.10
conda activate env_isaaclab
```

```
pip install --upgrade pip
pip install 'isaacsim[all,extscache]==4.5.0' --extra-index-url https://pypi.nvidia.com
```

## verifying the Isaac Sim installation
```
isaacsim
```

# IsaacLab
```
git clone https://github.com/isaac-sim/IsaacLab.git
sudo apt install cmake build-essential
./isaaclab.sh --install # or "./isaaclab.sh -i"
```
**For 50 series GPUs,please use the latest PyTorch nightly build instead of PyTorch 2.5.1, which comes with Isaac Sim:**

```
pip install --upgrade --pre torch torchvision --index-url https://download.pytorch.org/whl/nightly/cu128
```

### 安装报错
-  1. time out
  ```
  
  ```
  ```
Cloning https://github.com/ARISE-Initiative/robomimic.git (to revision v0.4.0) to /tmp/pip-install-tfcen5tv/robomimic_afe627348d7c4e87a7995076693847e1
  Running command git clone --filter=blob:none --quiet https://github.com/ARISE-Initiative/robomimic.git /tmp/pip-install-tfcen5tv/robomimic_afe627348d7c4e87a7995076693847e1
  fatal: unable to access 'https://github.com/ARISE-Initiative/robomimic.git/': Failed to connect to github.com port 443 after 132582 ms: Connection timed out
  error: subprocess-exited-with-error
  
  × git clone --filter=blob:none --quiet https://github.com/ARISE-Initiative/robomimic.git /tmp/pip-install-tfcen5tv/robomimic_afe627348d7c4e87a7995076693847e1 did not run successfully.
  │ exit code: 128
  ╰─> See above for output.
```
- ### solution 打开一个终端
``` 
git clone --filter=blob:none --quiet https://github.com/ARISE-Initiative/robomimic.git /tmp/pip-install-sc2ya3p5/robomimic_7eb7379dc66f445dbd701daaa7b6466e
```


- 2. warning log
  
  ```
  [Warning] [isaaclab.utils.math] the function 'quat_rotate_inverse' will be deprecated in favor of the faster method 'quat_apply_inverse'. Please use 'quat_apply_inverse' instead....
  ```
  - solution:  uncomment the isaaclab/isaaclab/utils/math.py/ quat_rotate_inverse l714-718
    ```
    # omni.log.warn(
    #     "The function 'quat_rotate_inverse' will be deprecated in favor of the faster method 'quat_apply_inverse'."
    #     " Please use 'quat_apply_inverse' instead...."
    # )
    ```

- 3. Error log
  ```
  File "/home/xxx/anaconda3/envs/isaacsim/lib/python3.10/site-packages/torch/jit/_script.py", line 1214, in _script_impl
    fn = torch._C._jit_script_compile(
  RuntimeError: 
  Unknown type name 'Level':
  File "/home/xxx/anaconda3/envs/isaacsim/lib/python3.10/site-packages/omni/kernel/py/omni/log/__init__.py", line 29
  ...
  File "/home/xxx/workspace/leggedlab/legged_lab/rl/rsl_rl/datasets/mira_motion_loader.py", line 12
  ```
  solution: comment @torch.jit.script
  ```
    legged_lab/rl/rsl_rl/datasets/mira_motion_loader.py
    # @torch.jit.script
  ```

- 4. 使用报错; 当vpn连不上时
    ```
    [omni.hydra.scene_delegate.plugin] Calling getBypassRenderSkelMeshProcessing for prim /World/envs/env_0/Robot/imu_link/visuals.proto_mesh_id0 that has not been populated
    FileNotFoundError: USD file not found at path: 'http://omniverse-content-production.s3-us-west-2.amazonaws.com/Assets/Isaac/4.5/Isaac/Props/UIElements/arrow_x.usd'.

    ```
  - solution; 下载离线资源库，将每次运行时load的环境改为资源库的地址 
    - Isaac Sim Assets [https://docs.isaacsim.omniverse.nvidia.com/4.5.0/installation/download.html]
    - 文件解压后放到同一级目录下，比如 /home/xxx/assets/Assets/Isaac/4.5
    - IsaacLab\source\isaaclab\isaaclab\utils\assets.py ; modify l24
      ```
        NUCLEUS_ASSET_ROOT_DIR = carb.settings.get_settings().get("/persistent/isaac/asset_root/cloud")
      ```
      to 
      ```
      NUCLEUS_ASSET_ROOT_DIR = ("/home/xxx/assets/Assets/Isaac/4.5")
      ```

#  urdf转为usd
cd Isaaclab
conda activate isaacsim
python scripts/tools/convert_urdf.py /home/xxx/workspace/leggedlab/legged_lab/assets/urdf/mirax_3/urdf/mirax3.urdf /home/xxx/workspace/leggedlab/legged_lab/assets/mirax_3/mirax_3.usd --joint-stiffness 0.0 --joint-damping 0.0 --joint-target-type none
python scripts/tools/convert_urdf.py /home/xxx/workspace/leggedlab/legged_lab/assets/urdf/mirax_1/urdf/mirax1.urdf /home/xxx/workspace/leggedlab/legged_lab/assets/mirax_1/mirax_1.usd --joint-stiffness 0.0 --joint-damping 0.0 --joint-target-type none
