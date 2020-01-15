# ahodges9 Dev Branch

Just keep in mind that at the time of writing these instructions, The dev Branch was the most supported. Things can change and may not be the most support version. Refer to [Readme.md](/README.md) for whats recommended.

* We are using ahodges9 (https://github.com/ahodges9/LedFx/tree/dev) as this is currently the most recent 15/01/2020. You are free to use the currently outdated master branch however this may be the most up to date version in the future.
* You are free to simply use adhodges master version by using `pip3 install ledfx` after doing `sudo pip3 install --upgrade pip`. If so you can skip to [Step 6](/Starting%20and%20Configuring%20LEDfx.md) 


* We need to install GIT with `sudo apt-get install git`
* We clone Mattallmighty's branch with `git clone https://github.com/Mattallmighty/LedFx.git`

* We have to update pip since we updated Python with `sudo pip3 install --upgrade pip`
* Then we need to install setuptools that will build a python program `pip3 install -U setuptools`
* This is a prerequisite required for linux LEDfx `sudo apt-get install portaudio19-dev`
* We then go into the directory with `cd LedFx`
* `pip3 install -r requirements.txt` to install any other requirements required for ledfx.
* `sudo pip3 install .` to install/Create LEDfx(The dot is included)

* Before starting we have to delete fadecandy python file due to error when starting ledfx (This may not be a needed step in the future, You can test with simply trying to launch ledfx and looking if fadecandy.py causes an error)
* `cd /usr/local/lib/python3.6/site-packages/ledfx/devices/`
* `sudo rm fadecandy.py`

[Final step, Starting and Configuring LEDfx](/Starting%20and%20Configuring%20LEDfx.md)
