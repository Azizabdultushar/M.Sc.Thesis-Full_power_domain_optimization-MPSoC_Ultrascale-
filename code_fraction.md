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


sudo apt install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libgstreamer-plugins-bad1.0-dev gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-tools gstreamer1.0-x gstreamer1.0-alsa gstreamer1.0-gl gstreamer1.0-gtk3 gstreamer1.0-qt5 gstreamer1.0-pulseaudio

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

sudo apt install v4l2loopback-dkms


sudo dmesg | grep usb

root@kria:/home/ubuntu# v4l2-ctl --list-device

root@kria:/home/ubuntu# v4l2-ctl -d /dev/video0 --list-formats-ext

# Definition of the GStreamer pipeline (software)
pipeline = "v4l2src device=/dev/video0 ! video/x-raw, width=640, height=480, framerate=30/1 ! videoconvert ! appsink"

test only with usb-camera-test.py

sudo su
cd /home/ubuntu/AMD-Pervasive-AI-Developer-Contest/src/usb-camera/
source /etc/profile.d/pynq_venv.sh
python3 usb-camera-test.py

python3 app_gst-yolox-real-normal-camera-gpio.py

```
