# 雙顯卡多系統同時輸出

2024/12/26 未完成

## 前言

## 主系統規格

- Fedora 41
- Desktop: KDE Plasma 6.2.4
- CPU: AMD Ryzen 7 1700
- RAM: 32GB
- GPU1: NVIDIA GeForce GTX 1070 Ti
- GPU2: NVIDIA GeForce GTX 4060 Ti

## 安裝 qemu-kvm

```bash
sudo dnf update
sudo dnf install qemu-kvm virt-manager
```

```bash
lspci -nnk | grep -i nvidia
```

Output:

```bash
0b:00.0 VGA compatible controller [0300]: NVIDIA Corporation AD106 [GeForce RTX 4060 Ti] [10de:2803] (rev a1)
        Kernel driver in use: nvidia
        Kernel modules: nouveau, nvidia_drm, nvidia
0b:00.1 Audio device [0403]: NVIDIA Corporation AD106M High Definition Audio Controller [10de:22bd] (rev a1)
0c:00.0 VGA compatible controller [0300]: NVIDIA Corporation GP104 [GeForce GTX 1070 Ti] [10de:1b82] (rev a1)
        Kernel driver in use: nvidia
        Kernel modules: nouveau, nvidia_drm, nvidia
0c:00.1 Audio device [0403]: NVIDIA Corporation GP104 High Definition Audio Controller [10de:10f0] (rev a1)
```

```bash
echo 'vfio-pci' > /etc/modules-load.d/vfio-pci.conf
```

編輯 `/etc/modprobe.d/vfio.conf`
```bash
options vfio-pci ids=10de:1b82,10de:10f0
```

```bash
sudo nano /etc/default/grub
```

在 `GRUB_CMDLINE_LINUX_DEFAULT=""` 最後面加上 `amd_iommu=on iommu=pt kvm.ignore_msrs=1 vfio-pci.ids=10de:1b82,10de:10f0`，如下：

```bash
GRUB_CMDLINE_LINUX_DEFAULT=" amd_iommu=on iommu=pt kvm.ignore_msrs=1 vfio-pci.ids=10de:1b82,10de:10f0"
```

```bash
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

```bash
sudo reboot
```

```bash
sudo dmesg | grep -i vfio
```

Output:
```bash
vfio-pci.ids=10de:1b82,10de:10f0
```

```bash
nvidia-smi
```

Output:
```bash
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 565.77                 Driver Version: 565.77         CUDA Version: 12.7     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA GeForce RTX 4060 Ti     Off |   00000000:0B:00.0  On |                  N/A |
|  0%   43C    P8             11W /  160W |    1074MiB /   8188MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+
                                                                                         
+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI        PID   Type   Process name                              GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|    0   N/A  N/A      2292      G   /usr/bin/Xwayland                               2MiB |
|    0   N/A  N/A      3341      G   /usr/bin/konsole                                2MiB |
+-----------------------------------------------------------------------------------------+
```

## 安裝 Windows 11

