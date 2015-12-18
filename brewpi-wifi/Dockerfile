FROM phusion/baseimage:0.9.18
 
MAINTAINER roach@dopegoat.com
 
RUN apt-get update && apt-get upgrade -y -o Dpkg::Options::="--force-confold"
RUN apt-get install --no-install-recommends -y apache2 libapache2-mod-php5 php5-cli php5-common php5-cgi git git-core php5  python-serial python-simplejson python-configobj python-psutil python-git 
 
RUN useradd -m -k /dev/null -u 99 -g 100 -G www-data,dialout brewpi && sh -c 'echo "brewpi:`openssl rand -base64 32`" | chpasswd'
RUN usermod -a -G www-data brewpi
RUN usermod -a -G users brewpi
RUN chown -R www-data:www-data /var/www
RUN chown -R brewpi:users /home/brewpi
RUN find /home/brewpi -type f -exec chmod g+rwx {} \;
RUN find /home/brewpi -type d -exec chmod g+rwxs {} \;
RUN find /var/www -type d -exec chmod g+rwxs {} \;
RUN find /var/www -type f -exec chmod g+rwx {} \;
RUN sed -i 's#DocumentRoot /var/www/html#DocumentRoot /var/www#' /etc/apache2/sites-available/000-default.conf
RUN sudo -u brewpi git clone --branch 0.3.8 --depth 1  https://github.com/BrewPi/brewpi-script /home/brewpi && \
    rm -rf /var/www/* && sudo -u www-data git clone https://github.com/BrewPi/brewpi-www /var/www && \
    /home/brewpi/utils/updateCron.sh
RUN sed -i 's#inWaiting = ser.inWaiting()#inWaiting = ser.readline()#' /home/brewpi/brewpi.py
RUN sed -i 's#newData = ser.read(inWaiting)#newData = inWaiting#' /home/brewpi/brewpi.py
RUN sed -i 's#ser = serial.Serial(port, baudrate=baud_rate, timeout=time_out)#ser = serial.serial_for_url(port, baudrate=baud_rate, timeout=0.75)#' /home/brewpi/BrewPiUtil.py
RUN echo 'port = socket://example:23' > /home/brewpi/settings/config.cfg
RUN echo 'altport = socket://example:23' >> /home/brewpi/settings/config.cfg
RUN chown -R brewpi:users /home/brewpi/settings
RUN echo "#!/bin/bash" >/root/runbrewpi.sh
RUN echo "sed -i 's#^port.*#port = socket://'\"\$AVR\"'#' /home/brewpi/settings/config.cfg" >>/root/runbrewpi.sh
RUN echo "sed -i 's#^altport.*#altport = socket://'\"\$AVR\"'#' /home/brewpi/settings/config.cfg" >>/root/runbrewpi.sh
RUN echo "/usr/sbin/apache2ctl -D FOREGROUND" >>/root/runbrewpi.sh
RUN chmod +x /root/runbrewpi.sh

VOLUME /home/brewpi/data
 
EXPOSE 80
 
ENTRYPOINT /root/runbrewpi.sh
