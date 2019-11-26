# Fedora 31 setup

Update and reboot:
```
sudo dnf upgrade --refresh -y
shutdown -r now
```

Install docker: (not working due to cgroups v2 upgrade)

https://fedoraproject.org/wiki/Common_F31_bugs#Docker_package_no_longer_available_and_will_not_run_by_default_.28due_to_switch_to_cgroups_v2.29
workaround

https://medium.com/nttlabs/cgroup-v2-596d035be4d7

```
sudo dnf config-manager     --add-repo     https://download.docker.com/linux/fedora/docker-ce.repo
sudo dnf install docker-ce -y
sudo systemctl start docker
sudo systemctl enable docker
```
Troubleshoot docker on f31:

https://www.reddit.com/r/linuxquestions/comments/dn2psl/upgraded_to_fedora_31_docker_will_not_work/f5sml7f/

Run Docker as non-root user
```
sudo usermod -aG docker [your-user]
```

Add company MITM Cert

https://unix.stackexchange.com/questions/366898/generate-hpkp-fingerprints-for-all-certificate-chain
```
echo q | openssl s_client -servername example.com -connect example.com:443 -showcerts \
  | awk '/-----BEGIN/{f="cert"(n++)".pem"} f{print>f} /-----END/{f=""}'

sudo cp cert[1-3].pem /etc/pki/ca-trust/source/anchors/
update-ca-trust enable
rm cert[0-3].pem
```

Install rust
```
curl https://sh.rustup.rs -sSf | sh
```
Install vscode
```
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
dnf check-update
sudo dnf install code -y
```
(configure File/Preferences/"Window: Title Bar Style" = custom)

.bashrc:
```
# blue/green prompt
PS1="\[\033[38;5;7m\][\[$(tput sgr0)\]\[\033[38;5;33m\]\u\[$(tput sgr0)\]\[\033[38;5;15m\]@\[$(tput sgr0)\]\[\033[38;5;33m\]\h\[$(tput sgr0)\]\[\033[38;5;15m\] \[$(tput sgr0)\]\[\033[38;5;10m\]\W\[$(tput sgr0)\]\[\033[38;5;7m\]]\\$\[$(tput sgr0)\]"

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
