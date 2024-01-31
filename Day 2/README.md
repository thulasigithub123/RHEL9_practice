

RHEL 9 Administration
=====================

Day 2: SSH
----------

SSH, which stands for Secure Shell, is a protocol used to securely access and manage remote machines.

### Configurations:

By default, the root profile is disabled, so to enable that, edit the configuration file:

`/etc/ssh/sshd_config`

Set the parameter `PermitRootLogin` to `yes`. This change has to be made in both the server and client.

To have the change in effect, restart the SSH service:

`systemctl restart sshd`

To test SSH:

`ssh clienthost` (authentication with password)

#### For Passwordless Authentication:

From the server, generate a public and private key pair:

`ssh-keygen -t rsa` (type RSA standard)

This will create a public and private key in `/root/.ssh/`.

Then, copy the public key identification file to the remote machine with authentication:

`ssh-copy-id -i /root/.ssh/id_rsa.pub root@client`

Similarly from the client to the server.

Now to test the changes, SSH client will authenticate without a password.

It is always advisable to disable "login with password".

In the file, update the parameter:

`vi /etc/ssh/sshd_config`  
`PasswordAuthentication to no`  
`systemctl restart sshd`

* * *

COCKPIT
-------

Cockpit is a web-based server management tool. Cockpit provides a user-friendly interface for managing Linux servers, and it allows you to perform various administrative tasks without the need for a command-line interface. (default port is: 9090)

Check if Cockpit is installed:

`dnf list cockpit`

Check if Cockpit is enabled:

`systemctl status cockpit`

Check if firewall rule is allowed for Cockpit:

`firewall-cmd --list-services`

To administer from the web-based interface:

*   Open a web browser
*   Search for: `http://[servername/ip]:9090/`

By default, root users are not allowed for Cockpit interface. To enable it:

`vi /etc/cockpit/disallowed-users`  
`Remove the root entry and save the changes`  
`Restart Cockpit and enable`

* * *

Registering our machine to Red Hat
----------------------------------

`subscription-manager` is a command-line tool used in Red Hat Enterprise Linux (RHEL) to manage subscriptions and entitlements. It is a part of the Red Hat Subscription Management (RHSM) system, which helps organizations manage their subscriptions for Red Hat products and services.

*   Create an account in [https://developers.redhat.com](https://developers.redhat.com)
*   `subscription-manager register`
*   Authenticate to register

This will set up the Red Hat repository. Then try installing `git` / `ansible-core`, so it will be downloaded from the Red Hat online repository.

To unregister the system from Red Hat Subscription Management:

`subscription-manager unregister`

* * *

Creating Custom Yum Repository
------------------------------

RHEL9 is shipped with 2 main repositories:

1.  BaseOS
2.  AppStream

From the VirtualBox settings, set the boot order to have the hard disk at first and select the RHEL ISO.

Then, to list the block storage devices:

`lsblk -fs`

Copy the UUID of the RHEL ISO. Then create a directory to mount:

`mkdir /dvd1`

To mount, edit `/etc/fstab` and add the entry:

`UUID=XXXX /dvd1 iso9660 loop,ro,auto 0 0`

To save the system changes:

`systemctl daemon-reload`  
`mount -a`

We can now see the ISO files mounted under `/dvd1`.

`yum` is replaced by `dnf` (Dandified YUM), so the `yum` execution is actually due to the soft link to `dnf`.

To check `yum`:

`which yum`  
`ls -l /usr/bin/yum`

To list all the repositories:

`dnf repolist`

And to see the configuration:

`cd /etc/yum.repos.d/`  
`vi swrepo.repo`

    \[baseos\]
    name=baseosrepo
    enabled=1
    baseurl=file:///dvd1/BaseOS
    gpgcheck=0     #to skip signature verification

    \[appstream\]
    name=appstreamrepo
    enabled=1
    baseurl=file://dvd1/Appstream
    gpgcheck=0
    

Then, these will be listed as repos, `dnf repolist`

To test this, install `git` or another sample package, so it will be downloaded from our custom repository:

`dnf install git`  
`git --version`