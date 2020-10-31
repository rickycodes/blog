---
title: "Publishing my Website to the peer-to-peer Web"
date: 2018-09-05T13:14:04-04:00
tags: [dat, peer-to-peer, peer-to-peer-web]
description: "How to publish and deploy your website with the decentralized dat:// protocol"
---
I've been experimenting with the <a href='https://datproject.org/'>DAT protocol</a> for a little while now and wanted to get my website up on the peer-to-peer Web. There's a few different ways to do this, but I wanted a method that worked well with my already existing method of deployment.

##### 1. Building

<a href='https://github.com/rickycodes/www'>My website</a> is currently written in <a href='https://www.rust-lang.org/'>Rust</a> using <a href='https://github.com/koute/stdweb'>stdweb</a> and building it is a single command:

```
cargo web build --target=wasm32-unknown-unknown
```

This creates a folder (`./target/deploy/`) that's ready to "deploy"

#### 2. Deploy

I deploy changes to my website using <a href='https://en.wikipedia.org/wiki/Rsync'>rsync</a> which looks something like:

```
rsync -avz -e "ssh -p 9999" ./target/deploy/ user@hostname:/path/to/website/
```

<em>(modified slightly)</em>

#### 3. Creating a DAT version

I created the DAT version of my website using the <a href='https://github.com/datproject/dat'>`dat` command line tool</a>. Once that was set up I also created a copy using <a href='hashbase.io'>hashbase.io</a> so I wouldn't have to worry about seeding it myself.

#### 4. Synching Changes to DAT

Synching changes to the DAT version is just a matter of copying the freshly built site (while keeping track of changes<sup>1</sup>) and running dat sync using the dat command line tool:

```
rsync -r --size-only target/deploy/* ignore/dat/
dat sync --dir=path/to/dat
```

That's basically it. For simplicity I add all the commands to a `deploy.sh` shell script so I can simply run `bash deploy.sh` to deploy changes to both the http(s) and DAT versions of my website in one go.

Now you can visit <a href='//ricky.codes'>ricky.codes</a> with a browser that supports the DAT protocol (like <a href='https://beakerbrowser.com/'>Beaker</a>).

<sup>1</sup>Changed from `cp -r ...` to `rsync --size-only` ... so we don’t needlessly replace files and create new revisions… (I actually blew through my hashbase.io allotment doing this). Perhaps managing all this in a different git repo would make more sense?

#### Further Reading

<a href='https://taravancil.com/blog/how-i-publish-taravancil-com/'>How I publish taravancil.com on the peer-to-peer Web</a>

