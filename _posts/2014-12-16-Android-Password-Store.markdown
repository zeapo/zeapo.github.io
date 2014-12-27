---
layout: post
title:  "[PwdStr] Migration to a simpler storage"
description: Adding support for external store management
categories:
- blog
---

One important request that we've got for [Android-Password-Store](https://github.com/zeapo/Android-Password-Store) is to support external synchronization tools such as Dropbox or manual scp and so on. At the beginning, I must admit, I was not really keen on implementing it that soon for one major reason: *adding more complexity will inherently add more bugs*. However, during the last few days I changed my mind.

### Why?

Two weeks ago I started a [pull request](https://github.com/zeapo/Android-Password-Store/pull/57) where  I refactor the whole git process. Few days after, I decided that it was time to implement the [multiple-stores feature](https://github.com/zeapo/Android-Password-Store/issues/38) and make it part of the the same pull request. This way I guarantee that the new git process is fully compatible with that feature. However, the more I work on this, the more I notice that we should delegate the synchronization of the store to a 3-rd party tool that will handle the whole process. 

### How?

The idea is simple, store the gpg files on the sdcard. That way the user can synchronize it using [agit](https://github.com/rtyley/agit) for Git, or any other synchronization tool (Dropbox, Drive, etc.)

### Does it mean less security?

The store is actually stored in the data directory. Therefore, only our application or the user himself (using `adb`) could access the files. On rooted devices, file explorers are able to access these files.

Storing the files outside the realms of our application means that the files are accessible by any application having rights to read from the sdcard. That being said, on your Linux or Mac, the store is as accessible as any other file on your home directory.

The security of pass files comes from the gpg encryption. And from the user himself, if he chooses to use a passphrase for his private key. Therefore, it will not be less secure, the passwords will still be encrypted.

### But, I really worry about my meta-information...

It is true that the name of the password files is by itself an information that an attacker can use against you. For instance, if you name the password `myuser_at_website.gpg`, the attacker will know what's your username and the website.

The current behavior **will not** be dropped. The user will be prompted when he/she would like to create or import a new store where to put it. 

### When? 

No ETA yet.