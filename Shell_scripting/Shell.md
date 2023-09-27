# Shell Scripting 

### What Is Shell Scripting ? 🤔
Shell scripting is the process of writing and executing a series of commands in a shell, which is a command-line interface to an operating system. A shell script is essentially a script or program written in a shell language, such as Bash, sh, zsh, or PowerShell ?

### What Is a Shebang or Hashbang (#!/bin/bash)
At the beginning of a script, is a ***shebang or hashbang***. It is a special notation used in Unix-like operating systems, to specify the interpreter that should be used to execute the script. In this case, #!/bin/bash specifically indicates that the Bash shell should be used to interpret and execute the script.

 The #! (pronounced as "shebang" or "hashbang"): This character sequence tells the operating system that what follows is the path to the interpreter that should be used to execute the script.

/bin/bash: This is the absolute path to the Bash shell executable. It tells the system to use the Bash interpreter located at /bin/bash to run the script.

When a script with a shebang line like #!/bin/bash, the operating system looks for the specified interpreter (in this case, Bash) and uses it to execute the script. 

Without a shebang line, the system may not know how to interpret and execute the script, and you may need to explicitly specify the interpreter when running the script. 

### Syntax For a Bash script
Every line of a bash file starts with a shebang 
   
    #!/bin/bash
Execute a bash script assuming the file containing the script is darey

    ./darey.sh
Note: all bash file are always ending with an extension .sh
Also, darey.sh must be an executable file before it can be executed

    sudo chmod u+x darey.sh
 The command above make darey.sh an executable file

 An alternative for executing a bash script

    bash darey.sh 


### Creating Shell Scripts

