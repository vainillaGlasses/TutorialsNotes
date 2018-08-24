# Git

## Install

### Windows

https://git-scm.com/download/win

### MacOS

https://git-scm.com/download/mac

### Linux

#### Ubuntu/Debian

`apt-get install git`

#### Fedora

`yum install git` (<= 21)

`dnf install git ` (>= 22)

#### Others

https://git-scm.com/download/linux



### Configure

For commit transactions

```
git config --global user.name "Name"
```

```
git config --global user.email "mail@mail.com"
```

**Define editor**

```console
git config --global core.editor <editor name>
```

**Colors**

Git automatically colors most of its output, but there’s a master switch if you don’t like this behavior.

Enables helpful colorization of command line output

```
git config --global color.ui auto
```

