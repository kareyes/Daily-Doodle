
### Install Google Cloud CLI

It has recently updated its packages

```
sudo apt-get update
```

It has [`apt-transport-https`](https://packages.debian.org/bullseye/apt-transport-https) and `curl` installed:

```
sudo apt-get install apt-transport-https ca-certificates gnupg curl
```

Import the Google Cloud public key:

```
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo gpg --dearmor -o /usr/share/keyrings/cloud.google.gpg
```


Add the gcloud CLI distribution URI as a package source:

```
echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
```

Update and install the gcloud CLI:

```
sudo apt-get update && sudo apt-get install google-cloud-cli
```

### Initialize GCloud CLI

```
gcloud init
```

or If you are in a remote terminal session, you can use the `--no-launch-browser` flag to prevent the command from launching a browser-based authorization flow, if required:

```

gcloud init --no-launch-browser
```
Complete the authorization step when prompted.

Choose a current Google Cloud project if prompted.

To view the properties set through the `gcloud init` command

```
gcloud config list
```



