# Command Line Interface tips & more


### Check if PORT is open on machine (IP_ADDRESS of machine)
```
echo > /dev/tcp/IP_ADDRESS/PORT && echo "Port is open"
```
sample:
```
echo > /dev/tcp/127.0.0.1/9000 && echo "Port is open"
```

### Setup a static IP Address
```
nano /etc/dhcpcd.conf

```
then add at the bottom:
```
interface wlan0
static ip_address=192.168.1.50/24
static routers=192.168.1.1
static domain_name_servers=192.168.1.1
```
where 192.168.1.1 is the IP of the router, and 192.168.1.50 is the static ip you are giving to the machine. To find the interface name, type:
```
ifconfig
```

### SSH tunnel - Port Forwarding (using sshpass)
```
sshpass -p PASSWORD ssh -L LOCAL_PORT:IP_ADDRESS_FINALDESTINATION:REMOTE_PORT USERNAME@IP_ADDRESS_FORWARDER -p SSH_PORT -N -f
```

### irssi: IRC Client from terminal
| Command | Description |
| --- | --- |
| CTRL+N | Next screen |

### screen: attach to and detach from SSH sessions, leave them running in background without losing your work.
| Command | Description |
| --- | --- |
| apt-get install screen | Install screen |
| screen | Run a new screen session |
| CTRL+A+D | Detach the current session from terminal |
| screen -list | View the list of all sessions |
| screen -r [session # you want to attach, es.705] | Attach the session with the given ID number to the current SSH session |
| screen -X -S [session # you want to kill] quit | Destroy a session |


### Massive change of file extension
Using bash parameters expansion
```
for file in *.txt; do mv "${file%.txt}{.txt,.xml}"; done
```

### Print all git repos from a user
```
curl -s https://api.github.com/users/<username>/repos?per_page=1000 |grep git_url |awk '{print $2}'| sed 's/"\(.*\)",/\1/'
```
or  (only curl and grep):
```
curl -s https://api.github.com/users/<username>/repos?per_page=1000 | grep -oP '(?<="git_url": ").*(?="\,)'
```

### Show all current listening programs by port and pid with SS (instead of netstat)
```
ss -plunt
```

### Show top 50 running processes ordered by highest memory/cpu usage refreshing every 1s
```
watch -n1 "ps aux --sort=-%mem,-%cpu | head -n 50"
```

### Get external ip address
```
curl ifconfig.me
```

### Execute command/script periodically:
```
crontab -e
```
follow the comments inside.


### Download an entire website
-p parameter tells wget to include all files, including images.
-e robots=off you don't want wget to obey by the robots.txt file
-U mozilla as your browsers identity.
--random-wait to let wget chose a random number of seconds to wait, avoid get into black list.
Other Useful wget Parameters:
--limit-rate=20k limits the rate at which it downloads files.
-b continues wget after logging out.
-o $HOME/wget_log.txt logs the output 
```
wget --random-wait -r -p -e robots=off -U mozilla http://www.example.com
```

### SSH connection through host in the middle
Unreachable_host is unavailable from local network, but it's available from reachable_host's network. This command creates a connection to unreachable_host through "hidden" connection to reachable_host.
```
ssh -t reachable_host ssh unreachable_host
```
