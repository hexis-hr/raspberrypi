#Raspberry PI emoncms module

This module is to be used with an emoncms installed on the PI to interface with an RFM12 to PI board in a seemless fashion.

##Features:
- set the emoncms account to send data to
- set the RFM12 frequency
- set the RFM12 group
- set the RFM12 board node id.
- gateway scripts to forward data from serial port to local and remote emonCMS

##Installation:

Install one of the two available gateway scripts to let them run on startup

1/ Legacy raspberry_run.php

  The php script is run every minute via cron daemon.

  Open crontab using nano or any other editor

  $ sudo nano /etc/crontab

  Add following line at the end of the file:

  */1 * * * * root cd /var/www/emoncms/Modules/raspberrypi && php raspberrypi_run.php

  Then reboot

  $ sudo reboot

2/ RFM2Pi Gateway (python script)

  Create groupe emoncms and make user pi part of it

  $ sudo groupadd emoncms
  $ usermod -a -G emoncms pi

  Create a directory for the logfiles and give ownership to user pi, group emoncms

  $ sudo mkdir /var/log/rfm2pigateway
  $ sudo chown pi:emoncms /var/log/rfm2pigateway
  $ sudo chmod 750 /var/log/rfm2pigateway

  Make script run as daemon on startup

  $ sudo cp rfm2pigateway.init.dist /etc/init.d/rfm2pigateway
  $ sudo chmod 755 /etc/init.d/rfm2pigateway
  $ update-rc.d rfm2pigateway defaults 99

  The gateway can be started or stopped anytime with following commands:

  $ sudo /etc/init.d/rfm2pigateway start
  $ sudo /etc/init.d/rfm2pigateway stop
  $ sudo /etc/init.d/rfm2pigateway restart

##Requires the pecl php serial module

- sudo apt-get install php-pear php5-dev
- sudo pecl install channel://pecl.php.net/dio-0.0.6
- Add "extension=dio.so" to php.ini
- Restart apache
