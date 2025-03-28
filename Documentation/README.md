**Nvidia Jetpack Version 4.6.1, Python Version 3.6**

**Connecting Jetson Nano to Ubuntu**

1. Install Screen to Bash
2. Connect Jetson to your laptop using MicroUSB cable
3. sudo screen /dev/ttyACM0 or screen /dev/ttyACM1

**Connecting to Network**

1. Show Network connection status using the command: ```nmcli -a```
1. Identify WiFi networks using the command: ```nmcli dev wifi list```
2. Enter the command, ```sudo nmcli dev wifi connect SSID password "PASSWORD"```, in Jetson, replace SSID with Network name and "PASSWORD" with the Network password

**More on Networking**
You cannot use static IP adress for all wifi addresses, because the wlan0 uses ipv4 for each connection  
Conclusion: You have to look up the IP of your Jetson everytime you connect to a new network. However you can use hotspot from you PC and configure it to connect to your hotspot.  
You cannot share internet from Linux OS becaue it's wireless driver does not support multiple tasks, i.e. you either connect to internet or provide internet from wifi adapter.  
You can however share wifi wirelessly from windows just by turning on the hotspot. [And Set up Static IP](https://forums.developer.nvidia.com/t/jetson-nano-faq/82953#p-410022-q-how-to-set-static-ip-on-jetson-nano-29)


**Installation of TensorRT**

TensorRT is required to run ONNX models on Jetson Nano.
Run the following commands in the terminal to install it.

`$ sudo apt update`

`$ sudo apt install nvidia-tensorrt`

**Connecting using SSH**  
Connect to Jetson using screen command and get it's IP address using ifconfig command.  
IP address is the **inet** code under 'wlan0'  
Connect your laptop to same network as Jetson's and run the command 
`ssh rakshak@<ip address>` 
in your terminal.

**Jetson Resources**
Jetson Support: https://developer.nvidia.com/embedded/community/support-resources  
Jetson Nano wiki: https://elinux.org/Jetson_Nano  
Script to get MAC Address of Jetson Nano: https://gist.github.com/dusty-nv/e4314241677cf38f40d556931d0c4a38  
Jetson Easy setup configurator: https://github.com/rbonghi/jetson_easy  
jetson-stats is a powerful tool to analyze your board: https://github.com/rbonghi/jetson_stats  

**Projects**
https://developer.nvidia.com/embedded/community/jetson-projects/hello_ai_world

**Screen Cast on Windows**
https://forums.developer.nvidia.com/t/jetson-nano-faq/82953  
Q: Is there any example of running RTSP streaming?  
There is an ease-to-use gstreamer sample to run RTSP server. The steps:  

Download test-launch.c  
[gst-rtsp-server/examples/test-launch.c at 1.14.5 · GStreamer/gst-rtsp-server · GitHub](https://github.com/GStreamer/gst-rtsp-server/blob/1.14.5/examples/test-launch.c)

Build the sample

```
sudo apt-get install libgstrtspserver-1.0 libgstreamer1.0-dev
gcc test-launch.c -o test-launch $(pkg-config --cflags --libs gstreamer-1.0 gstreamer-rtsp-server-1.0)
```
Launch the server
```
$ ./test-launch "videotestsrc is-live=1 ! nvvidconv ! nvv4l2h264enc ! h264parse ! rtph264pay name=pay0 pt=96"
```
At client side, if the device is a PC with Windows OS, you can open network stream rtsp://<SERVER_IP_ADDRESS>:8554/test via VLC. If it is a Jetson device, you can run the command:
```
$ gst-launch-1.0 uridecodebin uri=rtsp://<SERVER_IP_ADDRESS>:8554/test ! nvoverlaysink
```
