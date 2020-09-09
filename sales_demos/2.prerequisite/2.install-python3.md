# Install python3

Install Python 3 and pip3 on your specific machine. If you choose to set-up an AWS EC2 instance we recommend you use **Ubuntu Server 18.04 LTS \(HVM\) SSD Volume Type.** After the instance initialisation you will have to access it running command

```bash
ssh -i "<user-private-key-file.pem>" ubuntu@<instance-public-DNS>
```

Now that your Ubuntu/Linux machine is ready be sure that packages are updated...

```bash
sudo apt update
```

... and install python3 and pip3

```bash
sudo apt install python3
sudo apt install python3-pip
```

Verify the installation running command:

```bash
python3 --version
pip3 --version
```

{% hint style="warning" %}
To install python3 you may need to install the "universe" repository

`sudo add-apt-repository universe`
{% endhint %}

