# LRV
Scripts for HD video, navigation and collision avoidance

# vid.cfg - Configuration items for streaming video
#
PC_Address="IP address of video PC that drives the monitor"
#
WIDTH="1280"
HEIGHT="720"
BITRATE="2000000"
FPS="30"
UDP_Port="5700"
#
#
#!/bin/bash
# vid.sh
source /home/pi/vid.cfg
raspivid -t 0 -w $WIDTH -h $HEIGHT -b $BITRATE -fps $FPS -o - | \
        gst-launch-1.0 --gst-debug-level=0 -v \
        fdsrc ! \
        h264parse ! \
        rtph264pay config-interval=10 pt=96 ! \
        udpsink host=$PC_Address port=$UDP_Port
#
#
# ./video   script in ubuntu linux
# gst-launch-1.0 udpsrc port=5700 ! application/x-rtp,encoding-name=H264,payload=96 ! rtph264depay ! avdec_h264 ! videoconvert ! autovideosink
#

