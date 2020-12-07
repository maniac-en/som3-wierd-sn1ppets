# som3-wierd-sn1ppets

## Note
- Some of these snippets are gonna very specific use-cases, so please mind the rant!
- If you have any suggestions, please let me know! I'll add them `probably`
- If you have a use-case but you are still facing issues, you can DM me on [twitter](https://twitter.com/maniac_en)
- We are not responsible for kernel crashes, dead OSs, thermonuclear war, or you getting fired because the alarm app failed. Some scripts run on root level, give them a good read before using. 
- Contributors:  
  - Maniac_en ([profile](https://github.com/maniac-en)) 
  - MrHolmes ([profile](https://github.com/holmes-py)) 
## v0.2

## Index
1. [Transferring big file](#1-transferring-big-file)
2. [Downloading all busybox binaries](#2-downloading-all-busybox-binaries)
3. [Switching CPU governor](#3-switching-CPU-Governor)

#### 1. Transferring big file
  ##### when to use this?
  - You have a shell (not ssh) access to a server with no internet access
  - server hosts a file upload form but you can't upload a big_file to the server machine due to file size restrictions

  ##### snippet
- Method: 1 (using `netcat`)
    - On host machine:
    ```sh
        netcat -lvnp 80 < big_file
    ```
   - On server machine:
    ```sh
        cat > /tmp/big_file < /dev/tcp/<HOST_IP>/80
    ```
- Method: 2 (Using `split`)
    - On host machine:
    ```sh
    split -b 512K big_file # whatever max size is allowed by server
    ```
    - On server machine:
    ```sh
    # upload your generated files manually
    cat /tmp/xa* > /tmp/big_file
    ```
   
#### 2. Downloading all busybox binaries
  ##### when to use this?
  - you might use it only once, when you wanna download all [busybox binaries](https://www.busybox.net/)
  ##### snippet
  ```sh
  sudo apt install -y rename; mkdir bins && cd bins; \
    wget -r -e robots=off -nH --cut-dirs=3 --no-parent --accept-regex='busybox_' -R html,tmp,txt https://busybox.net/downloads/binaries/1.31.0-i686-uclibc/ ; \
    rename 's/busybox_//;y/A-Z/a-z/' *; chmod +x *
  ```
#### 3. Switching CPU Governor
       (This is technically safest overclocking you can do)
  ##### when to use this?
  - All Intel CPUs support inbuilt governor profiles, in this case we are switching the governor to Profile called Performance, it is built in and will push the CPU frequency to maximum supported by system. No CPU damage, as the profile is system created. 
  ##### snippet: For performance
  ```sh
  sudo bash -c 'for i in $(ls /sys/devices/system/cpu/ | grep cpu[0-9]); \
  do echo performance > /sys/devices/system/cpu/$i/cpufreq/scaling_governor; \
  done'
  ```
  (This will obviously drain the battery faster, so recommended to try when plugged in.)
  ##### snippet: For powersave
  ```sh
  sudo bash -c 'for i in $(ls /sys/devices/system/cpu/ | grep cpu[0-9]); \
  do echo powersave > /sys/devices/system/cpu/$i/cpufreq/scaling_governor; \
  done'
  ```
