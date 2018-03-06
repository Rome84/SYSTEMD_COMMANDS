# SYSTEMD_COMMANDS

# Systemd
VIEWING-SYSTEMD-INFORMATION
==> systemctl list-dependencies ==> Show a unit’s dependencies

==> systemctl list-sockets ==> List sockets and what activates

==> systemctl list-jobs ==> View active systemd jobs

==> systemctl list-unit-files ==> See unit files and their states

==> systemctl list-units ==> Show if units are loaded/active

==> systemctl get – default ==> List default target (like run level)

# WORKING-WITH-SERVICES-SERVICES

==> systemctl stop service ===> Stop a running service

==> systemctl start service ===> Start a service

==> systemctl restart service ===> Restart a running service

==> systemctl reload service ===> Reload all config files in service

==> systemctl status service ===>See if service is running/enabled

==> systemctl enable service ===> Enable a service to start on boot

==> systemctl disable service ===> Disable service–won’t start at boot

==> systemctl show service ===> Show properties of a service (or other unit)

==> systemctl -H host status network ===> Run any systemctl command remotely

# CHANGING-SYSTEM-STATES
==> systemctl reboot ===> Reboot the system (reboot.target)
==> systemctl poweroff ===> Power off the system (poweroff.target)
==> systemctl emergency ===> Put in emergency mode (emergency.target)
==> systemctl default ===> Back to default target (multi-user.target)

# VIEWING-LOG-MESSAGES
==> journalctl ===> Show all collected log messages
==> journalctl -u network.service ===> See network service messages
==> journalctl -f ===> Follow messages as they appear
==> journalctl -k ===> Show only kernel messages

# SYSVINIT-TO-SYSTEMD
*****Used START-A-SERVICE
(not reboot persistent)
Sysvinit:#==> service SERVICE_NAME start
Systemd: #==> systemctl start SERVICE_NAME (Example===>systemctl start cron.service)
 
# Used START-A-SERVICE
Sysvinithttps==> service SERVICE_NAME stop
Systemd: systemctl stop SERVICE_NAME
Notes: Used to stop a service (not reboot persistent)

# Used TO-STOP-AND-THEN-START-A-SERVICE
Sysvinit service SERVICE_NAME restart
Systemd==> systemctl restart SERVICE_NAME
 
# WHEN-SUPPORTED,RELOADS-THE-CONFIG-FILE-WITHOUT-INTERRUPTING-PENDING-OPERATIONS.
Sysvinit==>service SERVICE_NAME reload
Systemd==> systemctl reload SERVICE_NAME
 
# RESTARTS-IF-THE-SERVICE-IS-ALREADY-RUNNING.
Sysvinit==> service SERVICE_NAME condrestart
Systemd==>  systemctl condrestart SERVICE_NAME
 
# TELLS-WHETER-A-SERVICE-IS-CURRENTLY-RUNNING.
Sysvinit==> service SERVICE_NAME status
Systemd==>  systemctl status SERVICE_NAME

# TURN-THE-SERVICE-SERVICE-ON,FOR-START-AT-NEXT-BOOT,OR-OTHER-TRIGGER.
Sysvinit ==>chkconfig SERVICE_NAME on
Systemd ==> systemctl enable SERVICE_NAME

# TURN-THE-SERVICE-SERVICE-OFF-FOR-THE-NEXT-REBOOT,OR-OTHER-TRIGGER.S
Sysvinit==>chkconfig SERVICE_NAME off
Systemd==> systemctl disable SERVICE_NAME

# CHECK-IF-THE-SERVICEIS-CONFIGURED-TO-START-OR-NOT-IN-THE-CURRENT-ENVIRONMENT.
Sysvinit==>chkconfig SERVICE_NAME
Systemd==> systemctl is-enabled SERVICE_NAME

# PRINT-A-TABLE-OF-SERVICES-THAT-LISTS-lISTS-WHICH-RUNLEVELS-EACH-IS-CONFIGURED-OR-OFF
Sysvinit==>chkconfig –list
Systemd==> systemctl list-unit-files –type=service (or) ls /etc/systemd/system/*.wants/

# PRINT-A-TABLE-OF-SERVICES-THAT-WILL-BE-STATRED-WHEN-BOOTING-INTO-GRAPHICAL-MODE.
Sysvinit==> chkconfig –list | grep 5:on
Systemd==>  systemctl list-dependencies graphical.target

# TO-LIST-WHAT-LEVELS-THIS-SERVICE-IS-CONFIGURED-ON-OR-OFF.
Sysvinit==>  chkconfig SERVICE_NAME –list
Systemd==>  ls /etc/systemd/system/*.wants/SERVICE_NAME.service

# USED-WHEN-YOU-CREATE-A-NEW-SERVICE-FILE-OR-MODIFY-ANY-CONFIGURATION.
Sysvinit==>  chkconfig SERVICE_NAME –add
Systemd==>  systemctl daemon-reload

# HALT-THE-SYSTEM
Runlevels to Targets 
Sysvinit==>  0
Systemd==>  runlevel0.target, poweroff.target

# SINGLE-USER-MODE
Sysvinit: 1, s, single
Systemd: runlevel1.target, rescue.target


# Sysvinit: 2, 4
Systemdc==>  runlevel2.target, runlevel4.target, multi-user.target
Notes: User-defined/Site-specific runlevels. By default, identical to 3.

# Sysvinit==>  3
Systemd==>  runlevel3.target, multi-user.target
Notes==> Multi-user, non-graphical. Users can usually login via multiple consoles or via the network.

# MULTI-USER,GRAPHICAL.
Sysvinit==>  5
Systemd==>  runlevel5.target, graphical.target
Notes==>  Multi-user, graphical. Usually has all the services of runlevel 3 plus a graphical login.

# REBOOT
Sysvinit==>  6
Systemd==>  runlevel6.target, reboot.target

# EMERGENCY-SHELL
Sysvinit==> emergency
Systemd==>  emergency.target


# CHANGING-RUNLEVELS ==> Change to multi-user run level.
Sysvinit==>telinit 3
Systemd==> systemctl isolate multi-user.target (OR systemctl isolate runlevel3.target OR telinit 3)

# SET-TO-USE-MULTI-USER-RUNLEVEL-ON-THE-NEXT-REBOOT.
Sysvinit==> sed s/^id:.*:initdefault:/id:3:initdefault:/
Systemd==>  ln -sf /lib/systemd/system/multi-user.target /etc/systemd/system/default.target
