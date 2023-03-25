# omv_startup_sequence
If you're stumbled upon OpenMediaVault (OMV), you could trigger some problems with smb and other services not working properly after reboot. There's a script that fired up after every boot gives my server smooth start. 

======================================================


Without this command my server can't mount file systems properly, docker, smb and nfs have some problems and there's a problem with permissions. 
If you're experiencing the same try this:

Go to "Scheduled Jobs" in your OMV control panel 
Add new one
Time of execution: At reboot
User: root
Command: <code>mount -o remount, rw /; touch /forcefsck; sudo journalctl; sudo service smbd stop; sudo service smbd restart; sudo systemctl start systemd-resolved; sudo systemctl enable systemd-resolved; systemctl stop containerd; systemctl start containerd; systemctl start docker.service; sudo systemctl enable collectd.service; sudo systemctl start collectd.service; sudo systemctl restart nfs-kernel-server</code>

Make sure it's enabled and running. 



If your control panel isn't working, but you have access to terminal as root, try to type this command manually. 


ofc. you're using it at your own risk. 
