# Ubuntu
- [General commands](#general)
- [Loop devices](#loop_devices)
- [Find commands](#find)
- [Network](#network)
- [Audio/Video conversion](#av_conversion)

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
- `echo 'USER_NAME ALL=(ALL:ALL) ALL' | sudo EDITOR='tee -a' visudo` - Add a user to visudo/suderos
- `echo 'USER_NAME ALL=NOPASSWD: ALL' | sudo EDITOR='tee -a' visudo` - Add password free user
- `id -g USER_NAME` - Get primary group id for a user
- `id -gn USER_NAME` - Get primary group name for a user
- `usermod -g GROUP_NAME USER_NAME` - Change group for a user
- `sudo find /path/to/search -group GROUP_NAME -d type d` - Find out folders belong to a specific group
- Find and kill a process by name
```
PID=$(ps -ef | grep <process_name> awk '{print $2}')
kill -9 $PID
```

- Sdcard backup & restore
```
sudo dd bs=4M if=/dev/mmcblk0 | gzip > ~/Desktop/sdcard_backup.gz
sudo gzip -dc ~/Desktop/sdcard_backup.gz | dd bs=4M of=/dev/mmcblk0
```

<a name="loop_devices"></a>
## Loop devices

- Create a disk for FAT type
```
dd if=/dev/zero of=fat.bin bs=1M count=6
mkfs.vfat ramdisk.bin
mkdir fat
sudo mount -o loop fat.bin fat/
sudo cp <list of files> fat/
sudo umount fat
```

- Mount ext4 dd image
```
dd if=/dev/zero of=ext4.bin bs=1M count=50
losetup /dev/loop0 ext4.bin
mkfs.ext4 /dev/loop0

mkdir -p ext4
mount /dev/loop0 ext4
```

<a name="find"></a>
## Find commands

- Find a file of specific name
```
find . -type f -name "postgis"
```

- Get rid of the Clock skew detected. Your build may be incompleted.Put timestamps on all files equal to current time
```
find . -exec touch {} \;
```

- Change format of the files
```
for i in `find . -name "*.cc"`; do dos2unix $i; done
for i in `find . -name "*.cc"`; do unix2dos $i; done
```

- Find file(s) with a specific name (with loop and custom command)
```
for i in `find . -name ".mk"`; do echo $i; done
for i in `find . -name ".mk"`; do git add -f $i; done
```

- File file(s) of specific extension (Show full path path of the found file)
```
for i in `find . -name "*.cpp"`; do echo $i; done
```

- File file(s) of specific extension (Show only file name)
```
for i in `find . -name "*.cpp"`; do echo $(basename $i); done
```
NOTE: `basename` is part of the POSIX spec so hoping it is supported by underlying OS otherwise
```
for i in `find . -name "*.cpp"`; do echo ${i##*/}; done
```
- File file(s) of specific extension (Show only dir name)
```
for i in `find . -name "*.cpp"`; do echo $(dirname $i); done
```
NOTE: `dirname` is part of the POSIX spec so hoping it is supported by underlying OS otherwise
```
for i in `find . -name "*.cpp"`; do echo ${i%/*}; done
```

- File file(s) of specific extension (Show file with full path but without .ext)
```
for i in `find . -name "*.cpp"`; do echo ${i%.cpp}; done
```

- Rename files of specific extensions
```
for i in `find . -name "*.cpp"`; do mv $i ${i%.cpp}.cc; done
```

- Find all files
```
find /usr/share/ -type f -printf "%f\n" | sort > all.txt
find /usr/share/ -type f -printf "%f\n" | sort | uniq > uniq.txt
```

- Find all sym links in a directory
```
find . -type l -ls
```

- Delete empty folders
```
find /path/to/folder -depth -type d -empty -delete
```

<a name="network"></a>
## Network

- Enable eth0
```
udhcpc -i eth0
route add default gw xxx.xxx.xxx.xx eth0
```
- OR -
```
netcfg eth0 up
netcfg eth0 dhcp
```

- Show IP address assigned to eth0
```
ifconfig eth0 | grep 'inet addr:' | cut -d: -f2 | awk '{ print $1}'
```

- Network sockets usage
```
netstat -ltnp | grep -w ':80'
lsof -i :80
```
```
netstat -ano -p tcp
sudo netstat -ap | grep 5000
```

<a name="av_conversion"></a>
## Audio/Video conversion

- Handy commands to manipulate audio/video files.

### Convert MOV into MP4
```
ffmpeg -i input.mov -codec copy output.mp4
```

### Change speed of the video
```
ffmpeg -i input.mp4 -vf "setpts=speed_multiplier*PTS" output.mp4
```
- To slow down your video, you have to use a multiplier greater than 1.

```
ffmpeg -i input.mp4 -vf "setpts=PTS/factor" output.mp4
```
- Speed up the video by using x factor.

### Extract audio track
- MP4 to mp3
```
ffmpeg -i input.mp4 -vn -acodec copy audio.aac
```
```
ffmpeg -i input.mp4 -vn -f mp3 output.mp3
for file in *.mp4; do ffmpeg -i "${file%%.*}.mp4" -vn -f mp3 "${file%%.*}.mp3"; done
```

### Merge two MP4 files
```
ffmpeg -i concat:"input1.mp4|input2.mp4" output.mp4
```

### Mute the video
```
ffmpeg -i input.mp4 -an output.mp4
```

### Rotate a video
```
ffmpeg -i input.mp4 -vf transpose=1 output.mp4
```

## Reduce video size
```
ffmpeg -i input.mp4 -c:v libx264 -crf 18 -c:a copy output.mp4
```
- CRF @ https://slhck.info/video/2017/02/24/crf-guide.html
- Play with it to see quality vs size

### Reduce audio file size
```
ffmpeg -i old.mp3 -acodec libmp3lame -ac 2 -ab 64k -ar 44100 new-1.mp3
```

### Convert JPG to PNG
```
for file in *.jpg; do convert "${file%%.*}".jpg "${file%%.*}".png; done
```

### Convert video to PNG
```
ffmpeg -i input.mp4 output_%02d.png
```
- `-r 1.0` - Pass to above command to capture frame after 1 seconds instead of all.

### Convert PNG to RGB565
```
ffmpeg -vcodec png -i test1.png -vcodec rawvideo -f rawvideo -pix_fmt rgb565 test1.data
```
