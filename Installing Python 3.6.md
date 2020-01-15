# Installing Python 3.6
* Ok so Python 3.5 comes pre-installed with OSMC, But LEDfx requires Python 3.6+, So we have to install Python 3.6. 
WARNING: This process takes a long time, depending on Raspberry Pi. Took 40+ mins for me. So be mindful.
Source below but I will summarise what I did.
* Prerequisites
* `sudo apt-get install build-essential checkinstall`
* `sudo apt-get install libreadline-gplv2-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev`
* Download Python 3.6
* `cd /usr/src`
* `sudo wget https://www.python.org/ftp/python/3.6.9/Python-3.6.9.tgz`
* `sudo tar xzf Python-3.6.9.tgz`
* Compile Python Source
* `cd Python-3.6.9`
* `sudo ./configure --enable-optimizations` (Takes a while)
* `sudo make install` (Takes even longer) (For my installation I chose to use normal install instead of `sudo make altinstall`. Normal install replaces Python 3.5 with Python 3.6. I did that because I didn't want to deal with two Python installations. But if you know/want to install Python 3.6 along side Python 3.5 then use `altinstall`. Just keep in mind that commands may be different for the following if you do. With that said, I have not run into any issues yet with a python 3.6 install on OSMC.
* You can test if you have Python 3.6 by using `python3 -v` or `python3.6 -v` for alt install
Source: https://tecadmin.net/install-python-3-6-ubuntu-linuxmint/
