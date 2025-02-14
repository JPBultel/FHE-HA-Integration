# Fresh setup of Azure VM for FHE-HA Integrated Workflow

VM : [Standard_NV12ads_A10_v5](https://learn.microsoft.com/en-us/azure/virtual-machines/sizes/gpu-accelerated/nvadsa10v5-series?tabs=sizeaccelerators)
Image: [NVIDIA GPU-Optimized VMI with vGPU driver](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/nvidia.nvidia-gpu-optimized-vmi-a10?tab=Overview)

### 1. Environment preparation:

#### 1.1 Environment tools:

```
sudo apt update &&\
sudo apt-get install gcc make
```
#### 1.2 NVIDIA drivers:
```
wget https://download.microsoft.com/download/1/4/4/14450d0e-a3f2-4b0a-9bb4-a8e729e986c4/NVIDIA-Linux-x86_64-535.154.05-grid-azure.run &&\
sudo chmod +x NVIDIA-Linux-x86_64-535.154.05-grid-azure.run
```
```
sudo sh NVIDIA-Linux-x86_64-535.154.05-grid-azure.run --silent
```
#### 1.3 CUDA toolkit:
```
wget https://developer.download.nvidia.com/compute/cuda/12.2.0/local_installers/cuda_12.2.0_535.54.03_linux.run &&\
sudo chmod +x cuda_12.2.0_535.54.03_linux.run
```
```
sudo sh cuda_12.2.0_535.54.03_linux.run --silent --override --toolkit --samples --toolkitpath=/usr/local/cuda-12.2 --samplespath=/usr/local/cuda --no-opengl-libs
```
```
sudo ln -s /usr/local/cuda-12.2 /usr/local/cuda &&\
echo 'export PATH=/usr/local/cuda-12.2/bin:$PATH' >> ~/.bashrc &&\
echo 'export LD_LIBRARY_PATH=/usr/local/cuda-12.2/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc &&\
source ~/.bashrc &&\
sudo chmod -R a+rx /usr/local/cuda-12.2
```

#### 1.4 NVIDIA container toolkit:
```
sudo apt-get install -y nvidia-container-toolkit
```

#### 1.5 Configure Docker Runtime:
```
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker
```

#### 1.6 Include the machine ssh agent to Github (optional):

Create an ssh key for a github account:
`ssh-keygen -t ed25519 -C "user.name@email.com"`
Show the generated ssh key:
`cat ~/.ssh/id_ed25519.pub`
Copy this key to clip board and paste in "New SSH Key" form in Github (Settings, SSH and GPG keys, New SSH key)!
