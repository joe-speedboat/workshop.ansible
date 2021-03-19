# Ansible HandsOn Workshop

![enter image description here](https://github.com/joe-speedboat/workshop.ansible/raw/master/images/ansible_logo.png)

![enter image description here](https://github.com/joe-speedboat/workshop.ansible/raw/master/images/devops.png)



## History of Ansible
* Ansible was written by Michael DeHaan in 20212
* Acquired by Red Hat in 2015

## Design
* **Minimal in nature**
Management systems should not impose additional dependencies on the environment.

* **Consistent**
With Ansible one should be able to create consistent environments.

* **Secure**
Ansible does not deploy agents to nodes. 
Only OpenSSH and Python are required on the managed nodes.
Windows systems use WinRM ans PowerShell.

* **Highly Reliable**
When carefully written, an Ansible playbook can be idempotent, to prevent unexpected sideeffects on the managed systems.
It is entirely possible to have a poorly written playbook that is not idempotent.
	* Idempotent
On/Off buttons of a train's destination sign control panel. Pressing the On button (green) is an idempotent operation, since it has the same effect whether done once or multiple times. Likewise, pressing Off is idempotent.

* **Minimal learning required**
Playbooks use an easy and descriptive language based on YAML and Jinja templates.

## Architecture

![enter image description here](https://github.com/joe-speedboat/workshop.ansible/raw/master/images/ansible_architecture.png)
### Ansible Setup
To install Ansible, we need a CentOS 8 or Red Hat Enterprise Linux 8 vm.
Log into the vm with ssh as root and install Ansible:

    dnf install git epel-release wget curl ansible vim-enhanced nano
Now we do some basic configuration to suite our needs

    cp -av /etc/ansible/ansible.cfg /etc/ansible/ansible.cfg.orig
    ansibleconfigfile="/etc/ansible/ansible.cfg"
    test -f $ansibleconfigfile || cp -av $ansibleconfigfile ${ansibleconfigfile}.orig
    sed -i 's|^#inventory .*|inventory      = /etc/ansible/hosts|g' $ansibleconfigfile
    sed -i 's|^#roles_path .*|roles_path    = /etc/ansible/roles|g' $ansibleconfigfile
    sed -i 's|^#remote_user .*|remote_user = root|g' $ansibleconfigfile
    sed -i 's|^#log_path .*|log_path = /var/log/ansible.log|g' $ansibleconfigfile
    sed -i 's|^#nocows .*|nocows = 1|g' $ansibleconfigfile
    sed -i "/^roles_path/a\ \n#additional paths to search for collections in, colon separated\ncollections_paths = /etc/ansible/collections" $ansibleconfigfile
    
    test -d /etc/ansible/projects || mkdir /etc/ansible/projects ; chmod 700 /etc/ansible/projects
    test -d /etc/ansible/collections || mkdir /etc/ansible/collections ; chmod 755 /etc/ansible/collections

Now we look for the changes we made above



### Ansible Files
#### cfg
#### inventory
#### playbook


### Ansible Comands

### Ansible Projects



StackEdit stores your files in your browser, which means all your files are automatically saved locally and are accessible **offline!**

## Create files and folders

The file explorer is accessible using the button in left corner of the navigation bar. You can create a new file by clicking the **New file** button in the file explorer. You can also create folders by clicking the **New folder** button.

## Switch to another file

All your files and folders are presented as a tree in the file explorer. You can switch from one to another by clicking a file in the tree.

## Rename a file

You can rename the current file by clicking the file name in the navigation bar or by clicking the **Rename** button in the file explorer.

## Delete a file

You can delete the current file by clicking the **Remove** button in the file explorer. The file will be moved into the **Trash** folder and automatically deleted after 7 days of inactivity.

## Export a file

You can export the current file by clicking **Export to disk** in the menu. You can choose to export the file as plain Markdown, as HTML using a Handlebars template or as a PDF.


# Synchronization

Synchronization is one of the biggest features of StackEdit. It enables you to synchronize any file in your workspace with other files stored in your **Google Drive**, your **Dropbox** and your **GitHub** accounts. This allows you to keep writing on other devices, collaborate with people you share the file with, integrate easily into your workflow... The synchronization mechanism takes place every minute in the background, downloading, merging, and uploading file modifications.

There are two types of synchronization and they can complement each other:

- The workspace synchronization will sync all your files, folders and settings automatically. This will allow you to fetch your workspace on any other device.
	> To start syncing your workspace, just sign in with Google in the menu.

- The file synchronization will keep one file of the workspace synced with one or multiple files in **Google Drive**, **Dropbox** or **GitHub**.
	> Before starting to sync files, you must link an account in the **Synchronize** sub-menu.

## Open a file

You can open a file from **Google Drive**, **Dropbox** or **GitHub** by opening the **Synchronize** sub-menu and clicking **Open from**. Once opened in the workspace, any modification in the file will be automatically synced.

## Save a file

You can save any file of the workspace to **Google Drive**, **Dropbox** or **GitHub** by opening the **Synchronize** sub-menu and clicking **Save on**. Even if a file in the workspace is already synced, you can save it to another location. StackEdit can sync one file with multiple locations and accounts.

## Synchronize a file

Once your file is linked to a synchronized location, StackEdit will periodically synchronize it by downloading/uploading any modification. A merge will be performed if necessary and conflicts will be resolved.

If you just have modified your file and you want to force syncing, click the **Synchronize now** button in the navigation bar.

> **Note:** The **Synchronize now** button is disabled if you have no file to synchronize.

## Manage file synchronization

Since one file can be synced with multiple locations, you can list and manage synchronized locations by clicking **File synchronization** in the **Synchronize** sub-menu. This allows you to list and remove synchronized locations that are linked to your file.


# Publication

Publishing in StackEdit makes it simple for you to publish online your files. Once you're happy with a file, you can publish it to different hosting platforms like **Blogger**, **Dropbox**, **Gist**, **GitHub**, **Google Drive**, **WordPress** and **Zendesk**. With [Handlebars templates](http://handlebarsjs.com/), you have full control over what you export.

> Before starting to publish, you must link an account in the **Publish** sub-menu.

## Publish a File

You can publish your file by opening the **Publish** sub-menu and by clicking **Publish to**. For some locations, you can choose between the following formats:

- Markdown: publish the Markdown text on a website that can interpret it (**GitHub** for instance),
- HTML: publish the file converted to HTML via a Handlebars template (on a blog for example).

## Update a publication

After publishing, StackEdit keeps your file linked to that publication which makes it easy for you to re-publish it. Once you have modified your file and you want to update your publication, click on the **Publish now** button in the navigation bar.

> **Note:** The **Publish now** button is disabled if your file has not been published yet.

## Manage file publication

Since one file can be published to multiple locations, you can list and manage publish locations by clicking **File publication** in the **Publish** sub-menu. This allows you to list and remove publication locations that are linked to your file.


# Markdown extensions

StackEdit extends the standard Markdown syntax by adding extra **Markdown extensions**, providing you with some nice features.

> **ProTip:** You can disable any **Markdown extension** in the **File properties** dialog.


## SmartyPants

SmartyPants converts ASCII punctuation characters into "smart" typographic punctuation HTML entities. For example:

|                |ASCII                          |HTML                         |
|----------------|-------------------------------|-----------------------------|
|Single backticks|`'Isn't this fun?'`            |'Isn't this fun?'            |
|Quotes          |`"Isn't this fun?"`            |"Isn't this fun?"            |
|Dashes          |`-- is en-dash, --- is em-dash`|-- is en-dash, --- is em-dash|


## KaTeX

You can render LaTeX mathematical expressions using [KaTeX](https://khan.github.io/KaTeX/):

The *Gamma function* satisfying $\Gamma(n) = (n-1)!\quad\forall n\in\mathbb N$ is via the Euler integral

$$
\Gamma(z) = \int_0^\infty t^{z-1}e^{-t}dt\,.
$$

> You can find more information about **LaTeX** mathematical expressions [here](http://meta.math.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference).


## UML diagrams

You can render UML diagrams using [Mermaid](https://mermaidjs.github.io/). For example, this will produce a sequence diagram:

```mermaid
sequenceDiagram
Alice ->> Bob: Hello Bob, how are you?
Bob-->>John: How about you John?
Bob--x Alice: I am good thanks!
Bob-x John: I am good thanks!
Note right of John: Bob thinks a long<br/>long time, so long<br/>that the text does<br/>not fit on a row.

Bob-->Alice: Checking with John...
Alice->John: Yes... John, how are you?
```

And this will produce a flow chart:

```mermaid
graph LR
A[Square Rect] -- Link text --> B((Circle))
A --> C(Round Rect)
B --> D{Rhombus}
C --> D
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNjI2ODI0NzgsMTYyMTc2Mjc3NiwtMT
I3MjMyMDQ0NCwtMjQxNDk0MzM3XX0=
-->