# exim4-config-check-which 

Setting up Exim4 mail server?  

Got tired of guessing WHICH file to edit?  

Even after reading the 'man update-exim4.conf' manpage ... repeatedly?

This will help you determine which set of configuration files are being used by Exim4 mail server on your Debian OS distro.

## Weird Naming Convention

Naming convention could have been more intuitive.  Such as:

*  `update-exim4.conf` could be better named as `update-exim4-conf` (or `update-exim4`, or even better as `exim4-update-debian`, or best as `dpkgc-exim4`)
* `update-exim4.conf.conf` could be better named as `exim4.debian.conf`: then we would know which one to go to between `exim4.conf`/`exim4.conf.debian`.
* `update-exim4.conf.template` ... whew ... could have been `exim4.conf.template.debian`.
* `/var/lib/exim4/config.autogenerated` could have beeen `/var/lib/exim4/exim4.conf.autogenerated`.

## Debian Split-Configuration

A run example for the split-configuration method of Exim4 configuration files:

```console
# ~/bin/exim4-config-check-which.sh 
Determine which configs file that Exim4 is using.

Compiled-in config lookup sequence by /usr/sbin/exim4:
Configuration file search path is /etc/exim4/exim4.conf:/var/lib/exim4/config.autogenerated
Configuration file is /var/lib/exim4/config.autogenerated
/usr/sbin/exim4 is now using this config file:
              /var/lib/exim4/config.autogenerated

Reading /etc/exim4/update-exim4.conf.conf for its settings ...
'dc_use_split_config' is set to 'true' in /etc/exim4/update-exim4.conf.conf
  ignores          : /etc/exim4/exim4.conf
  ignores          : /etc/exim4/exim4.conf.template

  reads from       : /etc/exim4/update-exim4.conf.conf
  reads from       : /etc/exim4/exim4.conf.localmacros
  writes to        : /var/lib/exim4/config.autogenerated
  daemon reads from: /var/lib/exim4/config.autogenerated
```

## Debian Non-Split-Configuration

If using Debian non-split configuration, then:

```console
# ~/bin/exim4-config-check-which.sh 
Determine which configs file that Exim4 is using.

Compiled-in config lookup sequence by /usr/sbin/exim4:
Configuration file search path is /etc/exim4/exim4.conf:/var/lib/exim4/config.autogenerated
Configuration file is /var/lib/exim4/config.autogenerated
/usr/sbin/exim4 is now using this config file:
              /var/lib/exim4/config.autogenerated

Reading /etc/exim4/update-exim4.conf.conf for its settings ...
'dc_use_split_config' is set to 'false' in /etc/exim4/update-exim4.conf.conf
  ignores          : /etc/exim4/exim4.conf
  ignores          : /etc/exim4/update-exim4.conf.conf
  ignores          : /etc/exim4/exim4.conf.localmacros

  reads from       : /etc/exim4/exim4.conf.template
  writes to        : /var/lib/exim4/config.autogenerated
  daemon reads from: /var/lib/exim4/config.autogenerated
```

## Debian has `/etc/exim4/exim4.conf` file

If a Debian is missing `/etc/update-exim4.conf.conf` but has the required `/etc/exim4/exim4.conf`, then:

```console
# bash exim4-config-check-which.sh 
Determine which configs file that Exim4 is using.

Compiled-in config lookup sequence by /usr/sbin/exim4:
2022-03-25 13:14:38 Warning: purging the environment.
 Suggested action: use keep_environment.
Configuration file search path is /etc/exim4/exim4.conf:/var/lib/exim4/config.autogenerated
Configuration file is /etc/exim4/exim4.conf
/usr/sbin/exim4 is now using this config file:
              /etc/exim4/exim4.conf

Exim4 daemon will:
  Ignores          : /etc/exim4/update-exim4.conf.conf
  Ignores          : /etc/exim4/exim4.conf.template
  Ignores          : /etc/exim4/exim4.conf.localmacros
(update-exim4.conf writes nothing)

  daemon reads from: /etc/exim4/exim4.conf
```

## `/etc/exim4/exim4.conf` AND `/var/lib/exim4/config.autogenerated`

If a Debian had been transitioned to `exim.conf` but left an
artifact file behind that is named `/var/lib/exim4/config.autogenerated`, then

```console
# ~/bin/exim4-config-check-which.sh 
Determine which configs file that Exim4 is using.

Compiled-in config lookup sequence by /usr/sbin/exim4:
2022-03-25 13:28:04 Warning: purging the environment.
 Suggested action: use keep_environment.
Configuration file search path is /etc/exim4/exim4.conf:/var/lib/exim4/config.autogenerated
Configuration file is /etc/exim4/exim4.conf
/usr/sbin/exim4 is now using this config file:
              /etc/exim4/exim4.conf

Exim4 daemon will:
  Ignores          : /etc/exim4/update-exim4.conf.conf
  Ignores          : /etc/exim4/exim4.conf.template
  Ignores          : /etc/exim4/exim4.conf.localmacros
(update-exim4.conf writes nothing)

  daemon reads from: /etc/exim4/exim4.conf

WARNING: there is a lingering Debian artifact file that needs removing:
    /var/lib/exim4/config.autogenerated
```

## Debian host is missing both `/etc` files


If both config files are missing but ...
the `/var/lib/exim4/config.autogenerated` exists, then

```console
# ~/bin/exim4-config-check-which.sh 
Determine which configs file that Exim4 is using.

Compiled-in config lookup sequence by /usr/sbin/exim4:
Configuration file search path is /etc/exim4/exim4.conf:/var/lib/exim4/config.autogenerated
Configuration file is /var/lib/exim4/config.autogenerated
/usr/sbin/exim4 is now using this config file:
              /var/lib/exim4/config.autogenerated

WARNING: No configuration files found in /etc/exim4 directory.
WARNING: Exim4 daemon using the following UNKNOWN config file:
    /var/lib/exim4/config.autogenerated
```


