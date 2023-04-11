# Setup minikube on fedora 37

## Docker
### Install docker
* ```
    sudo dnf remove docker \
        docker-client \
        docker-client-latest \
        docker-common \
        docker-latest \
        docker-latest-logrotate \
        docker-logrotate \
        docker-selinux \
        docker-engine-selinux \
        docker-engine
  
* `sudo dnf -y install dnf-plugins-core`
* `sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo`
* `sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin`

### Setup docker rootless
* `/usr/bin/dockerd-rootless-setuptool.sh install`
* Add `export PATH=/usr/bin:$PATH` and `export DOCKER_HOST=unix:///run/user/1000/docker.sock` to .zshrc

### Start rootless docker
* `systemctl --user start docker`
* Add `export DOCKER_HOST=unix://$XDG_RUNTIME_DIR/docker.sock` to .zshrc

### (Optional) Launch rootless docker on startup
* `systemctl --user enable docker`
* `sudo loginctl enable-linger $(whoami)`

## Minikube
### Install minikube
* `curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64`
* `sudo install minikube-linux-amd64 /usr/local/bin/minikube`

### Start minikube
* `minikube config set rootless true`
* `minikube start --driver=docker --container-runtime=containerd`

