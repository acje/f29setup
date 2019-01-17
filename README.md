# Fedora 29 setup

Update and reboot:
```
sudo dnf upgrade --refresh -y
sudo shutdown -r now
```

Install docker:
```
sudo dnf config-manager     --add-repo     https://download.docker.com/linux/fedora/docker-ce.repo
sudo dnf install docker-ce -y
sudo systemctl start docker
sudo systemctl enable docker
```
Run Docker as non-root user
```
sudo usermod -aG docker your-user
```
Install rust (note -k ignores cert, usefull behind company FW with TLS inspection. The following line is not great security practice)
```
curl https://sh.rustup.rs -sSfk | sh
```
Install vscode
```
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
dnf check-update
sudo dnf install code -y
```
