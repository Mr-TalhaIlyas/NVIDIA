

## NVML: Driver/library version mismatch

tell us the Nvidia driver kernel module (kmod) have a wrong version, so we should unload this driver, and then load the correct version of kmod

How to do that ?
First, we should know which drivers are loaded.

lsmod | grep nvidia

you may get
`
nvidia_uvm            634880  8
nvidia_drm             53248  0
nvidia_modeset        790528  1 nvidia_drm
nvidia              12312576  86 nvidia_modeset,nvidia_uvm
`
our final goal is to unload `nvidia mod`, so we should unload the module depend on nvidia
`
sudo rmmod nvidia_drm
sudo rmmod nvidia_modeset
sudo rmmod nvidia_uvm
`
then, unload nvidia
`
sudo rmmod nvidia
`
Troubleshooting
if you get an error like `rmmod: ERROR: Module nvidia is in use`, which indicates that the kernel module is in use, you should kill the process that using the kmod:

`sudo lsof /dev/nvidia*`

and then kill those process, then continue to unload the kmods

Test
confirm you successfully unload those kmods

`lsmod | grep nvidia`

you should get nothing, then confirm you can load the correct driver

`nvidia-smi`

you should get the correct output
