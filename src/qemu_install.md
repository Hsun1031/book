# Qemu 安裝

2024/12/26 未完成

## CentOS 8

```bash
sudo dnf update
sudo dnf install qemu-kvm libvirt virt-install bridge-utils virt-manager
```

## Group

```bash
sudo usermod -aG libvirt $USER
sudo usermod -aG kvm $USER
```

```bash
sudo systemctl enable libvirtd --now
```

```bash
sudo reboot
```

![qemu_install01](https://filedn.eu/lWwCEAKJdY2RCGaYcUeyohJ/book/images/qemu_install01.png)