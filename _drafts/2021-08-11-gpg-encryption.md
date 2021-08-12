---
layout: post
title:  "GPG encryption, to manage secrets publicly"
date:   2021-08-11 15:00:00 +0000
categories:
tags: gpg, encryption
---


## TL;DR

If you want to send a private message over a public medium, keep on reading

# install

Start by install `gpg` software

```
11:09 $ gpg --version
gpg (GnuPG) 2.2.23
[TRUNCATED]
```

# generate

To create a new key

```
gpg --full-generate-key
```

Note that you'll be prompted to create a password for your GPG key

# list 

all public keys

```
gpg --list-keys --keyid-format LONG --with-fingerprint
```

or private keys

```
gpg --list-secret-keys --keyid-format LONG
gpg --list-secret-keys --keyid-format LONG --with-fingerprint
```

# export 

your public gpg key can be copied and publish to whatever public forum you want, you can grab it from the list above with looking for entries like this one `pub   ed25519/A5E1D80098235EEE`

```
gpg --armor --export jose.neta@arpa.tech > ~/.ssh/arpa-gpg.pem
```

# publish

or, instead as it is common practice you can send your key to a public key server

Prepend a '0x' to your key id

```
keyvalue=A1D156BA0580D973
keyprefix=0x
gpg --keyserver keyserver.ubuntu.com --send-key ${key_prefix}${key_value}
```

# lookup

Check if your key was uploaded to the [key server](http://keyserver.ubuntu.com/pks/lookup?search=0xA1D156BA0580D973&fingerprint=on&op=index)

and your friends can get your keys from the server

```
key_wihtout_prefix=
gpg --keyserver keyserver.ubuntu.com --recv-keys $key_wihtout_prefix
```

or in the case of this key server you can simply use their web form to search by, for instance part of my email 

`s=jose.neta; http://keyserver.ubuntu.com/pks/lookup?search=${s}&fingerprint=on&op=index`

you'll get all my published gpg keys and then you can add my key to your keyring so that you can use me as a recipient for your messages.

So let's say I have a new friend which I only know the email, I can go to keyserver.ubuntu.com and lookup for instance for `renato.lourenco` and find his key, which I can then import

```
17:28 $ gpg --keyserver keyserver.ubuntu.com --recv-keys C4EF12DD30A2401B
gpg: key C4EF12DD30A2401B: public key "Renato Lourenco <renato.lourenco@wemystic.com>" imported
gpg: Total number processed: 1
gpg:               imported: 1
```

# encrypt

and sign a document with it

```
gpg --encrypt --armor --recipient jose.neta@arpa.tech --recipient ttt@ist.utl.pt --output file.asc original.file
```

note that it is best pratices to suffix the encrypted file with `.asc`

# decrypt

you will be prompted for the password of your gpg key

```
gpg -d file.asc
```

# delete you own pair of keys

start by deleting the private part

```
gpg --delete-secret-keys 'jose.neta@arpa.tech'
```

then the public part

```
gpg --delete-key 'jose.neta@arpa.tech'
```

MISC

gpg --export-secret-keys -a A5E1D80098235EEE > ~/.ssh/50NOS.gpg.asc


# conclusion

You can now encrypt some messages with sensitive informations and commit/publish them wherever you want, so that you know that only the recipients of that encryptation will be able to read it.

# reference

- <https://www.digitalocean.com/community/tutorials/how-to-use-gpg-to-encrypt-and-sign-messages>
- <https://docs.github.com/en/github/authenticating-to-github/generating-a-new-gpg-key>
- <https://blog.chapagain.com.np/gpg-remove-keys-from-your-public-keyring/>
- <https://www.digitalocean.com/community/tutorials/how-to-use-gpg-to-encrypt-and-sign-messages>
- <https://docs.fedoraproject.org/en-US/quick-docs/create-gpg-keys/>
- <https://emailselfdefense.fsf.org/en/>
