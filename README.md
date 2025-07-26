# M.Sc.Thesis:Low-Power Design techniques for FPGA-Based Under water Robotics Computing: A Power Gating Approach.

### Motivation:
Energy consumption is one of the main constraints in the current era of multicore heteroge-
neous systems. Energy Proportional Computing (EPC) has emerged as a solution to deliver
only the energy required by a given application.[3] Deep-sea computing is among the most
power-constrained applications compared to other domains. One of the most effective techniques
in EPC is power gating[4] of unused blocks in heterogeneous multicore system-on-chip (SoC)
architectures. To achieve high computational throughput, a neural processing unit—typically
implemented in the Programmable Logic (PL) fabric—is required as a hardware accelerator.
Power gating is particularly effective in this context, as idle PL units contribute significantly to
static power consumption. Modern Field-Programmable Gate Arrays (FPGAs) support partial
and dynamic reconfiguration, which enables integration of power gating strategies.[5]


### Specifications:

### Hardware
Kria KR260 SOM + Carrier card/board (Robotics starter kit).
* Chip: MPSoC zynq ultrascale+ based silicon device
* size: 60x77mm SOM 132x140mm Carrier card
* sys logic cells: 256K
* Block RAM blocks: 144
* UltraRAM blocks: 64
* DSP Slices 1.2K

### Framwork
* Jupyter
* python- PYNQ
* Vivado
* Vitis
* OpenCV

### Firmware
* Platform Management Unit Firmware- pmufw
* PMIC (I2C)

## *Sample 01: Object detection from a webcam with KR260, DPU on FPGA YOLOX and YOLOX interference. Probably we can achieve fast, real time detection*


* **Introduction:**
Normal speed of the webcam, 640x480 at 25 fps can be confirmed and real time speed of 17fps was achieved with YOLOX. 

* **Dependencies:**
 (enable live streaming from USB camera)
i.  GStreamer pipelines 
(this is for video stream encoding )

https://github.com/nickel110/libuvc.git

```
sudo apt update
sudo apt install cmake
sudo apt install libusb-1.0-0-dev

sudo su
git clone https://github.com/nickel110/libuvc.git
cd libuvc/
mkdir build
cd build/
cmake ..
make && sudo make install
GStreamer
```

We use Gstreamer pipelines to encode the video stream.

Refer to the official GStreamer documentation:

https://gstreamer.freedesktop.org/documentation/installing/on-linux.html?gi-language=c
```
sudo apt install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libgstreamer-plugins-bad1.0-dev gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-tools gstreamer1.0-x gstreamer1.0-alsa gstreamer1.0-gl gstreamer1.0-gtk3 gstreamer1.0-qt5 gstreamer1.0-pulseaudio
```

ii. libuvc (obtain vedio streams from USB camera)
    https://github.com/nickel110/libuvc.git
```
sudo apt update
sudo apt install cmake
sudo apt install libusb-1.0-0-dev

sudo su
git clone https://github.com/nickel110/libuvc.git
cd libuvc/
mkdir build
cd build/
cmake ..
make && sudo make install
```

iii.v4l2loopback-dkms
This lib will create virtual video devices
```
sudo apt install v4l2loopback-dkms
```

*********************
Since we are integrating GStreamer, a powerful multimedia framework used for vedio/audio pipelines, within the PYNQ environment. This setup enables hardware-accelerated vedio processing or streaming directly on PYNQ-enabled boards. Additionally we want a LED indication using GPIO pins through PYNQ.
**********************

Webcam: After connecting webcam, we need to verify the cam
```
ubuntu@kria:~$ sudo dmesg | grep usb
[    8.850427] input: UVC Camera (046d:0825) as /devices/platform/axi/ff9d0000.usb/fe200000.usb/xhci-hcd.1.auto/usb1/1-1/1-1.2/1-1.2:1.0/input/input1
```
Using v4l2-ctl, you can verify that it is recognized as video0.
```
root@kria:/home/ubuntu# v4l2-ctl --list-device
UVC Camera (046d:0825) (usb-xhci-hcd.1.auto-1.2):
        /dev/video0
        /dev/video1
        /dev/media0
```
Can also check which formats the camera supports.
```
root@kria:/home/ubuntu# v4l2-ctl -d /dev/video0 --list-formats-ext
ioctl: VIDIOC_ENUM_FMT
        Type: Video Capture

        [0]: 'YUYV' (YUYV 4:2:2)
                Size: Discrete 640x480
                        Interval: Discrete 0.033s (30.000 fps)
                        Interval: Discrete 0.040s (25.000 fps)
                        Interval: Discrete 0.050s (20.000 fps)
                        Interval: Discrete 0.067s (15.000 fps)
                        Interval: Discrete 0.100s (10.000 fps)
                        Interval: Discrete 0.200s (5.000 fps)
```
**********************
Test program for GStreamer with webcam:
Program Repo:

https://github.com/iotengineer22/AMD-Pervasive-AI-Developer-Contest/tree/main/src/usb-camera

Setting up camera 640x480 at 30 fps
```
# Definition of the GStreamer pipeline (software)
pipeline = "v4l2src device=/dev/video0 ! video/x-raw, width=640, height=480, framerate=30/1 ! videoconvert ! appsink"
```
1. test only with usb-camera-test.py
```
sudo su
cd /home/ubuntu/AMD-Pervasive-AI-Developer-Contest/src/usb-camera/
source /etc/profile.d/pynq_venv.sh
python3 usb-camera-test.py
```
Live streaming on KR260 was successful or not?
Check the FPS ?
*********************


********************
Live streaming OD with YOLOx and DPU


GPIO is controlled from the PL (FPGA) IP.
The DPU and FPGA files(DPU.bit, dpu.hwh, dpu.xclbin) are also saved in the same repository.
https://github.com/iotengineer22/AMD-Pervasive-AI-Developer-Contest/tree/main/src/usb-camera

****************************

Now we are going to use KR260's DPU and YOLOX (app_gst_yolox_real_normal_camera_gpio.py)
this time when a yellow ball is detected, the LED(GPIO) is turned on.
```
python3 app_gst-yolox-real-normal-camera-gpio.py
```

So now DPU and GPIO integration:
1. Since it is the same board so we might try first upload the .bit .hwh .xclbin file directly from reporsitaroy.
2. After that I will design it by myself.
the tutorial will be like: https://www.hackster.io/iotengineer22/implementation-dpu-gpio-and-pwm-for-kr260-f7637b

*****************************
