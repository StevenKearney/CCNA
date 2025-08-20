# Passwords

🔑 Creating passwords make devices more secure. There are several ways to create passwords in ios depending on what you're trying to limit access to.

# Table of contents

* Enable Passwords
* Console Passwords
* VTY (Virtual Teletype) Passwords

## Enable Passwords

* Protects against unauthorized access to **priviliged exec** mode
    ```
    enable
    conf t 
    enable password *password*
    ```
    * This password will appear in plain text if we view the running config
    * We can obfuscate this password and other plain text passwords with a service
        ```
        enable
        conf t
        service password-encryption
        ```
        * It is important to note this is still not secure as it is easily cracked with online tools
* A better version of the password command is secret
    * Secret allows us to set a password similar to the enable password, but it is encrypted with MD5 by default and uses stronger encryption
    * 🚩 If a secret password is enabled, it will override the password command, and the password command will no longer function
        ```
        enable
        conf t
        enable secret *password*
        ```
* **Secret** should always be used if possible

## Console Passwords

* Console passwords prevent unauthorized access via console ports on the device
* These passwords start at **user exec** mode
    ```
    enable
    conf t
    line console 0
    password *password*
    login
    exit
    ```
    * **line console 0**: this selects the console line/port to configure (always 0)
    * **login**: enables the password requirement
        * Without this, we will not be prompted for a password at all
    * These passwords are also stored in plain text unless **service password-encryption** is used
    * These can also be used with login local, but do not do this unless you set up usernames **FIRST**
        * Failure to do so will lock you out of the ios
    
## VTY (Virtual Teletype) Passwords

* VTY lines are virtual terminal lines that handle remote access to the CLI via **Telnet** or **SSH**
    * There are usually multiple lines allowing multiple connections at the same time
        * Usually 5 or 16 connections
            * **5**: vty 0-4
            * **16**: vty 0-15
* Required if you want remote access with a shared password, and no usernames
    * Without a password, remote access is **denied**
