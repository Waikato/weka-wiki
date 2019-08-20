Where a user's home directory is located varies from platform to platform and among the users on a single computer. But the actual location of the home directory is available through special environment variables:

* Unix/Linux
> `$HOME`
* Windows
> `%USERPROFILE%`
* Cygwin
> `$USERPROFILE`

In order to find out where these environment variables actually point to, do the following:

* on **Unix/Linux**, open a terminal and type the following command
> `echo $HOME`
* on **Windows**, open a command-prompt and type the following command
> `echo %USERPROFILE%`
* on **Cygwin**, open a bash and type the following command
> `echo $USERPROFILE`
