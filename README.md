# Drocker

![Docker + Drupal](https://raw.githubusercontent.com/twinbit/drupal-docker-env/master/src/dde.png)

## Introduction - What is Drocker

@TODO:

- What's inside
- The problem it solves
- Our development environment

## Do I need it?

Probably if you work with Drupal on a local environment (as you should), you can do yourself a favor using Drocker. More specifically:

- **If you are working on a Linux box (natively)** and you use a local, native LAMP development stack
- **If you currently rely on VMs (VirtualBox, VMWare, etc)** to hold your development environment
- **If you like to tweak the environment on a per-project basis** to achieve maximum stability, performance, security, etc.
- **If you can use Docker in production** and you'd like to deploy your application atomically


## I feel like I don't need it...

If none of the above applies you probably don't need Drocker. On the other hand if none of the above applies you are making yourself a major disservice not adopting the best Drupal development and project-management practices, either in team or alone.

Think over trying Drocker to exit your comfort-zone and discover a new way of drupalling **like a boss**! :)

## Requirements

@TODO:

- GNU/Linux or boot2docker
- PHP (? really? Should we switch to bash script?)
- BindFS support at a filesystem level (Looking at you Windows...)
- A hostname resolution system (dnsmasq on linux... mac?)
- ...

## Installation

### Phar

[Download drocker.phar >](http://twinbit.github.io/drocker/drocker.phar)

```
wget http://twinbit.github.io/drocker/drocker.phar
```
To install globally put `drocker.phar` in `/usr/bin`.

```
sudo chmod +x drocker.phar && mv drocker.phar /usr/bin/drocker
```

Now you can use it just like `drocker`.


## Configuration

@TODO: 

- How to tweak `fig.yml` to match your needs and run `fig up -d` to run the containers. (At the first run fig will download and build remote containers, it can takes several minutes.)
- Activate / deactivate containers on a per-project basis
- Adding containers for additional services
- Using nginx with Drupal (.htaccess issues)


## Usage

Just run `drocker init` in a empty folder to bootstrap a new drocker project:

```
|-- bin
|   |-- cli
|   |-- drush
|   |-- mysql
|   `-- phing
|-- data
`-- fig.yml
```

@TODO: Then? Explain:

- The "one env per projects" idea (multiple containers per project)
- How to switch from a context to another (can you have more projects running at the same time on the same ports? don't think so...)
- Alternatives (multiple projects per env?)
- How to deploy your containers (change the data container)

## How this thing works

@TODO: explanation about architecture and internals

- Docker basics (link to advanced docs)
- Containers structure (with graphical schema)
- The data container pattern (is it worth?)
- The service stack: SOLR, mailcatcher, etc etc
- Our development environment


## Boot2docker configuration steps:

@TODO: is this up to date or what?

In order to avoid going insane trying to fix vboxfs shared folder permissions, it is better to
use NFS from OSX in order to have an automatic mapping to local user uid/gid:


### OSX NFS Export

Edit your /etc/exports file as follows:

```
/Users -mapall=youruser:staff boot2docker
```

### Boot2docker configuration

```
boot2docker ssh
sudo umount /Users
sudo /usr/local/etc/init.d/nfs-client start
# See http://www.slashroot.in/how-do-linux-nfs-performance-tuning-and-optimization
# See http://www.gossamer-threads.com/lists/wiki/mediawiki-cvs/500057
sudo mount 192.168.59.3:/Users /Users -o rw,async,noatime,rsize=32768,wsize=32768,proto=tcp
```

## Troubleshooting

### Sublime text NFS bug

@TODO: is this up to date or what?

If you are using SublimeText, you could experience this problem: https://github.com/mitchellh/vagrant/issues/2768:
`NFS not updating files if the file length stayed the same.`

To fix it: Sublime Text > Preferences > Settings- User

```
{
    "atomic_save": false
}
```
