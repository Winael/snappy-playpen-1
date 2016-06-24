---
file: snapcrafting-is-not-easy-as-it-seems.md
title: Snapcrafting is not easy as it seems
author: Winael
date: 2016-06-20
tags: snapcraft, ubuntu, snaps, etherpad-lite
---

Few month ago, I'd like to try to create my first snap package. On the paper it seemed so simple to do it. Virtually can can take any project on github and with the help of few tools like Snapcraft and its pugins, tadaaaaa, you're creating a snap.

So, I thank, _"OK, I'll try to build snaps package from Framacloud services"_, and begun by the most popular of them, Etherpad-lite

# Meta-informations

First of all, when you created a snaps, you'll need to create a `snapcraft.yaml` file. It's your recipe to create your snap from differents sources as git ou bazaar repository.

You can generated one with the `snapcraft init` command that will create the `snapcraft.yaml`file based on a basic template

So basically, this is what meta-infos look like for this snap :

````yaml
name: etherpad-lite-unofficial
version: 0.1
summary: Etherpad-lite, really-real time collaborative editor
description: |
  Etherpad allows you to edit documents collaboratively in real-time, much like
  a live multi-player editor that runs in your browser. Write articles, press
  releases, to-do lists, etc. together with your friends, fellow students or
  colleagues, all working on the same document at the same time.

````

I use _etherpad-lite_ as a **name**, this is the **version** _0.1_, the **sumary** is a little sentences for the store list, and the description is needed for the store. Note that you can use the pipe symbols to write very long description

# Confinement

At this step we don't need confinement. It would be easier to debug our snap without it. It will be set up at the very end of our journey

To desactivate confinement, you can set the yaml key `confinement` to `devmode`

````yaml
confinement: devmode
````

# The parts

## Etherpad-lite

According to the [install documentation for Linux systems][1], we need to install gzip, git, curl, libssl develop libraries, python and gcc.

it also says that you need nodejs installed. Etherpad-lite is a nodejs app, so we need the ǹodejs`plugin to build this part. The particularity of this pugin is that you need to specify the source subdirectory from where the app will be built

We will pull the source from the [Etherpad-lite github directory][2]

So `etherpad-lite` parts could be writen like this :

````yaml
parts:
  etherpad-lite:
    plugin: nodejs
    source: git://github.com/ether/etherpad-lite
    source-subdir: src
    build-packages:
      - gzip
      - git
      - curl
      - python
      - libssl-dev
      - pkg-config
      - build-essential
````

So now, you can run your first `snapcraft stage` to look how the part is built.

So what's happened ? 

As you can see, a folder `parts` is created. It's where the pats are built. If you look into your `parts` directory you can see an etherpad-lite folder corresponding to your etherpad-lite parts

````bash
$ ls -lrt parts
total 4
drwxrwxr-x 9 etherpad-lite etherpad-lite 4096 Jun 20 21:34 etherpad-lite
````



[1]: https://github.com/ether/etherpad-lite#gnulinux-and-other-unix-like-systems
[2]: git://github.com/ether/etherpad-lite
