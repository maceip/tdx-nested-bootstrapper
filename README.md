
<img src=https://github.com/user-attachments/assets/e01542cd-2b18-4693-a7ff-a9bfa932affa width="100" height="100">

# nested virtualization bootstrapping of confidential / tee infra



gcp only 

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
/run.sh -i ubuntu-24.04-server-cloudimg-amd64.img -t 10
```

boot the guest
```
sudo ./qemu-test.sh -i output.qcow2 -b grub -q vsock
```

