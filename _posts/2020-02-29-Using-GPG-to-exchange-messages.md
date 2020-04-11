---
layout: post
title:  Using gpg2 to exchange messages and files
Date: 2020-23-02 06:41  PM +05:30
---

* [What is gpg2 ?](#What is gpg2?)
* [Where and how gpg2 is used?](#Uses of gpg)
* [Create GPG2 keys](#Create GPG2 keys)
* [Generate a Revocation Certificate](#Generate a Revocation Certificate)
* [Exchange Public Keys](#Exchange Public Keys)
  * [Export Your Public Keys](#Export Your Public Keys)
  * [Import and Validate a Public Key](#Import and Validate a Public Key)
  * [Submit Your Public Key to a Key Server](#Submit Your Public Key to a Key Server)
* [Encrypt a Message](#Encrypt a Message)
* [Decrypt a Message](#Decrypt a Message)

## What is gpg2?
  * GNU Privacy Gaurd (GnuPG), Also Known as GPG, is a tool for secure communication  used to exchange encrypted files and messages between client machines using Public-Key Cryptography Methods.

## Uses of gpg
  * **Checking file Integrity**:-
   RStudio Signed Builds
    Official RStudio builds for Linux-based operating systems may be signed with a GnuPG key.
    **gpg2** can be used to validate the binary ourselves to
      * Ensure the binaries are genuine artifacts produces by RStudio, and
      * they have not been tampered with any third party during transmission

      Step 1: Obtaining the Public Key:-

        Importing RStudio Public Code-Signing Key from keysever using gpg2

       >  gpg2 --keyserver keys.gnupg.net --recv-keys 3F32EE77E331692F




      Step 2: Validating Build Signatures:-

        >    dpkg-sig --verify rstudio-download-1.2.3.deb

  ![key-import-image](/assets/gpg2/Validate-RStudio-Code-Signing.png)


  * **Sending Encrypted Messages and Files** * Encrypt Document to be sent


     > gpg2 --output encrypted-doc.gpg --encrypt --sign --armor --recipient tomcrse61@gmail.com --recipient kumar877prem@gmail.com doc-to-encrypt.txt

 Decrypt Document Recieved

  > gpg2 --output decrypted-doc --decrypt doc-to-decrypt.gpg

  * **Using gpg-key for ssh authentication**



##   Create GPG2 Keys

  1. Download and Install the most recent version of GPG Command Line Tool

      > sudo apt update
      > sudo apt install gnupg

  2. Create a new primary keypair

      > gpg2 --full-generate-keys

		***ScreenDump of the above command***

```
~/$ gpg2 --full-gen-key
gpg (GnuPG) 2.2.4; Copyright (C) 2017 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

~/$ Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
Your selection? 1
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (3072) 4096
Requested keysize is 4096 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
~/$ Key is valid for? (0) 10m
Key expires at Sat Jan 30 23:54:15 2021 UTC
~/$ Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: prem kumar
Email address: marzaanvan@gmail.com
Comment: NA
You selected this USER-ID:
    "prem kumar (NA) <marzaanvan@gmail.com>"

~/$ Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? o
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: key 8B49243047552028 marked as ultimately trusted
gpg: revocation certificate stored as '/home/ubuntu/.gnupg/openpgp-revocs.d/9900A9C62DE86CD2941774AD8B49243047552028.rev'
public and secret key created and signed.

pub   rsa4096 2020-04-05 [SC] [expires: 2021-01-30]
      9900A9C62DE86CD2941774AD8B49243047552028
uid                      prem kumar (NA) <marzaanvan@gmail.com>
sub   rsa4096 2020-04-05 [E] [expires: 2021-01-30]

```

  The system may require more entropy to generate the keypair, in a new shell session, install the `rng-utils` package:

  > sudo apt install rng-tools

  * Check and feed random data from an entropy source (e.g. hardware RNG device) to an entropy sink (e.g. kernel entropy pool) to provide the needed entropy for a secure keypair to be generated:

  > rngd -f -r /dev/urandom

  * Check the amount of entropy available on your Linode. The value should be somewhere near 3000 for keypair generation.

  > cat /proc/sys/kernel/random/entropy_avail

- Verify the keys on your public keyring:

  > gpg2 --list-keys

  The example output contains two public keys:
  ![keyring](/assets/gpg2/gpg-keys.png)

  Each value in the list represents the following information:

    1. Public key: `pub`

    2. Key size and type: `4096R`

    3. Short key ID: `A11C0F78`

    4. Creation date: `2018-08-02`

    5. Expiration date: `[expires: 2018-09-01]`

    6. User IDs: `exampleName2 (example comment) <user2@example.com>`

    7. Subkey: `sub`


## Generate a Revocation Certificate:-

  A revocation certificate is useful if you forget your passphrase or if your private key is somehow compromised. It is used to notify others that the public key is no longer valid. Create the revocation certificate immediately after generating your public key.

  Generate a revocation certificate. Replace `user@example.com` with the email address associated with the public key:

  > gpg2 --output revoke.asc --gen-revoke user@example.com

  * A prompt will ask you to select a reason for the revocation and provide an optional description. The default reason is recommended

  * The revocation certificate will be saved to the current directory as a file named `revoke.asc`. Save the certificate to a safe location on a different system so that you can access it in case your key is compromised in the future.

## Exchange Public Keys
  After Generating the keys , one needs to exchange the public keys in order to communicate securely. This can be done using two methods

  1. Importing the recipient public key manually
  2. Submitting the keys to the keyserver to be publically accessed any other client

## Export Your Public Key

  *  Export the public key using following command.Replace `public-key.gpg` with your choice of name and `user@example.com` with email associated with the public key.This will save the key in the current directory itself.

    > gpg2 --armour --output public-key.gpg --export user@example.com


  * Send the `public-key.gpg` file to the recipient in an email or copy and paste the contents of the `public-key.gpg` file.

  * The recipient should import the public key and validate it in order to use it to decrypt a message sent by you.

## Import and Validate a Public Key
One can add the recipient's public key to one's public keyring by importing it.The user public key must be first recieved using email or some other format, before it can be imported in the public ring

  * Once you’ve received the user’s public key and the `.gpg` file is saved to your local disk, import it to your public key ring

    > gpg2 --import public-key.gpg

  *  Verify that the public key has been added to your public key ring

    > gpg2 --list-keys

  *  Check the key’s fingerprint:
          The output will resemble the following

![fingerprint](/assets/gpg2/gpg-fingerprint.png)

Ask the owner of the public key to send you their public key’s fingerprint and verify that the fingerprint values match. If they match, you can be confident that the key you have added is a valid copy of the owner’s public key.

 > gpg2 --fingerprint marzaanvan@gmail.com


 When you have verified the public key’s fingerprint, sign the public key with your own key to officially validate it. Replace `user3@example3.com` with the associated email for the key you are validating:

Enter your passphrase when prompted.

    > gpg2 --sign-key user3@example3.com


*  View the public key’s signatures to verify that your signature has been added:

   > gpg2 --check-sigs user3@example3.com


   ![validate-sigs](/assets/gpg2/check-sigs.png)

*  You can export the signature to the public key and then send the signed copy back to the owner of the public key to boost the key’s level of confidence for future users:

  > gpg2 --output signed-key.gpg --export --armor user3@example3.com

  Send the signed key to the public key owner via email so they can import the signature to their GPG database


## Submit Your Public Key to a Key Server

Information about default keyserver is present in `~/.gnupg/gpg.cnf` file (most likely set as `hkp://keys.gnupg.net`), but any other source can be listed using `--keyserver` flag of gpg2 option. Following are the steps required to Submit a Public Key to a Key-Server

  * Find the long key ID for the public key you would like to send to the key server:

  >   gpg2 --keyid-format long --list-keys user@example.com

  Where , `user@example.com` is to be replaced with associated email-id of the key.

  One Will see Output similar to the below picture:

  ![long-key](/assets/gpg2/long-key-crop.png)

  The long key ID is the value after the key size `rsa4096` in the `pub` row. In the example the long key ID is `D536B962F4610A01`:

* To send your public key to the default key server use the following command and replace `keyid` with your public key’s long key ID:

  > gpg2 --keyserver server-name  --send-keys keyid

* Anyone can request your public key from the key server with the following command:

  > gpg2 --keyserver server-name  --recv-keys keyid

  The public key will be added to the user’s trust database using `trustdb.gpg` file.


## Exchanging Messages using gpg2

To Demonstrate this we'll take a text file `lorem-ipsum.txt` , encrypt it using `tomcrse61@gmail.com`'s gpg-key , send to my cs50-ide and decrypt both `.gpg`  files  to show the text content. We'll attach the screenshots for every step.

## Encrypt a Message/File

First we import `tomcrse61@gmail.com`'s public key , validate it by cross-checking their fingerprint and finally encrypt using their public-key , which finally will be decrypted using only thier own private key.

To Encrypt `lorem-ipsum.txt` :

  > gpg2 --output lorem-ipsum.gpg --encrypt --sign --armor --recipient tomcrse61@gmail.com -recipient kumar877prem@gmail.com lorem-ipsum.txt

This Will Produce `lorem-ipsum.gpg` as the encrypted file.

The extension `.gpg` is used for encrypted/binary data and `.asc` or `.sig` is used for detached or clearsign signatures. Including the `--armor` flag will encrypt the message in plain text.

## Decrypt a Message/File


A message will need to have been encrypted with your public key for you to able to decrypt it with your private key. Ensure that anyone that will be sending you an encrypted message has a copy of your public key.

To decrypt a message:

  > gpg --output lorem-ipsum.txt  --decrypt lorem-ipsum.gpg

  Decrypted file contents are same as of the original text file

  ![lorem-ipsum.txt](/assets/gpg2/lorem-ipsum.png)







