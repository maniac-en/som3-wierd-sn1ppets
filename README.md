# some-sn1ppets

## Note
- Some of these snippets are gonna very specific use-cases, so please mind the rant!
- If you have any suggestions, please let me know! I'll add them `probably`
- If you have a use-case but you are still facing issues, you can DM me on [twitter](https://twitter.com/maniac_en)

## v0.1

## Index
1. [Transferring big file](#1-transferring-big-file)
2. [Downloading all busybox binaries](#2-Downloading-all-busybox-binaries)

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
