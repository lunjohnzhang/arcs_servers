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

### Direct SSH

SSH into `pikachu0.lan.cmu.edu`, e.g.

```
ssh USERNAME@pikachu0.lan.cmu.edu
```

### Cloudfare SSH

We added a cloudflare tunnel if there is any situation for direct ssh of pikachu public domain (and because we do not like the `0` after `pikachu` in the public domain name).

1. Install cloudflared client in your computer.
To do so download the latest cloudflared debian package from [their website](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/install-and-setup/tunnel-guide/local/)

At the time of writing this readme this was the debian that worked:

```
https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/install-and-setup/tunnel-guide/local/
```

After downloading the above install it by running ```sudo dpkg -i <downloaded-package-name>```


2. Copy and paste the following in your ssh config (generally located at: `~/.ssh/config` )

   ```
   Host pikachu.noctis.cool
   ProxyCommand cloudflared access ssh --hostname %h
   ```


3. Then you can access pikachu by this domain : `pikachu.noctis.cool`.
   That is 
   ```
   ssh USERNAME@pikachu.noctis.cool
   ```
