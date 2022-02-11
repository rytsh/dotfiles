# configurations

Based on __Arch Linux__

![minimal](assets/minimal.png)

<details><summary>Packages</summary>

## Install packages

Font
```
ttf-anonymous-pro
```

Terminal
```sh
xterm
```

Window manager
```sh
openbox hsetroot unclutter
```

</details>

<details><summary>Git</summary>

## GPG sign

Generate new key

```sh
gpg --full-gen-key
```

Real Name: Eray Ates  
Email Address: eates23@gmail.com

```sh
gpg --list-secret-keys --keyid-format LONG eates23@gmail.com
```

And copy bold area rsa3072/__KEYID__ 2022-01-01

```sh
gpg --armor --export __KEYID__
```

Add to the gitconfig

```sh
git config --global user.signingkey __KEYID__
# git config --file .git/personal user.signingkey __KEYID__
```

Enable to sign always

```sh
git config --global commit.gpgsign true
```

## Specific folder use different key

Add this config in the `~/.gitconfig`

```
[includeIf "gitdir:~/github/*/"]
	path = ~/.git/personal
```

And personal file like

```
[user]
	email = eates23@gmail.com
	name = Eray Ates
	user = rytsh
```

Or use with `git config -l --file=.git/personal` and set new things.

</details>

<details><summary>Network</summary>

## VM Network Settings

### VirtualBox

Add NAT and Host-only network to the VM.

## Change names of the interface

```
cat /etc/udev/rules.d/10-network.rules
```

Find mac address with `ip link` command or manual way `cat /sys/class/net/enp0s3/address`.

```
SUBSYSTEM=="net", ACTION=="add", ATTR{address}=="08:00:27:a9:fd:20" NAME="nethost"
SUBSYSTEM=="net", ACTION=="add", ATTR{address}=="08:00:27:1a:c4:cd" NAME="netnat"
```

## Network Settings

Enable systemd-networkd service

```
sudo systemctl enable systemd-networkd.service
```

/etc/systemd/network/20-wired.network

```
[Match]
Name=nethost

[Network]
DHCP=yes
```

/etc/systemd/network/21-wired.network

```
[Match]
Name=netnat

[Network]
DHCP=yes
```

Check with `networkctl list` command.

</details>
