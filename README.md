# Singularity exec wrapper

This script is a simple helper wrapper to enable the use of software
distributed in singularity containers. It relies on a few bash tricks to
enable the script to use its filename inside the script itself.

## Usage

Make sure the script is executable (`chmod +x`). Create a directory
which will contain the singularity container file, which must be named
`container.simg`, and symlink (`ln -s`) the wrapper script into that
directory, naming it for each command which exists inside the container
you would like to wrap.

For example, you have a singularity container which contains a program
called `ANTS` and another called `PrintHeader`, the directory may look
like:

``` bash
$ ls -l
total 8.0K
lrwxrwxrwx 1 gdevenyi gdevenyi    7 2022-06-03 11:52 ANTS -> wrapper*
lrwxrwxrwx 1 gdevenyi gdevenyi    7 2022-06-03 11:54 PrintHeader -> wrapper*
-rw-rw-r-- 1 gdevenyi gdevenyi 1006 2022-06-03 11:54 README.md
-rw-rw-r-- 1 gdevenyi gdevenyi 1.0G 2022-06-03 11:52 container.simg
-rwxr-xr-x 1 gdevenyi gdevenyi 1.4K 2022-06-03 11:49 wrapper*
```

Adding this directory to `$PATH` will now allow you to use the commands
you have wrapped as though they they are natively installed.

## Tips

Singularity by default only exposes the following paths to processes
executing inside the container: `$HOME`, `/tmp`, `/proc`, `/sys`,
`/dev`, and `$PWD`. Your system administrator (if you have one) may have
configured additional paths.

See https://sylabs.io/guides/3.0/user-guide/bind_paths_and_mounts.html
for more details.

Attempting to acesss files and paths outside the defaults using wrapper
executables may result in errors.

You can define paths which should always be present in the container by
defining a comma-separated list in your environment via
`$SINGULARITY_BINDPATH`, or you can uncomment and modify the variable
export in the `wrapper`.
