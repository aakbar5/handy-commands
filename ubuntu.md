# Ubuntu
<!-- TOC -->

- [Ubuntu](#ubuntu)
    - [General commands](#general-commands)
    - [Loop devices](#loop-devices)
    - [rsync commands](#rsync-commands)
    - [dd commands](#dd-commands)
    - [cURL commands](#curl-commands)
    - [Find commands](#find-commands)
    - [Redirection](#redirection)
    - [Network](#network)
    - [Pandoc](#pandoc)
    - [VSCode](#vscode)
    - [PDF Manipulation](#pdf-manipulation)
    - [Audio Manipulation](#audio-manipulation)
    - [Video Manipulation](#video-manipulation)
    - [Trim a video file](#trim-a-video-file)
    - [Image Manipulation](#image-manipulation)

<!-- /TOC -->ib/dpkg/arch` - Available architecture
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
```bash
PID=$(ps -ef | grep <process_name> awk '{print $2}')
kill -9 $PID
```
- `sudo fc-cache -f -v` - Update font cache
- `gsettings list-recursively` - List settings
- Auto-mount sdcard/usb
```bash
gsettings set org.gnome.desktop.media-handling automount true
gsettings set org.gnome.desktop.media-handling automount false
```

- Remove a package from ubuntu
```bash
dpkg --list # Find package name
sudo apt remove <package.name> # Remove package
```

- Find broken link in a directory
```bash
find . -type l ! -exec test -e {} \; -print
```

<a name="loop_devices"></a>
## Loop devices

- Create a disk for FAT type
```bash
dd if=/dev/zero of=fat.bin bs=1M count=6
mkfs.vfat ramdisk.bin
mkdir fat
sudo mount -o loop fat.bin fat/
sudo cp <list of files> fat/
sudo umount fat
```

- Mount ext4 dd image
```bash
sudo losetup -f   # Find out next free loop

sudo dd if=/dev/zero of=ext4.bin bs=1M count=50
sudo losetup /dev/loop0 ext4.bin
sudo mkfs.ext4 /dev/loop0

mkdir -p ext4
sudo mount -t ext4 /dev/loop0 ext4
```

- Shrinking disk image (using gparted)
```bash
sudo losetup -f                 # Find out next free loop
sudo losetup /dev/loop<0> diskimage.img
sudo partprobe /dev/loop<0>     # Get available partitions
sudo gparted /dev/loop<0>       # Use gparted to manipulate partition
sudo losetup -d /dev/loop<0>    # Unload device
```

- Shrinking disk image (using gparted)
```bash
fdisk -lu diskimage.img         # Get details of partitions
```

```bash
Disk diskimage.img: 4096 MB, 4096000000 bytes, 8000000 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x000a1bc7

      Device Boot      Start         End      Blocks   Id  System
diskimage.img           2048     5872026     5869978    b  W95 FAT32
```
Make note of the number shown under `End`, now use it in following command:
```bash
truncate --size=$[(5872026+1)*512] diskimage.img
```

<a name="rsync"></a>
## rsync commands

- `rsync -av --exclude '*.jpg' --exclude '*.mp4' src/ dst/` - Copy the contents of `src` into `dst`
- `rsync -av --exclude '*.jpg' --exclude '*.mp4' src dst/` - Copy `src` into `dst`

    - `/` to the destination does not matter, command will create it if `dst` folder does not exist

- `rsync -av --remove-source-files --exclude '*.jpg' --exclude '*.mp4' src dst` - Copy `src` into `dst` and remove copied files from the source

<a name="dd"></a>
## dd commands

- Dump /dev/sda to /dev/sdb
```bash
dd if=/dev/sda of=/dev/sdb
dd if=/dev/sda of=/dev/sdb bs=64k conv=noerror,sync
```

- Dump disk to a compressed image
```
dd if=/dev/sda bs=1k | gzip > disk.gzip
gzip -d -c disk.gzip | dd of=/dev/sda bs=1k
```

- Create a 5MB disk image
```bash
dd if=/dev/zero of=disk.img bs=1k count=5MB
```

- Wipe a disk by writing zeros to it
```bash
dd if=/dev/zero of=/dev/sda bs=16M
```

- Write random numbers to disk
```bash
dd if=/dev/urandom of=/dev/sda bs=16M
```

- Send random data to framebuffer device
```bash
cat /dev/urandom > /dev/fb0
dd if=/dev/urandom of=/dev/fb0 bs=1024 count=8100
```

<a name="curl"></a>
## cURL commands

- `curl -i -X POST http://localhost:5500/link -F 'image=@image.jpg'` - Upload a file

- `curl -i -X POST http://localhost:5500/link -H "Content-Type: application/json" --data '{"key":"value"}'` - Send json command

- `curl --data "param1=value1&param2=value2" http://localhost:5500/link` - Send data

<a name="find"></a>
## Find commands

- Find a file of specific name
```bash
find . -type f -name "postgis"
```

- Get rid of the Clock skew detected. Your build may be in-complete.
  Put timestamps on all files equal to current time
```bash
find . -exec touch {} \;
```

- Change format of the files
```bash
for i in `find . -name "*.cc" or -name "*.hpp`; do dos2unix $i; done
for i in `find . -name "*.cc" or -name "*.hpp`; do unix2dos $i; done
```
- `or` -- Use to OR multiple `-name` flags.

- Find file(s) with a specific name (with loop and custom command)
```bash
for i in `find . -name ".mk"`; do echo $i; done
for i in `find . -name ".mk"`; do git add -f $i; done
```

- File file(s) of specific extension (Show full path path of the found file)
```bash
for i in `find . -name "*.cc"`; do echo $i; done
```

- File file(s) of specific extension (Show only file name)
```bash
for i in `find . -name "*.cc"`; do echo $(basename $i); done
```
NOTE: `basename` is part of the POSIX spec so hoping it is supported by underlying OS otherwise
```bash
for i in `find . -name "*.cc"`; do echo ${i##*/}; done
```

- File file(s) of specific extension (Show only dir name)
```bash
for i in `find . -name "*.cc"`; do echo $(dirname $i); done
```
NOTE: `dirname` is part of the POSIX spec so hoping it is supported by underlying OS otherwise
```bash
for i in `find . -name "*.cc"`; do echo ${i%/*}; done
```

- File file(s) of specific extension (Show file with full path but without .ext)
```bash
for i in `find . -name "*.cc"`; do echo ${i%.cc}; done
```

- Rename files of specific extensions
```bash
for i in `find . -name "*.cc"`; do mv $i ${i%.cc}.cc; done
```

- Find all files
```bash
find /usr/share/ -type f -printf "%f\n" | sort > all.txt
find /usr/share/ -type f -printf "%f\n" | sort | uniq > uniq.txt
```

- Find all sym links in a directory
```bash
find . -type l -ls
```

- Find and delete empty folders
```bash
find /path/to/folder -depth -type d -empty -delete
```

- Find and delete specific format file
```bash
find . ! -name '*.pdf' -type f -exec rm -f {} +
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
```bash
udhcpc -i eth0
route add default gw xxx.xxx.xxx.xx eth0
```
- OR -
```bash
netcfg eth0 up
netcfg eth0 dhcp
```
- OR -
```bash
ifconfig eth0 up
ifconfig eth0 down
dhclient eth0

auto eth0
iface eth0 inet dhcp
```

- Show IP address assigned to eth0
```bash
ifconfig eth0 | grep 'inet addr:' | cut -d: -f2 | awk '{ print $1}'
```

- Show network connection
```bash
route | grep -m1 ^default | awk '{print $NF}'
```

- Network sockets usage
```bash
netstat -ltnp | grep -w ':80'
lsof -i :80
```

```bash
netstat -ano -p tcp
sudo netstat -ap | grep 5000
```

<a name="pandoc"></a>
## Pandoc

- Convert word document to markup (Images in the document will be saved to folder # `media`)
```bash
pandoc --extract-media ./media -f docx -t markdown word.docx -o markup.md
```

<a name="vscode"></a>
## VSCode

- Export installed extensions and install back
```bash
code --list-extensions > extensions.list
cat vscode-extensions.list | xargs -L 1 code --install-extension
```

<a name="pdf_manipulation"></a>
## PDF Manipulation

- Remove a page from the pdf
```bash
gs -sDEVICE=pdfwrite -dNOPAUSE -dBATCH -dSAFER \
  -sPageList=1-3,6- \
  -sOutputFile=out.pdf in.pdf
```

- Split the range of pages from the pdf
```bash
gs -sDEVICE=pdfwrite -dNOPAUSE -dBATCH -dSAFER \
   -dFirstPage=22393 -dLastPage=23052 \
   -sOutputFile=input.pdf \
   output.pdf
```

- Search text in pdf
```bash
find docs/ '*.pdf' -exec sh -c 'pdftotext "{}" - | grep --with-filename --label="{}" --color "SwapDrawableWithDamage"' \;
pdftotext my.pdf - | grep 'pattern'
```

- Reduce pdf file size
```bash
convert -density 200x200 -quality 60 -compress jpeg input.pdf output.pdf
```
https://askubuntu.com/a/469255

- Convert png to PDFs
```bash
convert img1.png img2.png              -compress jpeg -quality 50 output.pdf
convert img1.png img2.png sample-*.png -compress jpeg -quality 50 output.pdf
```

<a name="audio_manipulation"></a>
## Audio Manipulation

- Handy commands to manipulate audio files.

### Swap audio channels
https://trac.ffmpeg.org/wiki/AudioChannelManipulation

### Extract audio track

- Audio track as aac
```bash
ffmpeg -i input.mp4 -vn -acodec copy audio.aac
```

- Audio track as mp3
```bash
ffmpeg -i input.mp4 -vn -f mp3 output.mp3
```

### Audio track + picture = video
- Convert mp3 file to audio with a picture
```bash
ffmpeg -loop 1 -i picture.jpg -i input_audio.mp3 -shortest -c:v libx264 -tune stillimage -c:a copy video.mp4
```

### Change audio `sample_fmt`
```bash
for i in `find . -name "*.wav"`; do echo $(basename $i); ffmpeg -i $i -sample_fmt s16 modify_$(basename $i); done
```

### Duplicate left channel
```bash
for i in `find . -name "*.wav"`; do echo $(basename $i); ffmpeg -i $i -sample_fmt s16 -filter_complex "channelmap=map=FL-FL|FL-FR:channel_layout=stereo" modify_$(basename $i); done
```

### Duplicate right channel
```bash
for i in `find . -name "*.wav"`; do echo $(basename $i); ffmpeg -i $i -sample_fmt s16 -filter_complex "channelmap=map=FR-FL|FR-FR:channel_layout=stereo" modify_$(basename $i); done
```
- https://trac.ffmpeg.org/wiki/AudioChannelManipulation


### wav files into c
```bash
for i in `find . -name "*.wav"`; do echo $(basename $i); xxd -i $(basename $i) > $(basename $i).c; done
```

### Reduce audio file size
```bash
ffmpeg -i old.mp3 -acodec libmp3lame -ac 2 -ab 64k -ar 44100 new-1.mp3
```

<a name="video_manipulation"></a>
## Video Manipulation

### Extract MP4 info
```bash
ffprobe -v quiet -print_format json -show_format -show_streams input.mp4

ffprobe -i video.mp4 -show_frames 2>&1|grep -c '^\[FRAME'
ffprobe -i video.mp4 -show_frames 2>&1 | grep -c media_type=video
ffprobe -i h264cars_small.mp4 -show_frames 2>&1 | grep -c media_type=video
ffprobe -select_streams v -show_streams video.mp4
```

## Trim a video file
```
ffmpeg -ss 00:00:00 -to 00:00:10  -i video.mp4 -c copy output.mp4
```

### Convert MOV into MP4
- MOV into MP4: `ffmpeg -i input.mov -codec copy output.mp4`
- Convert codec from HEVC to H264: `ffmpeg -i input.mp4 -c:v libx264 output.mp4`

### Change speed of the video
```bash
ffmpeg -i input.mp4 -vf "setpts=speed_multiplier*PTS" output.mp4
```
- To slow down your video, you have to use a multiplier greater than 1.

```bash
ffmpeg -i input.mp4 -vf "setpts=PTS/factor" output.mp4
```

### Merge two MP4 files
```bash
ffmpeg -i concat:"input1.mp4|input2.mp4" output.mp4
```

### Mute the video
```bash
ffmpeg -i input.mp4 -an output.mp4
```

### Rotate a video
```bash
ffmpeg -i input.mp4 -vf transpose=1 output.mp4
```

### Reduce video size
- Change quality of the video
```
ffmpeg -i input.mp4 -c:v libx264 -crf 18 -c:a copy output.mp4
```

+ CRF @ https://slhck.info/video/2017/02/24/crf-guide.html
+ Play with it to see quality vs size

- Change fps of the video
```bash
ffmpeg -y -r 30 -i input.mp4 output.mp4
```

- Change the resolution & bit rate
```bash
ffmpeg -i input.mp4 -s 320x240 -b:v 512k -vcodec mpeg1video -acodec copy output.mp4
```

- Change the resolution
```bash
ffmpeg -i input.mp4 -vf "scale=iw/2:ih/2" output.mp4
```

- Clip the video
```bash
ffmpeg -i input.mp4 -c copy -ss 00:03:22.0 -to 00:08:49.0 output.mp4
```

### Embed subtitles
```bash
ffmpeg -i input.mp4 -vf "subtitles=lyrics.srt:force_style='FontName=DejaVu Serif,FontSize=24'" output.mp4
```

<a name="image_manipulation"></a>
## Image Manipulation

### Convert JPG to PNG
```bash
for file in *.jpg; do convert "${file%%.*}".jpg "${file%%.*}".png; done
```

### Convert PNG to RGB565
```bash
ffmpeg -vcodec png -i test1.png -vcodec rawvideo -f rawvideo -pix_fmt rgb565 test1.data
```

### Convert RAW data image format to PNG
```bash
ffmpeg -f rawvideo -pixel_format rgba -video_size 1920x1080 -i input.data test.png
```

### Convert Planner RGB data image to PNG
```bash
convert -depth 8 -interlace plane -size 100x100 rgb:input.data output.png
```

NOTE: ffmpeg got a complicated command.

### Convert PNG image to YUV format
```bash
ffmpeg -i test.png -pix_fmt nv12 test.yuv
```

### Convert video to PNG
```bash
ffmpeg -i input.mp4 output_%02d.png
```
- `-r 1.0` - in above command will ask to capture frame after 1 seconds instead of all.

- Output one image every 1 seconds: `ffmpeg -i input.mp4 -vf fps=1 out%d.png`
- Output one image every minute: `ffmpeg -i test.mp4 -vf fps=1/60 thumb%04d.png`
- Output one image every 10 minutes: `ffmpeg -i test.mp4 -vf fps=1/600 thumb%04d.png`

### Convert PNGs to video

- Single image to video of 1 minute length and specific video format
```bash
ffmpeg -loop 1 -i test.png -vf format=yuv420p -r 60 -t 60 output.mp4
```

- Multiple images to a video
```bash
ffmpeg -framerate 60 -i image-%02d.png -vf format=yuv420poutput.mp4
```
- Input png are named: `input_01.png` `input_02.png` ... `input_10.png`
- `output.mp4` will have frame rate of 25fps
- Each image will be shown for 1/25

```bash
ffmpeg -framerate 1/5 -i image-%02d.png -vf format=yuv420p output.mp4
ffmpeg -framerate 1/5 -i input_%02d.png -vf format=yuv420p -r 60 test.mp4
```
- Show each input image for at least 5 seconds

### Darken the images
- B & W image
```bash
for i in `find . -name "*.jpg"`; do convert $i -normalize -threshold 80% $i; done
```

NOTE: If file name is having spaces, `$i` will not have full file name. To avoid this
```bash
IFS=$'\n'; for i in `find . -name "*.jpg"`; do echo "$i"; done
```

- Enhance color image
```bash
for i in `find . -name "*.jpg"`; do convert $i -channel RGB -contrast-stretch 1x1% $i; done
```
https://legacy.imagemagick.org/Usage/color_mods/

### PNGs to GIF
```bash
convert -delay 25 -loop 0 *.png test.gif
```
