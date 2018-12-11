# Ubuntu
- [General commands](#general)
- [Setup IP Address](#network)
- [Audio/Video Conversion](#av_conversion)

<a name="general"></a>
## General commands

- `dpkg --print-foreign-architectures` - Get foreign architecture
- `dpkg --print-architecture` - Get native architecture 
- `cat /var/lib/dpkg/arch` - Available architecture
- `sudo add-apt-repository ppa:graphics-drivers/ppa` - Add a PPA to pull new packages
- `sudo apt-add-repository --remove ppa:graphics-drivers/ppa` - Remove a PPA
- `apt-rdepends -r python3-pip` - Show recursive dependency listings of the package
- `cat /proc/cpuinfo  | egrep "(flags|model name|vendor)" | sort | uniq -c` - Get CPU flag detection
- `mount -o remount rw /` - Remount / path with read-write access

<a name="network"></a>
## Setup IP Address

- Enable eth0
```
udhcpc -i eth0
route add default gw xxx.xxx.xxx.xx eth0
```
- Show IP address assigned to eth0
```
ifconfig eth0 | grep 'inet addr:' | cut -d: -f2 | awk '{ print $1}'
```

<a name="av_conversion"></a>
## Audio/Video Conversion

- Handy commands to manipulate audio/video files.

### Convert MOV into MP4
```
avconv -i input.mov -codec copy output.mp4
```

### Change speed of the video
```
avconv -i input.mp4 -vf "setpts=speed_multiplier*PTS" output.mp4
```
- To slow down your video, you have to use a multiplier greater than 1.

```
avconv -i input.mp4 -vf "setpts=PTS/factor" output.mp4
```
- Speed up the video by using x factor.

### Extract audio track
- MP4 to mp3
```
avconv -i input.mp4 -vn -acodec copy audio.aac
```
```
avconv -i input.mp4 -vn -f mp3 output.mp3
for file in *.mp4; do avconv -i "${file%%.*}.mp4" -vn -f mp3 "${file%%.*}.mp3"; done
```

### Merge two MP4 files
```
ffmpeg -i concat:"input1.mp4|input2.mp4" output.mp4
```

### Mute the video
```
avconv -i input.mp4 -an output.mp4
```

### Rotate a video
```
avconv -i input.mp4 -vf transpose=1 output.mp4
```

## Reduce video size
```
avconv -i input.mp4 -c:v libx264 -crf 18 -c:a copy output.mp4
```
- CRF @ https://slhck.info/video/2017/02/24/crf-guide.html
- Play with it to see quality vs size

### Reduce audio file size
```
avconv -i old.mp3 -acodec libmp3lame -ac 2 -ab 64k -ar 44100 new-1.mp3
```

### Convert JPG to PNG
```
for file in *.jpg; do convert "${file%%.*}".jpg "${file%%.*}".png; done
```