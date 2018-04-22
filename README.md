
# rqlite_build
This is cloned rqlite with all dependencies, ready to build where go get is not available

#4/7/2018:
```bash

#NOTE the /... at the end - otherwise no dependencies will be downloaded !
[ec2-user@ip-172-31-38-111 dev]$ go get -v github.com/rqlite/rqlite/...
[ec2-user@ip-172-31-38-111 dev]$ ll rqlite
drwxrwxr-x 2 ec2-user ec2-user 4096 Apr  7 21:32 bin
drwxrwxr-x 3 ec2-user ec2-user 4096 Apr  7 21:31 pkg
drwxrwxr-x 4 ec2-user ec2-user 4096 Apr  7 21:31 src


[ec2-user@ip-172-31-38-111 rqlite]$ go build -a -v -x  -o bin/rqlite  src/github.com/rqlite/rqlite/cmd/rqlite/main.go   #this is the wrong way to do it:
# command-line-arguments
src/github.com/rqlite/rqlite/cmd/rqlite/main.go:65: undefined: query
src/github.com/rqlite/rqlite/cmd/rqlite/main.go:67: undefined: query
src/github.com/rqlite/rqlite/cmd/rqlite/main.go:69: undefined: query
src/github.com/rqlite/rqlite/cmd/rqlite/main.go:79: undefined: query
src/github.com/rqlite/rqlite/cmd/rqlite/main.go:81: undefined: execute
[ec2-user@ip-172-31-38-111 rqlite]$ ll src/github.com/rqlite/rqlite/cmd/rqlite
total 20
-rw-rw-r-- 1 ec2-user ec2-user 1260 Apr  7 21:31 execute.go
-rw-rw-r-- 1 ec2-user ec2-user 5305 Apr  7 21:31 main.go
-rw-rw-r-- 1 ec2-user ec2-user 2220 Apr  7 21:31 query.go
-rw-rw-r-- 1 ec2-user ec2-user 1257 Apr  7 21:31 README.md

[ec2-user@ip-172-31-38-111 rqlite]$ GOPATH=$PWD go build -a -v -x  -o bin_dynamic/rqlite  src/github.com/rqlite/rqlite/cmd/rqlite/*.go  #this is the right way:
cp $WORK/command-line-arguments/_obj/exe/a.out bin_dynamic/rqlite

[ec2-user@ip-172-31-38-111 rqlite]$ GOPATH=$PWD  go build -a -v -x  -o bin_dynamic/rqlited src/github.com/rqlite/rqlite/cmd/rqlited/*.go
cp $WORK/command-line-arguments/_obj/exe/a.out bin_dynamic/rqlited

[ec2-user@ip-172-31-38-111 rqlite]$ GOPATH=$PWD  go build -a -v -x  -o bin_dynamic/rqbench src/github.com/rqlite/rqlite/cmd/rqbench/*.go
cp $WORK/command-line-arguments/_obj/exe/a.out bin_dynamic/rqbench
[ec2-user@ip-172-31-38-111 rqlite]$ ldd bin_dynamic/*

#static compilation:
[ec2-user@ip-172-31-38-111 rqlite]$ GOPATH=$PWD  CGO_ENABLED=0 go build -a -v -x  -o bin_static/rqlite  src/github.com/rqlite/rqlite/cmd/rqlite/*.go
[ec2-user@ip-172-31-38-111 rqlite]$ GOPATH=$PWD  CGO_ENABLED=0 go build -a -v -x  -o bin_static/rqlited src/github.com/rqlite/rqlite/cmd/rqlited/*.go #this does not produce the binary!
[ec2-user@ip-172-31-38-111 rqlite]$ GOPATH=$PWD  CGO_ENABLED=0 go build -a -v -x  -o bin_static/rqbench src/github.com/rqlite/rqlite/cmd/rqbench/*.go
[ec2-user@ip-172-31-38-111 rqlite]$ ldd bin_static/*
bin_static/rqbench:
        not a dynamic executable
bin_static/rqlite:
        not a dynamic executable

#remove references to the current github repo, we are going to commit to another one
[ec2-user@ip-172-31-38-111 rqlite]$ find src -type d -name .git -ls -exec rm -rf {} \;
[ec2-user@ip-172-31-38-111 rqlite]$ git init; git remote add github  https://github.com/dbabits/rqlite_build;
[ec2-user@ip-172-31-38-111 rqlite]$ find src -type f |xargs git add -v -n #dry run
[ec2-user@ip-172-31-38-111 rqlite]$ find src -type f |xargs git add -v 		
...

#on the other side:
curll https://github.com/dbabits/rqlite_build/archive/master.zip > rqlite.zip

```
