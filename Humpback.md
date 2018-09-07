# Humpback

## Installation of requirements 

### Linux

#### Docker

**Update the `apt`** package index

```
$ sudo apt-get update
```

Install packages to allow `apt` to use a repository over HTTPS:

```
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
```

Add Docker’s official GPG key:

```
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Verify that you now have the key with the fingerprint`9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88`, by searching for the last 8 characters of the fingerprint.

```
$ sudo apt-key fingerprint 0EBFCD88

pub   4096R/0EBFCD88 2017-02-22
      Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid                  Docker Release (CE deb) <docker@docker.com>
sub   4096R/F273FCD8 2017-02-22
```

Use the following command to set up the **stable** repository. 

1. x86_64 / amd64

   ```
   $ sudo add-apt-repository \
      "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) \
      stable"
   ```

2. armhf

   ```
   $ sudo add-apt-repository \
      "deb [arch=armhf] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) \
      stable"
   ```

3. IBM Power (ppc64le)

   ```
   $ sudo add-apt-repository \
      "deb [arch=ppc64el] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) \
      stable"
   ```

4. IBM Z (s390x)

   ```
   $ sudo add-apt-repository \
      "deb [arch=s390x] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) \
      stable"
   ```

**Install Docker**

Update the `apt` package index.

```
$ sudo apt-get update
```

Install the *latest version* of Docker CE, or go to the next step to install a specific version:

```
$ sudo apt-get install docker-ce
```

To install a *specific version* of Docker CE

1. By list

   ```
   apt-cache madison docker-ce
   
   docker-ce | 18.03.0~ce-0~ubuntu | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
   ```

2. Knowing version

   ```
   $ sudo apt-get install docker-ce=<VERSION>
   ```

Verify is installed correctly

```
$ sudo docker run hello-world
```



Get from: https://docs.docker.com/install/linux/docker-ce/ubuntu/#uninstall-docker-ce



**After installation**

Create the `docker` group.

```
$ sudo groupadd docker
```

Add your user to the `docker` group.

```
$ sudo usermod -aG docker $USER
```

Log out and log back in so that your group membership is re-evaluated.

Get from: https://docs.docker.com/install/linux/linux-postinstall/#systemd



#### NPM

```sudo apt-get update```

```sudo apt-get install npm```



#### AHOY

```
sudo wget -q https://github.com/ahoy-cli/ahoy/releases/download/2.0.0/ahoy-bin-`uname -s`-amd64 -O /usr/local/bin/ahoy && sudo chown $USER /usr/local/bin/ahoy && chmod +x /usr/local/bin/ahoy
```

Get from: https://github.com/ahoy-cli/ahoy



### MAC

#### DOCKER

Download Community Edition here: https://store.docker.com/editions/community/docker-ce-desktop-mac

Install: Applications folder to start Docker.

Run: The whale in the top status bar indicates that Docker is running, and accessible from a terminal.

Get from: https://docs.docker.com/docker-for-mac/install/#install-and-run-docker-for-mac

#### Homebrew

Install

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Update

```
brew update
```

Verify

```
brew doctor
```

#### NPM

```
brew install node
```

#### AHOY

```
brew tap ahoy-cli/tap
brew install ahoy
```



## Install Humpback Generator

Install

```
npm install -g yo 

npm install -g generator-humpback
```

Set up Humpback

```
mkdir humpback-tutorial
cd humpback-tutorial
yo humback
```

Run

```
ahoy up
ahoy site local-settings
ahoy composer install
npm install
ahoy site install
```

LogIn Drupal

```
User: admin
Password: admin
```