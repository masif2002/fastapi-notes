# Linux commands
```
ls
```
* returns the contents of the current directory
___

```
ls -la
```
* prints out a list of ALL files and folders,
___

```
sudo apt update && sudo apt upgrade -y
```
* Updates all of the installed packages
    * -y flag signifies 'yes' for all the prompts that appear, automatically
___

```
sudo apt install <package_name>
```
* Installs the package for us 
___

```
whoami
```
* Returns the current user
___
```
sudo cat /etc/passwd
```
* Returns a list of users installed in your ubuntu machine
___
```
su - <name_of_the_user>
```
* Switches to the specified user
___

```
systemctl restart <name_of_the_service>
```
* Restarts the service (or the application)
___

```
adduser <username>
```
* Creates a new user    
___

```
usermod -aG sudo <username>
```
* This command gives the user, root access (sudo)
___

```
pwd
```
* Returns the current directory
___

```
cat <filename>
```
* prints the content of a file onto the standard output stream
___

```
export <env_name>=<value>
```
* Sets an environment variable 
___

```
printenv
```
* Prints all of the environment variables
___

```
unset <env_name>
```
* Removes the environment variable
___
```
set -o allexport; source <path>/.env; set +o allexport
```
* Sets the envirnoment variables from .env file instead of having to use the _export_ command to set every environment variable
* To ensure that the env variables are set if the system is rebooted, you need to paste the same command in _.profile_ file in the home directory of your machine.
___

```
touch <file_name>
```
* Creates an empty file
___
```
source <file_name>
```
* Reads and executes the content of a file (which has a set of commands)
___
```
vi <filename>
```
* Opens the file in a text editor 
* Similarly `nano` is also a text editor
___


```
rm <filename>
```
* Deletes the file
* A directory can be deleted by adding an extra `-r` flag. (**r** for recursive)
___

```
cp <source_file> <target_file>
```
* Copies one file to another
___

```
sudo ufw <command>
```
* ufw is the firewall in ubuntu
* You can add various rules here
* `sudo ufw --help` displays the different commands

Ex:
```
sudo ufw allow from <ip_adress> to any port <port_number>
```
* This command adds a rule which allows connection from the given ip address to the specific port
_
## Shortcuts
```
Ctrl + L
```
* Clears the terminal screen
___

```
Ctrl + R
```
* Reverse search 
* Searches for the previous commands used in terminal
___
```
Shift + insert
```
* Paste
___

## Notes
* *sudo* is used for root privileges (Like running as Administrator in windows machine)
* If you make any changes to the configuration files, the changes are only reflected when you restart the service using `sudo systemctl restart`