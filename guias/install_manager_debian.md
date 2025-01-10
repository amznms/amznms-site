# Downloading Manager Server

Create folder for Manager Server

```mkdir /usr/share/manager-server```

Download the latest version of Manager Server.

```wget https://github.com/Manager-io/Manager/releases/latest/download/ManagerServer-linux-x64.tar.gz -O /usr/share/manager-server/ManagerServer-linux-x64.tar.gz```

Then untar downloaded ManagerServer-linux-x64.tar.gz using following command

```tar xvzf /usr/share/manager-server/ManagerServer-linux-x64.tar.gz -C /usr/share/manager-serve```

# Install Manager Server as a service

## Install service unit configuration file:

```sudo nano /etc/init.d/manager-server```

## add the following content to the file

```
#!/bin/sh
### BEGIN INIT INFO
# Provides:          manager-server
# Required-Start:    $network
# Required-Stop:     $network
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Start daemon at boot time
# Description:       Enable service provided by daemon.
### END INIT INFO

case "$1" in
  start)
    echo "Starting manager-server"
    ulimit -n 1048576
    /usr/share/manager-server/ManagerServer -port 8080 &
    ;;
  stop)
    echo "Stopping manager-server"
    pkill -f '/usr/share/manager-server/ManagerServer -port 8080'
    ;;
  restart)
    $0 stop
    $0 start
    ;;
  *)
    echo "Usage: /etc/init.d/manager-server {start|stop|restart}"
    exit 1
    ;;
esac

exit 0
```

## Make the service file executable:

```sudo chmod +x /etc/init.d/manager-server```

## Use the update-rc.d command to add the service to the startup scripts:

```sudo update-rc.d manager-server defaults```

## Start the service

```sudo service manager-server start```

## Check if the service is running:

```sudo service manager-server status```

# Connecting to Manager Server

By default, Manager Server will listen on port 8080. Open your web-browser and navigate to http://127.0.0.1:8080 .