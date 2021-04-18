## Instalation 
To install the Go compiler you just need to download the installer from the website. To check if the installation was correct you can run:
```
go version
```

## Packages
It seems that third party packages are installed on the `$HOME/go` folder. Kind of like a node_modules but available to every program :thinking:. This could be problematic when we need to upload the code to a server. I´m sure there is way to make sure everything runs fine on the origin device like venv in python.

The book says that is recommended to add set the *GOPATH*

```
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```

We can see a list of env variables available with the command `go env`. That looked very organized and clear. 

## Go command
The compiler comes with a bunch of preinstalled tools. There´s one command that works as prettier.

```
go fmt hello.go
```

We can also run the file with *run* or compile it with *build*.

Go is compiled language but what the *run* command does is to compile a temporal binary and deletes it after the program was executed. That´s cool because it acts like an scripting language like python.