# ARCS Lab Servers Documentation

Documentation for lab servers at ARCS lab of CMU Robotics Institute.

## General Policies

1. Do not brick the servers.
1. [Do not brick the servers.](https://www.youtube.com/watch?v=dC1yHLp9bWA&t=12)
1. Since the servers are shared, please prefer to use containers such as [Singularity/Apptainer](https://apptainer.org) and [Docker](https://www.docker.com/). For python usage, please install [miniconda](https://docs.conda.io/en/latest/miniconda.html) to your account.
1. For people who have sudo access, it is possible to change other peoples' passwords and mess around with their files. Do not do this.
1. Do not store sensitive information.

## Getting Access

1. Request Yulun (or other PhD students) to create an account for you.
1. By default, all users, except for PhD students, do NOT have sudo access. If you need sudo to install some simple packages, ask PhD students for help. If you need sudo to do something more complicated, discuss with PhD students for sudo access.

## Pikachu

<img src="img/pikachu.png" alt="drawing" width="200"/>


### System Specs

CPU: `AMD Threadripper 3990X`, 64 cores/128 threads.

RAM: 192 GB

GPU: `NVIDIA RTX 3090Ti`


### Direct SSH

SSH into `pikachu0.lan.cmu.edu`, e.g.

```
ssh USERNAME@pikachu0.lan.cmu.edu
```

### Cloudfare SSH

We added a cloudflare tunnel if there is any situation for direct ssh of pikachu public domain (and because we do not like the `0` after `pikachu` in the public domain name).

1. Install cloudflared client in your computer.

   On Linux, download the latest cloudflared debian package from [their website](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/install-and-setup/tunnel-guide/local/).

   At the time of writing this readme this was the debian that worked:

   ```
   https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/install-and-setup/tunnel-guide/local/
   ```

   After downloading the above install it by running `sudo dpkg -i <downloaded-package-name>`

   Instructions for macOS and Windows are on [their website](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/install-and-setup/tunnel-guide/local/).

1. Copy and paste the following in your ssh config (generally located at: `~/.ssh/config` ):

   ```
   Host pikachu.noctis.cool
       ProxyCommand cloudflared access ssh --hostname %h
   ```

1. Then you can access pikachu by this domain : `pikachu.noctis.cool`.
   That is:
   ```
   ssh USERNAME@pikachu.noctis.cool
   ```

### Disks

Pikachu has two disks, a main disk (1TB) with Ubuntu 20.04 OS (`/home`) and a project disk (4TB) mounted at `/media/project0`. To use them:

1. When your account is created, your home directory is at `/home/<username>` in the main disk. Please store config related stuff (packages, softwares, etc) there.

1. Move your project related stuff (mainly log data and code) to the project disk. Create a folder under your name (e.g. `/media/project0/yulun`) and store everything there. The project disk in general needs sudo access to read/write. Please let Yulun (or other people with sudo) know if you need to store something there and they will help you create the folder and grant you read/write access to that folder. This is to prevent people from accidentally manipulate other people’s files.


## Eevee

<img src="img/eevee.png" alt="drawing" width="200"/>


### System Specs

CPU: `AMD Threadripper 7980X`, 64 cores/128 threads.

RAM: 256 GB

GPU: `NVIDIA GTX 1080Ti`

### Direct SSH

SSH into `eevee0.lan.cmu.edu`, e.g.

```
ssh USERNAME@eevee0.lan.cmu.edu
```

### Cloudfare SSH

Same as Pickachu and add this to your `.ssh/config`

```bash
Host eevee.noctis.cool
ProxyCommand cloudflared access ssh --hostname %h
```



### Disks

Eevee has one disk of 4TB with Ubuntu 20.04 OS (`/home`). For now you may put everything here.


## Remote Desktop

If you have `sudo` and there is no `xrdp` in your computer, follow [install xrdp](https://gist.github.com/aatizghimire/068f7b3537a33df8279e871ba20c8cc6)

Please confirm your `xrdp` is running and get the port number.

Run this in your own computer
```
ssh -CNfL your_port:127.0.0.1:remote_server_rdp_port remote_server
```

Then download `Remote Desktop` from Microsoft in your own computer.

In `Remote Desktop` use address (PC name) `locoalhost:remote_server_rdp_port`, and you can use server with desktop remotely.



