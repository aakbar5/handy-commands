# Ubuntu
- [General commands](#general)
- [Loop devices](#loop_devices)
- [Find commands](#find)
- [DD commands](#dd)
- [Redirection](#redirection)
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
sudo losetup -f   # Find out next free loop

sudo dd if=/dev/zero of=ext4.bin bs=1M count=50
sudo losetup /dev/loop0 ext4.bin
sudo mkfs.ext4 /dev/loop0

mkdir -p ext4
sudo mount -t ext4 /dev/loop0 ext4
```

- Shrinking disk image (using gparted)
```
sudo losetup -f                 # Find out next free loop
sudo losetup /dev/loop<0> diskimage.img
sudo partprobe /dev/loop<0>     # Get available partitions
sudo gparted /dev/loop<0>       # Use gparted to manipulate partition
sudo losetup -d /dev/loop<0>    # Unload device
```

- Shrinking disk image (using gparted)
```
fdisk -lu diskimage.img         # Get details of partitions
```

```
Disk diskimage.img: 4096 MB, 4096000000 bytes, 8000000 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x000a1bc7

      Device Boot      Start         End      Blocks   Id  System
diskimage.img           2048     5872026     5869978    b  W95 FAT32
```
Make note of the number shown under `End`, now use it in following command:
```
truncate --size=$[(5872026+1)*512] diskimage.img
```

<a name="dd"></a>
## DD commands

- Dump /dev/sda to /dev/sdb
```
dd if=/dev/sda of=/dev/sdb
dd if=/dev/sda of=/dev/sdb bs=64k conv=noerror,sync
```

- Dump disk to a compressed image
```
dd if=/dev/sda bs=1k | gzip > disk.gzip
gzip -d -c disk.gzip | dd of=/dev/sda bs=1k
```

- Create a 5MB disk image
```
dd if=/dev/zero of=disk.img bs=1k count=5MB
```

- Wipe a disk by writing zeros to it
```
dd if=/dev/zero of=/dev/sda bs=16M
```

- Write random numbers to disk
```
dd if=/dev/urandom of=/dev/sda bs=16M
```

- Send random data to framebuffer device
```
cat /dev/urandom > /dev/fb0
dd if=/dev/urandom of=/dev/fb0 bs=1024 count=8100
```

<a name="find"></a>
## Find commands

- Find a file of specific name
```
find . -type f -name "postgis"
```

- Get rid of the Clock skew detected. Your build may be in-complete.
  Put timestamps on all files equal to current time
```
find . -exec touch {} \;
```

- Change format of the files
```
for i in `find . -name "*.cc" or -name "*.hpp`; do dos2unix $i; done
for i in `find . -name "*.cc" or -name "*.hpp`; do unix2dos $i; done
```
- `or` -- Use to OR multiple `-name` flags.

- Find file(s) with a specific name (with loop and custom command)
```
for i in `find . -name ".mk"`; do echo $i; done
for i in `find . -name ".mk"`; do git add -f $i; done
```

- File file(s) of specific extension (Show full path path of the found file)
```
for i in `find . -name "*.cc"`; do echo $i; done
```

- File file(s) of specific extension (Show only file name)
```
for i in `find . -name "*.cc"`; do echo $(basename $i); done
```
NOTE: `basename` is part of the POSIX spec so hoping it is supported by underlying OS otherwise
```
for i in `find . -name "*.cc"`; do echo ${i##*/}; done
```
- File file(s) of specific extension (Show only dir name)
```
for i in `find . -name "*.cc"`; do echo $(dirname $i); done
```
NOTE: `dirname` is part of the POSIX spec so hoping it is supported by underlying OS otherwise
```
for i in `find . -name "*.cc"`; do echo ${i%/*}; done
```

- File file(s) of specific extension (Show file with full path but without .ext)
```
for i in `find . -name "*.cc"`; do echo ${i%.cc}; done
```

- Rename files of specific extensions
```
for i in `find . -name "*.cc"`; do mv $i ${i%.cc}.cc; done
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

<a name="redirection"></a>
## Redirection

- `1` denotes `stdout`; `2` denotes `stderr`.
- `>` - Redirect output of the command onto the right side (Overwrite the contents of output.txt)
- `>>` - Redirect output of the command onto the right side (Append to the contents of output.txt)
- `command  > output.txt` - Redirect standard output stream to output.txt
- `command 2> output.txt` - Redirect error stream to output.txt
- `command &> output.txt` - Redirect both (standard & error) streams to output.txt
- `command > output.txt 2>&1` - Redirect standard output to output.txt and also redirect error stream to standard output stream
- `command | tee output.txt` - Copy standard output stream to output.txt
- `command | tee -a output.txt` - Copy & append standard output stream to output.txt
- `command |& tee output.txt` - Copy both (standard & error) streams to output.txt

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
- OR -
```
ifconfig eth0 up
ifconfig eth0 down
dhclient eth0

auto eth0
iface eth0 inet dhcp
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

### Extract audio track
- audio track as aac
```
ffmpeg -i input.mp4 -vn -acodec copy audio.aac
```
- audio track as mp3
```
ffmpeg -i input.mp4 -vn -f mp3 output.mp3
```

### audio track + picture = video
- Convert mp3 file to audio with a picture
```
ffmpeg -loop 1 -i picture.jpg -i input_audio.mp3 -shortest -c:v libx264 -tune stillimage -c:a copy video.mp4
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
- Change quality of the video
```
ffmpeg -i input.mp4 -c:v libx264 -crf 18 -c:a copy output.mp4
```

+ CRF @ https://slhck.info/video/2017/02/24/crf-guide.html
+ Play with it to see quality vs size


- Change the resolution & bit rate
```
ffmpeg -i input.mp4 -s 320x240 -b:v 512k -vcodec mpeg1video -acodec copy output.mp4
```

- Change the resolution
```
ffmpeg -i input.mp4 -vf "scale=iw/2:ih/2" output.mp4
```

- Clip the video
```
ffmpeg -i input.mp4 -c copy -ss 00:03:22.0 -to 00:08:49.0 output.mp4
```

## Embed subtitles
```
ffmpeg -i input.mp4 -vf "subtitles=lyrics.srt:force_style='FontName=DejaVu Serif,FontSize=24'" output.mp4
```

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

## Convert RAW data image format to PNG
```
ffmpeg -f rawvideo -pixel_format rgba -video_size 1920x1080 -i input.data test.png
```

## Convert PNGs to video
```
ffmpeg -framerate 1/5 -c:v libx264 -r 30 -pix_fmt yuv420p -i *%03d.png output.mp4
```

## Darken the images
- B & W image
```
for i in `find . -name "*.jpg"`; do convert $i -normalize -threshold 80% $i; done
```

- Enhance color image
```
for i in `find . -name "*.jpg"`; do convert $i -channel RGB -contrast-stretch 1x1% $i; done
```
https://legacy.imagemagick.org/Usage/color_mods/