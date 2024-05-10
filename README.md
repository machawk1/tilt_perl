# tilt_perl 

This is a Tilt Hydrometer application written in Perl/Tk, which will read bluetooth signals, parse raw Tilt data, and log the information to a third party URL, such as [Brewfather](https://brewfather.app/). This is similar to the [Tilt Pi](https://tilthydrometer.com/collections/tilt-pi/products/tilt-pi-raspberry-pi-disk-image-download) software offered by [Tilt](https://tilthydrometer.com/).

# Background
I wanted avoid the need to flash my Raspberry Pi SD card to run the Tilt Pi web server, and wanted to make custom tweaks and features.

# System Requirements
This program is only written and tested with Unix/Linux operating systems in mind. Support with Microsoft Windows may be possible with third party Perl installations, such as [Strawberry Perl](https://strawberryperl.com/), but no testing or guarantees can be given.

* A bluetooth radio antenna is required to take BLE readings
  * built-in or external antennas will work.
* several third party software installations are required, as outlined in the installation instructions below.

# Reference
* Tilt hydrometer: https://tilthydrometer.com/
* Tilt iBeacon data format: https://kvurd.com/blog/tilt-hydrometer-ibeacon-data-format/
* Tilt iBeacon git python libraries: https://github.com/frawau/aioblescan  (not used here)

# Installation

Install hcidump for reading raw bluetooth advertised packet data

`sudo apt-get install hcidump`

Install Perl-Tk for Perl GUI toolkit

`sudo apt-get install perl-tk`

Install cpanm in order to easily install CPAN Perl modules (CPAN itself should be installed by default)

`sudo cpan App::cpanminus`

Install LWP::UserAgent module to make HTTP requests (note this will also install a LOT of (good) dependencies)

`sudo cpanm LWP::UserAgent`

If you want to run hcitool and hcidump as a normal user, grant the appropriate capabilities to the executables, then remove the 'sudo' calls in the perl script. Note this may have security implications.

```
sudo setcap 'cap_net_raw,cap_net_admin+eip' `which hcitool`
sudo setcap 'cap_net_raw,cap_net_admin+eip' `which hciconfig`
```

# Usage

1. In a terminal window, execute the program
`tilt.pl`

2. A generic "searching" screen is displayed until a Tilt is detected
   - If nothing appears within 30 seconds or so, make sure your Tilt is on, floating, and in range.
   - Also make sure there is no obvious source of bluetooth interference, and that your device's bluetooth is functioning in general

3. The Tilt status will appear in place of the search window once a Tilt device is found.
   - data is updated automatically in real time.

4. To start URL web-end logging:
   1. `Configure => Logging => <color> => Setup Log`
   2. In the new popup window, enter the URL end-point, a logging interval (in minutes, minimum 15), and a beer name
   3. Click START
   4. Any errors will be displayed in the "Status:" area

5. To add a calibration offset:
   1. `Configure => Calibrate => <color> => <cal choice>`
   2. Manual calibrations will open a popup window to enter the offset
   3. Note: only a single calibration offset is currently supported. device level calibration in prepared sugar solutions is recommended
   4. Note: calibration will be applied upon the next subsequent reading

6. Any logging or calibration settings will be automatically saved to a config.ini file in the local directory, then automatically loaded on the next program start. The settings saved are color-specific and can support multiple devices.

# CONTACT
irontonbrewing@gmail.com
