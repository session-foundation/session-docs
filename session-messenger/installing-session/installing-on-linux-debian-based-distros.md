# Installing on Linux (Debian based distros)

Open a terminal and run the following commands

1. Add the Oxen GPG key using

```
sudo curl -so /etc/apt/trusted.gpg.d/oxen.gpg https://deb.oxen.io/pub.gpg
```

2. Add repository list

```
echo "deb https://deb.oxen.io $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/oxen.list
```

3. Update repositories

```
sudo apt update
```

4. Install Session

```
sudo apt install session-desktop
```

### Updating Session

If you want to update Session, you can run the following commands:

```
sudo apt update
```

then,

```
sudo apt upgrade
```
