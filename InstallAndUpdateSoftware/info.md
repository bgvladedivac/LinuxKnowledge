## RPM
RPM package manager provides a standard way to package software for distribution. Managing software in the form of RPM packages is simpler than working with software that has been extracted into a file system from an archive. RPM package files are named using a combination of the package **name-version-release.architecuture**. Each rpm package is made up of 3 components:

1. Files installed by the package.
2. Information about the package(metadata).
3. Scripts.

RPM packages can be digitally signed by the organization that packaged them. All packages from such an organization are normally signed by the same **GPG private key**. If the package is altered, the signature will no longer be valid. 

## Yum
Yum searches repositories for packages and their dependencies so they are installed together. The main configuration file for yum is **/etc/yum.conf** with additional repository configuration files located in the **/etc/yum.repos.d**.
