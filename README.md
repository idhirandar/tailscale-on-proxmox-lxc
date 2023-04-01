# Tailscale install guide on proxmox lxc

Simple yet effective guide to install tailscale on proxmox lxc conatainer in this demonstration i used Ubuntu 20.4 standard for other distro you can used tailscale offecial [guide](https://tailscale.com/download/linux/oracle-linux-8).

## Create a fresh ct on proxmox with privilege permission.

![image](https://github.com/idhirandar/test-01/blob/main/img/01.png)

## Install tailscale on conatainer (from container cli)

- This script will install tailscale package. (from tailscale offcial site )

```shell
curl -fsSL https://tailscale.com/install.sh | sh
```

- Enable ip forwading on container (container cli)

```shell
echo 'net.ipv4.ip_forward = 1' | sudo tee -a /etc/sysctl.d/99-tailscale.conf
echo 'net.ipv6.conf.all.forwarding = 1' | sudo tee -a /etc/sysctl.d/99-tailscale.conf
sudo sysctl -p /etc/sysctl.d/99-tailscale.conf
```

## Edit container file to allow container to intract with proxmox host (from proxmox cli)

- Monunt the tuntap devices to the lxc container from proxmox host.

***( Replace xxx with your_container_id on proxmox)***

```shell
echo 'lxc.cgroup.devices.allow: c 10:200 rwm' >> /etc/pve/lxc/xxx.conf
echo 'lxc.mount.entry: /dev/net/tun dev/net/tun none bind,create=file' >> /etc/pve/lxc/xxx.conf
```

## Restart and enable tailscale services.

```shell
systemctl start tailscale
systemctl start tailscaled
systemctl enable tailscaled
systemctl enable tailcale
```