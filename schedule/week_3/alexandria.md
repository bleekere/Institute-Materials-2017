# Configuring Alexandria

_Alexandria_ is a text repository and database that supports Text As Graph (TAG). We’ll use it in the Institute to gain perspective on modeling by exploring non-XML structured text.

## What is it?
In short: Alexandria is a text repository system in which you can store and edit documents. It is the reference implementation of TAG (Text-as-Graph), a flexible graph data model for text. TAG and Alexandria are under active development at the Research & Development group of the Humanities Cluster.

## Why should I use it?
You may wonder, why use Alexandria if you already have a text editing tool? Well, if you'd like to carry out advanced text analysis and you don't mind a little command-line work, Alexandria is the tool for you. If you're used to working with XML, you'll find it especially enlightening to work with a data model like TAG in which you can easily model overlapping structures, discontinuous elements, and nonlinear text without having to resort to workarounds.

## Installation
### 1. Download
An up-to-date version of _Alexandria_ can be downloaded from <https://cdn.huygens.knaw.nl/alexandria/alexandria-app.zip>

### 2. Unpack the zip
Unpack the zip to a new directory of your choice. Remember the path to that directory. Now you have to make sure that your machine can always find the `bin` directory that contains the _Alexandria_ code when you call it. You have three options:

#### 2a. Create a permanent alias in your .bash_profile
Open your .bash_profile. If you're on a Unix machine, you can type `open -a "Sublime Text" ~/.bash_profile` in your terminal window. This will open your bash_profile in the Sublime Text editor (of course you can use an editor of your choice).  

You can create an alias for _Alexandria_ by writing `alias alexandria="<path to alexandria>"`. For instance, your alias could say `alias alexandria="/Users/alexandria-markup-server/bin/alexandria"`. Save and close your bash_profile. Before the alias works, you have to resource the bash_profile: type `source ~/.bash_profile` in your terminal.

#### 2b. Add the directory your `PATH`
In your terminal window, type: 
```
export PATH=$PATH:<path to alexandria>
```
For example:
```
export PATH=$PATH:/Users/alexandria-markup-server/bin/alexandria
```
if that's where you've stored _Alexandria_. You can check if it works by typing
```
echo $PATH
```
in your terminal window. It should return something like the following, with the path to _Alexandria_ directory newly added at the end:
```
/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/Users/alexandria-markup-server/bin/alexandria
```

#### 2c. Create a softlink
If you don't like to change your path, you can create a softlink.

A soft link (also known as a symbolic link or symlink) consists of a special type of file that serves as a reference to another file or directory. You can create them on your command line: 
```
$ ln -s {source-filename} {symbolic-filename}
```

For example: 
```
$ ln -s /Users/alexandria-markup-server/bin/alexandria /usr/local/bin/alexandria
```

Verify if it works by running 
```
$ ls -l /usr/local/bin/alexandria
```

Your output will look something like:
```
lrwxr-xr-x  1 veryv  wheel  5 Mar  7 22:01 alexandria -> Users/alexandria-markup-server/bin/alexandria
```
Notice the `->` that indicates the link between the link name and the file.

### Working with _Alexandria_

We created a [tutorial](https://huygensing.github.io/alexandria-markup-server/tutorial/). The tutorial takes the form of a [Jupyter Notebook](http://nbviewer.jupyter.org/github/DiXiT-eu/collatex-tutorial/blob/master/unit1/Jupyter_notebook.ipynb). The notebook contains blocks of text and small snippets of code: commands that you give to your version of Alexandria. You can run these commands from within the notebook. The notebook, in other words, is a secure environment for you to play around with and get to know Alexandria.


<!---

NOTE: This is an OLD VERSION of the installation instructions: we no longer use Docker. It is kept here for historical purposes.
1. Install Docker
	* _Mac (Yosemite 10.10.3 or later)_: Docker CE (community edition) for Mac from <https://store.docker.com/editions/community/docker-ce-desktop-mac>
	* _Mac (Older than Yosemite 10.10.3)_: Docker toolbox for Mac <https://download.docker.com/mac/stable/DockerToolbox.pkg>
	* _Windows 10 Professional or Enterprise_: Docker CE for Windows <https://docs.docker.com/docker-for-windows/install/>. **Please note that Hyper-V *should only be enabled* which Docker does for you at installation time. It is probably that you do not have _Virtualization_ enabled in your BIOS if there is an error about this. In BIOS it could be named _VT-X_/_AMD-v_ or simply _Virtualization_. 
	* _Windows 10 Home, Windows 8, Windows 7_: Docker Toolbox: this installs Docker into a GNU/Linux system in a VirtualBox <https://docs.docker.com/toolbox/toolbox_install_windows> (direct link <https://download.docker.com/win/stable/DockerToolbox.exe>)
	* _GNU/Linux_: Install Docker via your package manager.

2. Create a directory and copy the following file (which must be called `docker-compose.yml`) into it:

```
version: '3'
services:
  # the alexandria server
  alexandria:
        image: huygensing/alexandria-markup-server:latest
        ports:
          - 8002:8080
          - 8003:8081
        environment:
          - BASE_URI=http://localhost:8002

  # the tex-util server, which can convert the LaTeX from the alexandria server to SVG)
  latex:
        image: huygensing/tex-util-server:latest
        ports:
          - 8000:8080
          - 8001:8081
        depends_on:
          - alexandria
        environment:
          - BASE_URI= http://localhost:8000

  # the relevant notebooks and python code to connect to the alexandria and latex services
  notebook:
        image: huygensing/alexandria-markup-notebook:latest
        ports:
          - 8888:8888
        depends_on:
          - latex
          - alexandria
        volumes:
          # Your work notebooks will be stored here
          - /Users/djb/docker:/work
```

1. Change the last line to specify your own workspace by replacing the “djb” with your own userid. If not on Docker CE (Modern Macs, Windows Entreprise) and on Docker Toolbox instead (old Macs, Windows Home) also change the two occurrences of “localhost“ to the ip address of your Docker. 

2. Create a subdirectory called `docker` under your home directory.

1. If you are using ports 8000, 8001, 8002, 8003, or 8888 on your machine for other purposes (you probably aren’t), you need to change the ports Alexandria uses. The only numbers you have to change are for the ports you are using, and only in the following two types of situations:

	1. The numbers after the colons in the lines that set a value for `BASE_URI`.

	1. The numbers *before* the colons in the lines that have two port numbers separated by a colon. Do not change the numbers *after* the colons in these lines, even if you are using ports with those numbers for other purposes.

1. In the directory where you saved `docker-compose.yml`, run:

	```
	docker-compose pull && docker-compose up
	```

1. In a web browser, navigate to either <http://localhost:8888> or <http://ipaddress.of.your.docker:8888> (This is an ip address starting with either 192. or 172. E.g. 172.17.0.1).
1. Click on the “examples” directory and then on the notebook “markup-init.ipynb”. 
1. When on a modern Mac and running Docker CE change the server_url and latex_server in the *first cell* of the notebook to "http://docker.for.mac.localhost:8002" and "http://docker.for.mac.localhost:8000/" respectively.
1. When not on Docker CE and on Docker Toolbox instead if required also *change* the ip/host address to that of your docker as above in the *first cell* of the notebook.
1. Run the notebook from the menu bar with “Cell” → “Run All”.

-->


