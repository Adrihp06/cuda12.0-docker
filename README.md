# Nvidia-docker in latest GPUS
<p align="center">
<a href="http://nvidia.com/" target="blank"><img src="https://assets.nvidiagrid.net/ngc/logos/OSS-Nvidia-Partnership-Tensorflow.png" width="400" alt="Nvidia-tensorflow" /></a>
</p>

To configure Cuda and Tensorflow in a newer GPU, you should use Nvidia-docker and then install the latest container of tensorflow in NGC. 
With that you should have an docker image that can use your GPU tu run neural networks.

## Installation
We cover the installation on a Ubuntu/Debian os which has the nvidia drivers already insalled,  if you have another os you can check the documentation. We should start installing [nvidia-docker](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html).

### Install and setting up docker
```
curl https://get.docker.com | sh \
  && sudo systemctl --now enable docker
```

### Setting up the nvidia container
```
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
      && curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
      && curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | \
            sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
            sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```

### Install the nvidia container
```
sudo apt-get update
sudo apt-get install -y nvidia-docker2
```

### Restart the docker daemon
```
sudo systemctl restart docker
```

### Check that the installation has been done correctly
```
sudo docker run --rm --gpus all nvidia/cuda:11.6.2-base-ubuntu20.04 nvidia-smi
```
You should look something like this:
```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 450.51.06    Driver Version: 450.51.06    CUDA Version: 11.0     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  Tesla T4            On   | 00000000:00:1E.0 Off |                    0 |
| N/A   34C    P8     9W /  70W |      0MiB / 15109MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+
```

## Tensorflow
Now we have to install the tensorflow container. First, sing up in [NGC](https://catalog.ngc.nvidia.com/orgs/nvidia/collections/gettingstarted). This allow you to check all versions
of tensorflow that you can install in your docker container.
The installation of the container can be made up simply, using the following command.

```
docker run --gpus all -it --rm nvcr.io/nvidia/tensorflow:xx.xx-tfx-py3
```

In my case, with a rtx 4080 the tensorflow version was 23.01.


```
docker run --gpus all -it --rm nvcr.io/nvidia/tensorflow:23.01-tf2-py3
```

<h2 align="center">Stack used</h2>
<p align="center">
<a href="#"><img src="https://img.shields.io/badge/-Docker-2496ED?style=for-the-badge&logo=Docker&logoColor=white"></a>
<a href="#"><img src="https://img.shields.io/badge/-Nvidia-76b900?style=for-the-badge&logo=Nvidia&logoColor=white"></a>
<a href="#"><img src="https://img.shields.io/badge/-Tensorflow-FF6F00?style=for-the-badge&logo=Tensorflow&logoColor=white"></a>
</p>
