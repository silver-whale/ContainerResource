Basic Server Access: ssh {userName}@{serverIP} -p {PortNumber}

After VNC PortForwarding: ssh -x -L {PortNumber}:{LocalHostIP}:{PortNumber} {userName}@{ServerIP} -p {PortNumber}