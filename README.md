
<img src=https://github.com/user-attachments/assets/e01542cd-2b18-4693-a7ff-a9bfa932affa width="100" height="100">

# bootstrapping trust in cloud with nested virtualization
![l3](https://github.com/user-attachments/assets/37e07c3e-4d37-4d18-a346-133b6d4b7fa3)
>[!IMPORTANT]
>this repo bootstraps trust using nested virtualiation (L2), which is experimental. Intel advocates this be done on metal


**gcp only:**

```
gcloud beta compute instances create gold     --machine-type=c3-standard-4     --zone=us-central1-a     --confidential-compute-type=TDX     --maintenance-policy=TERMINATE     --image-family=ubuntu-2204-lts     --image-project=tdx-guest-images     --project=$PROJ   --enable-nested-virtualization --boot-disk-size=1024G
```

fetch

```
sudo apt install qemu-utils guestfs-tools virtinst genisoimage libvirt-daemon-system libvirt-daemon
```

virt
```
sudo usermod -aG libvirt $USER
```
chmod
```
sudo chmod o+r /boot/vmlinuz-*
```

cloud init
```
wget https://ftp.debian.org/debian/pool/main/c/cloud-init/cloud-init_24.2-1_all.deb
sudo dpkg -i cloud-init_24.2-1_all.deb
```
    
img to convert 
    
```
https://cloud-images.ubuntu.com/releases/noble/release/ubuntu-24.04-server-cloudimg-amd64.img
```

    

refresh
    ```
    sudo usermod -aG libvirt $USER
    sudo systemctl daemon-reload
    sudo systemctl restart libvirtd
    ```


start ish
    ```
    virsh net-start default
    ```

convert -- its slow 
```
sudo ./run.sh -i ubuntu-24.04-server-cloudimg-amd64.img -t 10 -s /var/run/libvirt/libvirt-sock
```

boot the guest
```
sudo ./qemu-test.sh -i output.qcow2 -b grub -q vsock
```

