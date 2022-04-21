Vagrant.configure("2") do |config|
    config.vm.box = "MSEdge - Win10"
    config.vm.box_url = "~/VirtualBox VMs/MSEdge - Win10.box"
	
	# Map local port 8888 to port 8080 on the VM so we can view the Mendix application on http://localhost:8888
#    config.vm.network "forwarded_port", guest: 8080, host: 8888

    config.vm.communicator = "winrm"
    config.vm.guest = :windows
    config.winrm.username = "IEUser"
    config.winrm.password = "Passw0rd!"
    config.vm.boot_timeout = 600
    config.vm.graceful_halt_timeout = 600
    config.vm.disk :disk, size: "80GB", primary: true
    config.vm.disk :dvd, name: "installer", file: "/usr/share/virtualbox/VBoxGuestAdditions.iso"
    config.ssh.username="IEUser"
    config.ssh.password="Passw0rd!"
    config.ssh.insert_key = false
    config.ssh.sudo_command = ''
    config.ssh.shell = 'sh -l'

    config.vm.provider "virtualbox" do |vb|
        vb.name = "Sasjas Windows Machine"
        vb.memory = 8192
        vb.cpus = 4
        vb.gui = true
        vb.customize ["modifyvm", :id, "--vram", "128"]

        vb.customize ["sharedfolder", "add", :id,
                      "--name", "localshare",
                      "--hostpath", ".",
                      "--automount",
                      "--auto-mount-point", "W:"]

        #vb.customize ["guestcontrol", :id, "updateadditions", "--source", "/usr/share/virtualbox/VBoxGuestAdditions.iso", "--verbose"]
    end

    config.vm.provision "shell", binary: true, privileged: true, inline:  '''
        reg add "HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
        netsh advfirewall firewall set rule group="remote desktop" new enable=yes
        net localgroup "remote desktop users" IEUser /add
        echo "installing python"
        W:\python-3.10.4-amd64.exe /quiet InstallAllUsers=1 PrependPath=1
        echo "updating guest additions"
        D:\VBoxWindowsAdditions.exe /S
    '''
end
