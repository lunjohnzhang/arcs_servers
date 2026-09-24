# ARCS Lab Servers Documentation

Documentation for lab servers at ARCS lab of CMU Robotics Institute.

## General Policies

1. Do not brick the servers.
1. [Do not brick the servers.](https://www.youtube.com/watch?v=dC1yHLp9bWA&t=12)
1. Since the servers are shared, please prefer to use containers such as [Singularity/Apptainer](https://apptainer.org) and [Docker](https://www.docker.com/). For python usage, please install [miniconda](https://docs.conda.io/en/latest/miniconda.html) to your account.
1. For people who have sudo access, it is possible to change other peoples' passwords and mess around with their files. Do not do this.
1. Do not store sensitive information.

## Getting Access

**Both Pikachu and Eevee require SSH keys for SSH access. Username + password login over SSH is disabled on both servers. You can still log in at either server's desktop using your Linux username and password.**

1. Generate an SSH key on your own computer using the [migration instructions below](#migrating-to-ssh-key-authentication).
1. Request Yulun (or other PhD students) to create an account for you. Send your **public key** (`.pub` file) and specify which servers you need access to. An administrator must install your public key before your first SSH login; never send your private key.
1. By default, all users, except for PhD students, do NOT have sudo access. If you need sudo to install some simple packages, ask PhD students for help. If you need sudo to do something more complicated, discuss with PhD students for sudo access.

## Migrating to SSH Key Authentication

Password-based SSH login is disabled on both Pikachu and Eevee; SSH keys are required. You will still use your existing server username, but authenticate with a key instead of your Linux account password. This applies to both direct SSH and SSH through Cloudflare, including SSH tunnels for remote desktop. Your Linux account password remains usable for local desktop login, screen unlocking, and `sudo` where permitted; remote desktop authentication is configured separately.

### 1. Generate a key on your own computer

Run the following in a terminal on macOS/Linux or in Windows PowerShell with OpenSSH installed:

```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_arcs
```

Choose a passphrase to protect the private key. If that file already exists, reuse your existing key or choose a different filename; **do not overwrite an existing key**. If you use another filename, substitute it throughout the examples below.

- `~/.ssh/id_ed25519_arcs` is your **private key**. Keep it on your own computer and never share it or upload it to a server.
- `~/.ssh/id_ed25519_arcs.pub` is your **public key**, which is installed on each server you use. Each user must have their own key pair.

### 2. Install your public key on each server

Send the contents of your `.pub` file to Yulun or another administrator and specify your username and which servers you need. The administrator will install the public key in your account's `~/.ssh/authorized_keys` on each requested server:

```bash
cat ~/.ssh/id_ed25519_arcs.pub
```

Since password-based SSH login is disabled, `ssh-copy-id` cannot use your account password to install your first key. Send only the public key, never the file without the `.pub` extension.

### 3. Configure SSH to use your key

Add the following to `~/.ssh/config` on your own computer (`$HOME\.ssh\config` on Windows), replacing `USERNAME` with your server username. If you already have entries for these hosts, add the `User`, `IdentityFile`, and `IdentitiesOnly` settings to those entries. Use a separate `User` value in each block if your usernames differ.

```sshconfig
Host pikachu0.lan.cmu.edu pikachu.noctis.cool
    User USERNAME
    IdentityFile ~/.ssh/id_ed25519_arcs
    IdentitiesOnly yes

Host eevee0.lan.cmu.edu eevee.noctis.cool
    User USERNAME
    IdentityFile ~/.ssh/id_ed25519_arcs
    IdentitiesOnly yes
```

Keep the existing `ProxyCommand cloudflared access ssh --hostname %h` entries for the `.noctis.cool` hosts as described below. `IdentityFile` selects your private key, and `IdentitiesOnly yes` avoids offering unrelated keys from your SSH agent.

### 4. Test key login

Keep any working SSH session open. In a **new terminal on your own computer**, test each server using public-key authentication only:

```bash
ssh -o PreferredAuthentications=publickey USERNAME@pikachu0.lan.cmu.edu
ssh -o PreferredAuthentications=publickey USERNAME@eevee0.lan.cmu.edu
```

Use the Cloudflare hostnames instead if that is how you normally connect. A prompt for your **key's passphrase** is normal; it is different from your server account password. Once these tests succeed, the ordinary SSH commands below will use your configured key as well.

If you get `Permission denied (publickey)`, check your username and `IdentityFile`, and ask an administrator to confirm that the matching public key is installed on that server. The server's `~/.ssh` directory should be owned by your account with permissions `700`, and `~/.ssh/authorized_keys` should be owned by your account with permissions `600`.

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

Pikachu has two disks:

- A **4TB main drive** containing Ubuntu 24.04 and the root directory (`/`), including user home directories at `/home/<username>`. Store your packages, configuration, code, and project data in your home directory.
- A **1TB drive mounted as an external drive at `/media/project0`**. Contact Yulun (or another administrator) for help creating a folder with the appropriate permissions if you need to use it.



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
