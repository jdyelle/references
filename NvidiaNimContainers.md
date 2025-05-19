Check the version of WSL to make sure it's 5+
```
PS C:\Users\jeremy.yelle> wsl --version
WSL version: 2.3.24.0
Kernel version: 5.15.153.1-2
WSLg version: 1.0.65
MSRDC version: 1.2.5620
Direct3D version: 1.611.1-81528511
DXCore version: 10.0.26100.1-240331-1435.ge-release
Windows version: 10.0.26100.3775
PS C:\Users\jeremy.yelle>
```

Download the [Nvidia Toolkit for WSL](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=WSL-Ubuntu&target_version=2.0&target_type=deb_local)  
There are instructions on the page for instlling it:
```
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin
sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/12.9.0/local_installers/cuda-repo-wsl-ubuntu-12-9-local_12.9.0-1_amd64.deb
sudo dpkg -i cuda-repo-wsl-ubuntu-12-9-local_12.9.0-1_amd64.deb
sudo cp /var/cuda-repo-wsl-ubuntu-12-9-local/cuda-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
sudo apt-get -y install cuda-toolkit-12-9
```

Follow this gist to install docker on WSL2:  
https://gist.github.com/dehsilvadeveloper/c3bdf0f4cdcc5c177e2fe9be671820c7

Install the nvidia container toolkit:
https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#installing-the-nvidia-container-toolkit

Sometimes there is an error with permissions for the newer GPUs
For me, this was addressed by editing `config.toml` in the nvidia-container-runtime and setting `no-cgroups` to `false`
```
sudo vi /etc/nvidia-container-runtime/config.toml
```


Log in to Docker:
```
export NGC_API_KEY="nvapi-K1I********************************EuRH"
docker login nvcr.io --username '$oauthtoken' --password "${NGC_API_KEY}"
export LOCAL_NIM_CACHE=~/.cache/nim
mkdir -p "$LOCAL_NIM_CACHE"
chmod 777 "$LOCAL_NIM_CACHE"
docker run -d --name Deepseek-R1-Distill-Qwen-7B --gpus all -e NGC_API_KEY -v "$LOCAL_NIM_CACHE:/opt/nim/.cache" -u $(id -u) -p 8000:8000 nvcr.io/nim/deepseek-ai/deepseek-r1-distill-qwen-7b:latest
docker run -d --name llama3-8b-instruct --gpus all -m 16384m -e NGC_API_KEY --env NIM_RELAX_MEM_CONSTRAINTS=1 -v "$LOCAL_NIM_CACHE:/opt/nim/.cache" -u $(id -u) -p 8000:8000 nvcr.io/nim/meta/llama3-8b-instruct:latest
```

