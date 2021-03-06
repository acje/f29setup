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
sudo usermod -aG docker [your-user]
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
.bashrc:
```
#prompt with git branch info
source /usr/share/git-core/contrib/completion/git-prompt.sh
PS1='\[\033[0;32m\]\[\033[0m\033[0;32m\]\u\[\033[0;36m\] @ \[\033[0;36m\]\h \w\[\033[0;32m\]$(__git_ps1)\n\[\033[0;32m\]└─\[\033[0m\033[0;32m\] \$\[\033[0m\033[0;32m\] ▶\[\033[0m\] '

# blue/green prompt (old)
#PS1="\[\033[38;5;7m\][\[$(tput sgr0)\]\[\033[38;5;33m\]\u\[$(tput sgr0)\]\[\033[38;5;15m\]@\[$(tput sgr0)\]\[\033[38;5;33m\]\h\[$(tput sgr0)\]\[\033[38;5;15m\] \[$(tput sgr0)\]\[\033[38;5;10m\]\W\[$(tput sgr0)\]\[\033[38;5;7m\]]\\$\[$(tput sgr0)\]"

# pull :latest for all local docker containers
alias docker-all='docker images --format "{{.Repository}}:{{.Tag}}" | grep :latest | xargs -L1 docker pull'

```

Set hostname:
```
hostnamectl set-hostname --static [hostname]
```

Configure git:
```
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```

Install diesel cli for rust/postgres development:
```
sudo dnf install postgres-devel
cargo install diesel_cli --no-default-features --features postgres
```

make gcloud path work over time (replace ### with "current"), .bachrc:
```
PATH="$HOME/.local/bin:$HOME/bin:$PATH:/var/lib/snapd/snap/google-cloud-sdk/current/bin"
```

http://neowaylabs.github.io/programming/unix-shell-for-data-scientists/

bazel
https://docs.bazel.build/versions/master/install-redhat.html

IntelliJ community edition
```
sudo snap install intellij-idea-community --classic
```

