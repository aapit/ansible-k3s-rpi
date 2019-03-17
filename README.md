This Ansible playbook turns your fleet of Raspberry Pi into Kubernetes cattle with k3s, the streamlined version of Kubernetes by @rancher that runs silky smooth on the ARM processor powering your Pi.

# Manually prepare your Raspberries
- Burn Raspian Stretch Lite with EtcherBalena, or something alike
- Create an empty `ssh` file on the SD card
- Copy your pubkey to the Pi with `ssh-copy-id pi@kube1.example.com`
- In `/etc/ssh/sshd_config`, set `PasswordAuthentication` to `no`

# Configure ansible setup
- Copy `ansible/hosts.template` to `ansible/hosts` for configuration
- Fill out the ip / hostname for the leading Raspberry under `[k3s-server]`
- Fill out the ip's / hostnames for the rest of your cattle under `[k3s-agents]`
I personally prefer using the Pi's hardware mac address to assign a hostname and ip address within the LAN.

# Provision the nodes
When you're all set up and configured, run the Ansible playbook:
```bash
$ make
```
This will:
- Install the k3s binary on the 'server' Pi (the leading node)
- Install a `k3s-server` service on the server and start it
- Fetch the node token from the server
- Start the k3s service on the agents with the server node token
- Enable autostart on boot for k3s on all nodes

# See if it worked
Log into your server node and run:
```bash
$ sudo k3s kubectl get node -o wide
```
You should see all of your nodes broadcasting a _Ready_ status.

Happy cattle herding!
