## Intro to bash scripting (Shell scripting for DevOps)
shebang -> #!/ 
`Interview Q: What's the difference between the executables sh and bash `
#!/bin/sh : some years back this sh executable was widely used for bash scripting as it was linked to #!/bin/bash
using the lining concept of the linux .

- but nowadays , for some operating system (Ubuntu), it is forwarded to the executable named dash 
- so now if you write #!/bin/sh , it meant you've written #!/bin/dash

conclusion : previously sh exe was same as the bash casue sh was redirected to bash using linking concept  but now 
its not the case as some of the oeprating system have decided to use dash as the default  

## commands
### touch
- creates a file
### vi/vim
- opens up a file to read /write in it (if file is not there then it is created.)
- esc then  i to insert
- esc then :wq! to save and exit
### ./file_name
- to execute any file in linux.
### chmod 777 file_name
- to grant permission for file accessing.
- 7 : 4(R) , 2(W), 1(E)
- R : read, W: write, E: execute
- 7: user: Owner: RWX ;;; 7: group: Owner: RWX ;;; 7: others: RWX
### check history 
- history
### man
- gives the manual of any command 
- eg: man ls ; gives the manual documentation of ls command;
### cat
- to show the insider content of the file.
### rm -rf
-  tp delete the folder 

:::: Not finished 
