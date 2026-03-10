## Sur la machine Gitlab 

### Créer l'Authentication Token

Sur Gitlab

![[Pasted image 20260101205510.png]]

![[Pasted image 20260101205600.png]]

Cocher `Exécuter les jobs sans étiquettes`.

Ajouter une description.

Puis sur `Créer un runner`.

## Sur la machine Gitlab Runner

### Installer et lancer 
Pour Debian: 

```bash
# Download the binary for your system
sudo curl -L --output /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64

# Give it permission to execute
sudo chmod +x /usr/local/bin/gitlab-runner

# Create a GitLab Runner user
sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash

# Install and run as a service
sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
sudo gitlab-runner start
```


### Rejoindre Gitlab en tant que runner

```bash
gitlab-runner register \
--url https://gitlab.int.lavaduck.net \
--token glrt-xxxx
```

Rajouter l'Executor selon le type du runner (`docker`, `shell`, etc..).

Lancer `gitlab-runner run`.