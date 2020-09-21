# Setup CUDA v10.2 and CUDNN v8.02 on Ubuntu 18.04

Note: Heavily inspired by https://medium.com/@sh.tsang/tutorial-cuda-v10-2-cudnn-v7-6-5-installation-ubuntu-18-04-3d24c157473f

If you have any NVIDIA packages preinstalled, remove them:
```
sudo rm /etc/apt/sources.list.d/cuda*
sudo apt remove --autoremove nvidia-cuda-toolkit
sudo apt remove --autoremove nvidia-*
```

## CUDA v10.2 Installation
- Go to https://developer.nvidia.com/cuda-downloads
- Select the target platform based on your OS setting.
- I got the *runfile (local)*
- After downloading run the following command:

```
sudo sh cuda_10.2.89_440.33.01_linux.run
```

 **Note**: You may have to run this without the GUI. You can do this with Ctrl+Alt+F1 on the login screen. See X Server Error for more details.

### X Server Error

```
ERROR: You appear to be running an X server; please exit X before installing. For further details, please see the section INSTALLING THE NVIDIA DRIVER in the README available on the Linux driver download page at www.nvidia.com
```

From https://askubuntu.com/questions/149206/how-to-install-nvidia-run and https://forums.fedoraforum.org/showthread.php?218463-VNC-Server-Warning-localhost-1-is-taken-because-of-tmp-X1-lock

Run the following steps:

1. Hit Ctrl+Alt+F1 and login using your credentials.
2. Kill your current X server session by typing `sudo service lightdm stop` or `sudo lightdm stop`
3. Enter runlevel 3 by typing `sudo init 3`
4. Install your *.run file.
5. `sudo reboot`

Another issue may be the file `/tmp/.X1-lock`. If you've followed the above steps and you're still getting the X error, you may have the `.X1-lock` file in your `/tmp/` dir. Delete it with:
```
sudo rm /tmp/.X1-lock
```

After that, you should be fine.

### Nouveau Error
```
ERROR: The Nouveau kernel driver is currently in use by your system...
```
From https://askubuntu.com/questions/841876/how-to-disable-nouveau-kernel-driver

Create a file
```
sudo nano /etc/modprobe.d/blacklist-nouveau.conf
```
Put the following content in the file:
```
blacklist nouveau
options nouveau modeset=0
```
Regenerate the kernel initramfs:
```
sudo update-initramfs -u
```
Reboot
```
sudo reboot
```

## Add to path

Edit `~/.bashrc`:
```
sudo nano ~/.bashrc
```

Add the following to the end of the file:
```
export PATH=/usr/local/cuda/bin:$PATH  
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
```

Source the `.bashrc`
```
source /.bashrc
```

Check the CUDA version to see if it is successfully installed
```
nvcc -V
```

## cuDNN v8.03 Installation
Download the library from https://developer.nvidia.com/rdp/cudnn-download. You'll need to make an account to access the files. Download the following files:
```
cuDNN Runtime Library for Ubuntu18.04 (Deb)
cuDNN Developer Library for Ubuntu18.04 (Deb)
cuDNN Code Samples and User Guide for Ubuntu18.04 (Deb)
```

Install them:
```
libcudnn8_8.0.3.33-1+cuda10.2_amd64.deb
libcudnn8-dev_8.0.3.33-1+cuda10.2_amd64.deb
libcudnn8-samples_8.0.3.33-1+cuda10.2_amd64.deb
```

To verify, copy the sample files to:
```
cp -r /usr/src/cudnn_samples_v8/ ~
```

Then change the directory:
```
cd ~/cudnn_samples_v8/mnistCUDNN
```

Make file:
```
make clean && make
```

Run:
```
./mnistCUDNN
```

If all went well, the results should be:
```
Test passed!
```