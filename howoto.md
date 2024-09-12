# how to run a macos
## build images
```text
git clone https://github.com/sickcodes/Docker-OSX.git
cd Docker-OSX
docker build -t sickcodes/docker-osx:sonoma --build-arg SHORTNAME=sonoma .
```
## fine tune the ubuntu
1. 设置wsl参数
go to `C:/Users/<Your_Name>/.wslconfig`, 如果没有就新建, 修改内容为如下
```text
[wsl2]
nestedVirtualization=true
```
2. 检查kvm是否开启
输入`wsl`进入wsl后, 输入`kvm-ok`, 如果已经成功开启, 输出为
```text
INFO: /dev/kvm exists
KVM acceleration can be used
```
如果没有开启, 输入下面的命令进行安装
```text
sudo apt -y install bridge-utils cpu-checker libvirt-clients libvirt-daemon qemu qemu-kvm
```
3. 设置docker
在docker desktop中->`设置`->`Resources`->`WSL integration`, 开启`Enable integration with my default WSL distro`
4. 安装x11-apps
```text
sudo apt install x11-apps -y
```
5. wsl中执行
```text
sudo docker run -it --device /dev/kvm -p 50922:10022 -e "DISPLAY=${DISPLAY:-:0.0}" -v /mnt/wslg/.X11-unix:/tmp/.X11-unix -e GENERATE_UNIQUE=true -e CPU='Haswell-noTSX' -e CPUID_FLAGS='kvm=on,vendor=GenuineIntel,+invtsc,vmware-cpuid-freq=on' -e MASTER_PLIST_URL='https://raw.githubusercontent.com/sickcodes/osx-serial-generator/master/config-custom-sonoma.plist' sickcodes/docker-osx:sonoma
```

## 如何重启
```text
# find last container
docker ps -a

# docker start old container with -i for interactive, -a for attach STDIN/STDOUT
docker start -ai -i <Replace this with your ID>
```
