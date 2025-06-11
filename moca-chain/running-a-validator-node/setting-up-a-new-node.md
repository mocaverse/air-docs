# Setting Up a New Node

1. You need to choose a home folder `$NODE_HOME` (i.e. \~/.mechaind) for Moca Chain. You can set this up by:

```bash
mkdir -p ~/.mechaind/config
```

2.  Download from [Releases · MocaLabs/mechain](https://github.com/zkMeLabs/mechain/releases) and copy the config `$NODE_HOME/config`

    You can use the following command to download the configuration file suitable for your environment. Please replace "linux\_x86\_64" with your operating system as needed. For specific details, visit the release page.

```bash
wget $(curl -s https://api.github.com/repos/Mocalabs/mechain/releases/latest | grep 
browser_download_url | grep linux_x86_64 | cut -d\" -f4)
```

&#x20;       This command automatically finds and downloads the latest version for Linux\_x86\_64.

{% hint style="info" %}
The download link includes "linux\_x86\_64" in its name, which indicates that it’s compatible with Linux systems
{% endhint %}

3.
