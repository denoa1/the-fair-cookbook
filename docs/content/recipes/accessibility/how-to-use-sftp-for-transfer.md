
# How to use SFTP to transfer data files between collaborating institutions.

---
recipe_metadata:
  - identifier: "none"
  - version: 1.0
difficulty_level: 3
reading_time_in_minutes: 20
type_of_document: "hands on, step-by-step recipe"
document_contains_executable_code: false
intended_audience:
  - "principal investigators"
  - "data managers"
  - "data scientists"
  - "funders"
---

## Abstract

Collaborating teams at two or more organizations often need to transfer and share data files. There are a number of ways to share files, all with various degrees of ease and usability.
The particular information security risk management (ISRM) protocols at the sending and receiving institutions need to be considered when one chooses and optimizes file-transfer solutions.
One common method for transferring files is SFTP or scp (secure copy).
[final sentence missing]


## Background info
- SFTP: Secure or SSH File Transfer protocol is a standard way to transfer files securely using a remote server.
- SFTP is not to be confused with FTP or FTPS. While FTP does not use encryption at all and therefore can be considered insecure, FTPS adds a layer of encryption on top of FTP but it still comes with a number of drawbacks from the FTP protocol, e.g. requiring a range of open ports. SFTP uses an entirely different protocol based on SSH (secure shell) and uses strong encryption for authentication information as well as the data transferred.
- In order to upload and download files, the client needs to communicate with the server over port 22 (which is the default port for SFTP) and the network configurations on the sender as well as the recipient side need to allow this traffic. If network restrictions block this communication, one might try to run the SFTP server on a different port (e.g. 443).
- In this scenario, a SFTP server is a pure file transfer server, i.e. it lives outside of any sensitive network area and both parties (the sender and recipient) need to use a SFTP client to upload from and download to their internal storage systems. After transfer and integrity check, files would be typically removed by the receiver.


## Graphical Overview:

not existent


## Requirements:

For client (receiver/sender):
-	Basic understanding of SFTP client configurations
-	(optional) Basic programming skills to automate upload or download process

For server (system administrator):
-	Compliance with company IT-security policies
-	Understanding firewall configurations
-	Ability to use terminal (bash)


## Recipe instructions:

Overview:

- (1)	Setting up SFTP server
  - 1.a.	Instructions
  - 1.b.	Security considerations
- (2)	Data upload/download
  - 2a.	Manual
  - 2b.	Automatic
- (3) Correctness and completeness of transfer

### (1)	Setting up a SFTP server

While you can run an SFTP server also in a Windows environment (e.g. using the open source software FileZilla Server), a Linux server is certainly recommended. Most Linux distributions come with all required libraries (libssh2, OpenSSH) pre-installed. Following is a step-by-step summary for a CentOS server:

a.	Create a dedicated group for all future SFTP users:
```
$ groupadd sftpusers
```

b.	First create a folder on a volume with sufficient free space:
```
$ mkdir -p /data/sftp
```

c.	Set permissions:
```
$ chown root:sftpusers /data/sftp
$ chmod 775 /data/sftp
```

d.	Create one or more SFTP users, assigning them to the previously created group:
```
$ useradd -g sftpusers -d / -s /sbin/nologin USERNAME
```

e.	Set the password for the new user:
```
$ passwd USERNAME
```

f.	Edit the SSHD configuration at /etc/ssh/sshd_config (e.g. using vi or nano) by adding the following lines:
```
Match Group sftpusers
ChrootDirectory /data/sftp
ForceCommand internal-sftp
```

g.	Restart the SSH services
```
$ service sshd restart
```

h.	Now you have to make sure you open port 22 in your network to the outside world under a specific domain name or static IP address.


### (2) Data upload and download

#### 2.a. Manual, i.e. drag'n'drop

Data could be transferred to/from SFTP server using multiple clients. Here there are some examples:

FileZilla

OS: Windows, Mac OS, Linux

License: Free Software (GPL)

Pros:
- easy to setup;
- portable version available (no installation, i.e. administrator rights, required)
- cross-platform

Cons:
-	By default installs adware


WinSCP

OS: Windows

License: Free Software (GPL)

Pros:
- easy to setup;
- portable version available (no installation, i.e. administrator rights, required)

Cons:
-	Only Windows
-	No x64 version (as of 07.07.2020)


Other SFTP clients: Cyberduck, MonstaFTP (Free and paid) and many others

Tipp:
The portable version of WinSCP can be preconfigured so that a user only needs to enter the password, without requiring knowledge of host names, protocols, ports or user name!


#### 2.b. Automatic

Libraries implementing SFTP are available for different programming languages.

- Python – pysftp (https://pysftp.readthedocs.io/en/release_0.2.8/index.html#)
- Perl – Net::SFTP (https://metacpan.org/pod/Net::SFTP)
- JavaScript - ssh2-sftp-client (https://www.npmjs.com/package/ssh2-sftp-client)
- Bash – sftp (similar to scp)


### (3) Correctness and completeness of transfer

It is a good practice to ensure that file transfer is correct and complete. See recipe on checksum creation and validation.

TODO: Kick out the following block:
---
Sender should calculate checksum (md5, sha512, etc) for every file:
bash: md5sum * > md5sum.txt
or
bash: sha512sum * > sha512sum.txt
or
Windows: CertUtil -hashfile FILENAME MD5
Recipient compares checksums:
Bash: md5sum -c md5sum.txt *
Or
Bash: sha512sum -c sha512sum.txt *
---


The sender can use the sender organization’s HPC node to
- (1)	set up a shell which runs in the background,
(- 2)	launch the FTP session in the same local network as the server and directory of files to be transferred.
- (3)	Transfer the files via the filesystem on both the local and remote system

For example : An IMI collaboration project requires transfer and sharing of a number of  image data folders,  each approximately ~300-500 GBThe process involved copying the files over to a secure FTP server, the receiving institution copies to their server, then the sender deletes the files on the FTP server.

Pros and cons:
- Double copy process with an intermediate space
- Cumbersome
- Works for mid size data (Gigabyte range)
- It works in most cases, especially if the file transfer is “one-time” batch of files.
- It can be considered a good short term or “one-off” solution.

This common process is described in a number of publically available resources, examples in the Further Reading section below.


## 4. Possible improvements from the state of this recipe:
- One could provide for increased automation by writing a small script to iterate through each directory when one is transferring a set of directories, each containing a number of data files.


## 5. Further reading:

- Wikipedia article on SFTP: https://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol
- The Geek Stuff, FTP and SFTP Beginners guide with 10 examples https://www.thegeekstuff.com/2010/06/ftp-sftp-tutorial/
- Example of customization for a specific institution:
  - https://hpc.uni.lu/users/docs/filetransfer.html
- https://www.howtoforge.com/tutorial/how-to-setup-an-sftp-server-on-centos/


## 6. Capability and Maturity Table.

Capability Initial Maturity Level Final Maturity Level
Interoperability – minimal - repeatable


## 7. [FAIRification Objectives, Inputs and Outputs](#FAIRification Objectives, Inputs and Outputs)

COMMENT: the concepts in this recipe did not map to any terms from the EDAM ontology.


## 8.Table of Data Standards

COMMENT: the concepts in this recipe did not map to any terms from the FAIRsharing.org database.


## 9.Authors:

| Name           | Affiliation    | orcid          | CrediT role   |
| :------------- | :------------- | :------------- |:------------- |
| Dorothy S. Reilly | Novartis | | Writing - Original Draft |
| Vitaly Sedlyarov  | CeMM     | | Writing - Original Draft |
| Ulrich Goldmann   | CeMM     | | Writing - Original Draft |


## 10.License:

(https://creativecommons.org/licenses/by/4.0/)
