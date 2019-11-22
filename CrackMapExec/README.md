# CrackMapExec-Docker
Run CrackMapExec in Docker

Build:

```
docker build -t crackmapexec .
```

Run:

```
docker run --rm -it -p 445:445 -v $( pwd ):/data -v ~/.cme:/root/.cme --name=crackmapexec --entrypoint=/usr/local/bin/cme crackmapexec
```

Or, you can alias the above docker run command to just cme in your .bash_profile or .bashrc file, and just run 'cme' from the command line.
