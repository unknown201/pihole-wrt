Things to fixup
/etc/lighttpd/conf.d/99-pihole.conf #diff the files
/etc/lighttpd/lighttpd.conf #need to run as root because of php sock
/opt/pihole/COL_TABLE  # right at the top of the file
    tputCommand="echo 8"
    if command -v tput &> /dev/null; then
    tputCommand="tput colors"
    else
    tputCommand="echo 8"
    fi

    if ([[ -t 1 ]] && [[ $(${tputCommand}) -ge 8 ]]) || [[ "${WEBCALL}" ]]; then





need to fix gravity.db / gravity.sh

https://github.com/ilya-fedin/pihole-openwrt



taillog.php doesn't seem to work

groups.php throws an error

update dnsmasq.conf # to add default upstream server

//////////// FIXED///////////////////
/etc/pihole-FTL-poststop.sh
/etc/pihole-FTL-prestart.sh

need to add all of pihole git to /etc/.pihole
the scripts are used by the program to update things

need to patch basic-install.sh to fix the pihole location from /usr/local/bin to /usr/bin


/opt/pihole/webpage.sh # sources basic-install for something?

settings.php
Fatal error: Uncaught Error: Call to undefined function filter_var() in /www/admin/scripts/pi-hole/php/func.php:67 Stack trace: #0 /www/admin/scripts/pi-hole/php/savesettings.php(117): validIP('8.8.8.8') #1 /www/admin/scripts/pi-hole/php/savesettings.php(196): readDNSserversList() #2 /www/admin/settings.php(12): require('/www/admin/scri...') #3 {main} thrown in /www/admin/scripts/pi-hole/php/func.php on line 67
----- need to add php8-mod-filter to dependencies

coreutils-install
coreutils-chmod
coreutils-chown