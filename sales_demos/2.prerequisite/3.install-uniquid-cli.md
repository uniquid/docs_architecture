# Install UniquID CLI

{% hint style="info" %}
For  details about `uniquid` package click [here](https://pypi.org/project/uniquid/).
{% endhint %}

To install our UniquID CLI run command

```bash
sudo pip3 install uniquid
```

Make sure uniquid CLI client has been installed and is on the latest stable version

```bash
uniquid --version
```

If you previously installed the UniquID CLI but you're not on latest version you can issue the command

```bash
sudo pip3 install uniquid --upgrade
```

If you installed the UniquID CLI on an AWS EC2 instance you must upload your _credentials.json_ file to the instance. To do this you have two options:

**Option 1** - use scp command to copy that file on your instance

```bash
scp -i <user-key-pair.pem> credentials.json ubuntu@<ec2-instance-ip-address>:.
```

**Option 2** - cut and paste the contents of the UniquID credentials file after you SSH into the instance

```bash
cat > ~/credentials.json
--paste the content of the file--
press <enter> press CTRL D
```

### Login into your organisation

```bash
uniquid login --credentials-file ~/credentials.json
uniquid status --verbose
```



