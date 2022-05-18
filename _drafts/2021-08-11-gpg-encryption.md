---
layout: post
title:  "GPG encryption, to manage secrets publicly"
date:   2021-08-11 15:00:00 +0000
categories:
tags: gpg, encryption
---

[[_TOC_]]

# TL;DR

Send a private message over a public medium, no matter sender and receiver are exposed, because we still know who encpryts and also who receives the messages.

We will use OpenPGP as a method of encrypting and/or signing data in a secure end to end way. This means, the message is encrypted on messages' owner computer, using the recipient's public key, in a way that the medium where your messages as to travel has no knowledge of the content of the message. The recipient of the message then decrypts the message on their own computer using their private key.

# Install

Start by install `gpg` software. I'm on a mac hence `homebrew` is my friend.

```sh
11:09 $ gpg --version
gpg (GnuPG) 2.2.23
[TRUNCATED]
```

# generate

To create a new key, you'll be prompted to create a password for your GPG key

```sh
gpg --full-generate-key
```

The above command will generate a key pair, meaning, one "part of the key" is private and the other is public, for that reason keep in mind that the machine which creates the gpg key doesn't allow it to leave that same machine

# list 

all public keys

```sh
gpg --list-keys --keyid-format LONG --with-fingerprint
```

or private keys

```sh
gpg --list-secret-keys --keyid-format LONG
gpg --list-secret-keys --keyid-format LONG --with-fingerprint
```

# export 

your public gpg key can be copied and publish to whatever public forum you want, you can grab it from the list above with looking for entries like this one `pub   ed25519/A5E1D80098235EEE`

```sh
gpg --armor --export jose.neta@wemystic.com > ~/.ssh/jose.neta.wemystic.gpg.pub.asc
```

Note that it is common to use `.sig` is used for detached signatures using the binary OpenPGP format, and `.asc` when contents is ASCII-armored. For everything else, `.gpg` is common for the binary format, `.asc` when armored.

# Publish

or, instead as it is common practice you can send your key to a public key server

Prepend a '0x' to your key id

```sh
gpg --list-keys jose.neta@wemystic.com
gpg --list-keys --keyid-format '0xlong' jose.neta@wemystic.com
```

```sh
keyvalue=f4bdef5eb9504240
keyprefix=0x
gpg --keyserver keyserver.ubuntu.com --send-key ${key_prefix}${key_value}
```

# lookup

Check if your key was uploaded to the [key server](http://keyserver.ubuntu.com/pks/lookup?search=0xA1D156BA0580D973&fingerprint=on&op=index)

and your friends can get your keys from the server

```
key_without_prefix=
gpg --keyserver keyserver.ubuntu.com --recv-keys $key_without_prefix
```

or in the case of this key server you can simply use their web form to search by, for instance part of my email 

`s=jose.neta; http://keyserver.ubuntu.com/pks/lookup?search=${s}&fingerprint=on&op=index`

you'll get all my published gpg keys and then you can add my key to your keyring so that you can use me as a recipient for your messages.

# In Pratice

So let's say I have a new friend which I only know the email, I can go to [keyserver.ubuntu.com](keyserver.ubuntu.com) and lookup for instance for `renato.lourenco` and find his key, which I can then import

```sh
17:28 $ gpg --keyserver keyserver.ubuntu.com --recv-keys C4EF12DD30A2401B
gpg: key C4EF12DD30A2401B: public key "Renato Lourenco <renato.lourenco@wemystic.com>" imported
gpg: Total number processed: 1
gpg:               imported: 1
```

## encrypt message

and sign a document with it

```
gpg --encrypt --armor --recipient jose.neta@arpa.tech --recipient ttt@ist.utl.pt --output file.asc original.file
```

note that, as stated above, it is best pratices to suffix the encrypted file with `.asc`

## decrypt message

you will be prompted for the password of your gpg key

```sh
gpg -d file.asc
```

## delete you own pair of keys

start by deleting the private part

```sh
gpg --delete-secret-keys 'jose.neta@wemystic.com'
```

then the public part

```sh
gpg --delete-key 'jose.neta@wemystic.com'
```

# Conclusion

Encrypted messages with sensitive informations can be commit/publish wherever you want, so that you know that only the recipients will be able to read it.



---

WIP/MISC

```
gpg --export-secret-keys -a A5E1D80098235EEE > ~/.ssh/50NOS.gpg.asc
gpg --default-key F4BDEF5EB9504240 --sign-key C4EF12DD30A2401B

Please read email self defense


This server is a member of the sks-keyserver pool of servers. It hosts OpenPGP keys in a fashion that allows them to be quickly and easily retrieved and used by different client software.

You may connect to this server by adding one of the following entries to your OpenPGP client software.

pool.sks-keyservers.net
na.pool.sks-keyservers.net
eu.pool.sks-keyservers.net
oc.pool.sks-keyservers.net
p80.pool.sks-keyservers.net
ipv4.pool.sks-keyservers.net
ipv6.pool.sks-keyservers.net
subset.pool.sks-keyservers.net
Note: keys.gnupg.net and pgp.ipfire.org are both alias for pool.sks-keyservers.net. Requests sent to either of these hosts will also be served by this server.

OpenPGP Resources

GnuPG Homepage - The main location for the OpenPGP Standard.
SKS Keyserver Homepage - The keyserver software running on this server.
PGP Inc. - The historical home of PGP, but has since been sold to Symantec.
Email Self-defense - A teaching site about how to use OpenPGP to communicate.
Wikipedia - Pretty Good Privacy - A nice overview of what OpenPGP is.

https://pgp.surfnet.nl


SURF's OpenPGP Public Key
If you have any issues or concerns about this site, or if you wish to peer with this server, please contact me via the email address within the below public key 0x2de51bc259a925cd



OpenPGP is a method of encrypting and/or signing data (for example an email) in a secure “end to end” way. This means, the message is encrypted on your computer, using the recipient’s public key, in a way that the e-mail server has no knowledge of the content of the message. The recipient of the message then decrypts the message on their own computer using their private key.
              
# MISC

Let's say you decrypt a message from someone but you don't have the public key of that someone, that will result in

> gpg: Can't check signature: No public key

will not work with htts: gpg --keyserver https://keyserver.ubuntu.com --search-keys noreply@kraken.com
gpg --keyserver hkps://keyserver.ubuntu.com --search-keys noreply@kraken.com
gpg --list-keys --keyid-format LONG --with-fingerprint

I need to try receive key as I did above 


# reference

- <https://www.digitalocean.com/community/tutorials/how-to-use-gpg-to-encrypt-and-sign-messages>
- <https://docs.github.com/en/github/authenticating-to-github/generating-a-new-gpg-key>
- <https://blog.chapagain.com.np/gpg-remove-keys-from-your-public-keyring/>
- <https://www.digitalocean.com/community/tutorials/how-to-use-gpg-to-encrypt-and-sign-messages>
- <https://docs.fedoraproject.org/en-US/quick-docs/create-gpg-keys/>
- <https://emailselfdefense.fsf.org/en/>
https://support.mozilla.org/en-US/kb/introduction-to-e2e-encryption

# WIP

0. Signing the keys  

1. Segurança na partilha da chave pub, guardar num server é seguro? Isto num caso em que p.e. tenhas uma api para uma chat app?

2. Até que ponto existe segurança de manter a chave priv no dispositivo? Tanto mobile como pc/desktop/raspberry etc

3. Mesmo com a password/phrase, sabemos que existem métodos de dar “advinhar” a pass, mesmo criando uma pass random (como o lastpass faz)

4. Serão plataformas como lastpass assim tão seguras? Não será a mesma coisa escrever as passes em ficheiros .txt no teu dispositivo?

Ou seja, estamos numa fase onde a segurança/privacidade é uma mera miragem e a realidade é que com estes passos só dificultas que te encontrem?
