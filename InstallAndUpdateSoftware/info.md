## RPM
RPM package manager provides a standard way to package software for distribution. Managing software in the form of RPM packages is simpler than working with software that has been extracted into a file system from an archive. RPM package files are named using a combination of the package **name-version-release.architecuture**. Each rpm package is made up of 3 components:

1. Files installed by the package.
2. Information about the package(metadata).
3. Scripts.

RPM packages can be digitally signed by the organization that packaged them. All packages from such an organization are normally signed by the same **GPG private key**. If the package is altered, the signature will no longer be valid. 

## Yum
Yum searches repositories for packages and their dependencies so they are installed together. The main configuration file for yum is **/etc/yum.conf** with additional repository configuration files located in the **/etc/yum.repos.d**. Repository configuration file includes at least:

1. Label
2. Name
3. URL

```{r, engine='bash', count_lines}
yum search KEYWORD 
```

Search all will search through the metadata as well.

```{r, engine='bash', count_lines}
yum search all 'server' | yum info httpd | 
```

The update obtains and install a new version of the software package, including any dependencies. The process tries to preserver the old configuration files in place, but in some cases they may be renamed if the packager thinks the old one wil not work after the update. 
```{r, engine='bash', count_lines}
yum update PKGNAME 
```

A new kernel can only be tested by booting to it, the package is designed so that multiple versions may be installed at once.
```{r, engine='bash', count_lines}
yum list kernel | yum update kernel
```

Yum also has the concept of groups, which are collections of related software installed together for a particular purpose. 
```{r, engine='bash', count_lines}
yum history | tail -10 /var/log/yum.log
```

## Enable repository
Enable and disable repository with the **yum-config-manager**. Install the RPM GPG key before installing signed packages.

## Examing rpm packages
-q => query <br />
-a => all <br />
-l => list files installed by the package <br />
-c => configuration files <br />
-d => documentation files
