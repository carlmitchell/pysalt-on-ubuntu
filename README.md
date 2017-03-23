# pysalt-on-ubuntu
**Instructions for getting Python, IRAF, DS9, Pyraf, and PySALT set up on a fresh installation of Ubuntu 16.04**

1) First, we're going to install Python. I'm a fan of the "Anaconda" distribution available [here](https://www.continuum.io/downloads). You probably want version 2.X, not 3.X. Download the anaconda installer and follow the installation instructions on that website. The default options in the installer are pretty good and you probably don't want to change any of them.  
2) Next, we want to install IRAF, Pyraf, and DS9. There's a handy install script [here](http://www.astrosen.unam.mx/~favilac/IRAF/) that someone made for this purpose. The "16.04" folder is probably what you want. Download that installer and find it in your file explorer. Right click and choose Open With -> Disk Image Mounter. Then, in a terminal:
```
cd /media/{YourUserName}/IRAF for Ubuntu 16.04/
sudo sh install.sh
```
The default options for most of the questions it asks are all pretty good. Once that's all done, you can unmount the installation image (right click the drive in your sidebar and click "unmount") and delete the ISO file.  
3) Now we want to configure IRAF for your user account. In a terminal, execute the following:
```
cd ~
mkdir iraf
cd iraf
mkiraf
```
This will great a file called "login.cl" which is a startup script for IRAF linked to your account. Now there's a particular line of that file we want to comment out. This line causes IRAF to check for updates from the NOAO server every time it's opened. Unfortunately, if the NOAO servers are down, it makes IRAF refuse to load. Open the file for editing:
```
gedit chkupdate
```
Locate the line that reads:
```
chkupdate
```
And change it to:
```
#chkupdate
```
For me, it's line 142 in the file.  
4) There's a lot of other packages that PySALT needs (or supposedly needs) to work properly. From a terminal, execute the following:
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install gfortran liblapack-dev libblas-dev g++ libfreetype6-dev  tk8.5 tk8.5-dev tcl8.5 tcl8.5-dev texlive dvipng tk libqt4-dev lsb-release tcsh sharutils lynx libc6-dev libncurses5-dev libf2c2 python-urwid libx11-dev libcfitsio3-dev pgplot5 libxt-dev libxaw7-dev openjdk-6-source openjdk-6-jre libmotif-dev unzip fort77 cfortran gv cups-bsd cups-common cups mc saods9
pip install --upgrade pip
pip install pyfits pyds9 pyspectrograph aplpy pyraf pyephem astropy ccdproc pywcs
conda install pyqt=4
```
5) Now we want to install pysalt. From a terminal:
```
wget http://pysalt.salt.ac.za/versions/pysalt.nightly.tar.gz
tar zxf pysalt.nightly.tar.gz
sudo mv pysalt /iraf/iraf/extern/
sudo gedit /iraf/iraf/unix/hlib/extern.pkg
```
Now you're editing the package configuration file for IRAF. We need to add a few lines to tell it about PySALT. In the first section, add two lines that look like:
```
reset 	pysalt 		= extern$pysalt/
reset 	pysalt.pkg 	= pysalt$pysalt.cl
```
And in the second section, add this line:
```
			  ,pysalt$lib/helpdb.mip\
```
And then save the file. Now IRAF is configured to know about PySALT!
6) Finally, let's test to see if things worked. In a terminal, type:
```
pyraf
```
This should open a PyRAF interactive terminal. In that terminal, type:
```
pysalt
saltred
saltspec
proptools
salthrs
.exit
```
If all of that proceeded without any errors, you're good to go!