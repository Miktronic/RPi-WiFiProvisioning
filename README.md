# RPi-WiFiProvisioning by Bluetooth Serial terminal

# Step1 : Install bluetooth packages for python application and download github app

sudo apt-get install bluetooth bluez python-bluez

git clone ....

chmod 755 -R ./RPi-WiFiProvisioning

# Step2 Raspberry pi Bluetooth configuration

- Add the followings in /etc/rc.local before "exit 0"

sudo bluetoothctl <<EOF
power on
discoverable on
pairable on
default-agent
agent NoInputNoOutput
EOF
sudo hciconfig hci0 sspmode 0
bash /home/{username, ex:pi}/RPi-WiFiProvisioning/remove_all_paired_devices.sh

- Set bluetooth discoverable permanently

sudo nano /etc/bluetooth/main.conf
...
DiscoverableTimeout = 0
...

- Copy/Paste Service files to /etc/systemd/system/

sudo cp ./RPi-WiFiProvisioning/simple_agent.service /etc/systemd/system  # this is a bluetooth agent service with PIN Code Authentication
sudo cp ./RPi-WiFiProvisioning/rfcomm_server.service /etc/systemd/system # this is a bluetooth terminal server

sudo systemctl daemon-reload
sudo systemctl enable simple_agent.service
sudo systemctl enalbe rfcommn_server.service

sudo reboot


