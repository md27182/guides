## SSH with NGROK

### Create public and private keys on your personal computer
- ```ssh-keygen -t rsa -b 4096``` and hit ```ENTER``` twice to create key pair without a password
- Transfer the resulting ```id_rsa.pub``` file to the remote computer

### Set up ssh server on remote computer (Ubuntu 20.04 LTS)
- ```sudo apt-get install openssh-server```
- ```nano /etc/ssh/sshd_config```
  - Set the following, and don't forget to remove the ```#``` if there is one at the start of the line\
  ```ChallengeResponseAuthentication no```\
  ```PasswordAuthentication no```\
  ```usePAM no```
- ```cp <path to id_rsa.pub> ~/.ssh/authorized_keys```
  - If you need to add more authorized keys later, you can add them as new lines in the ```authorized_keys``` file
- Restart ssh server with ```sudo service ssh restart```

### Open tcp connection with ngrok on remote computer
- ```/opt/ngrok tcp --remote-addr=X.tcp.ngrok.io:XXXXX 22```
  - Replace X's with your ngrok static remote address
- Alternatively, set up a service to keep this running for you
  - https://jarifibrahim.github.io/blog/ssh-using-tailscale-or-ngrok/

### SSH from personal computer
- ```ssh user@X.tcp.ngrok.io -p XXXXX```
- ```screen```
- If the ssh connection drops, restart it and use ```screen -dr``` to pick up where you left off

### Reference material
- SSH https://www.youtube.com/watch?v=n9QBoXKot40&t=174s
