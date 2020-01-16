# CrackMapExec-Docker
Run CrackMapExec in Docker. Includes lsassy (https://github.com/Hackndo/lsassy) with the lsassy CrackMapExec module integration.

Build:

```
docker build -t crackmapexec .
```

Run:

```
docker run --rm -it -p 443:443 -p 445:445 -v $( pwd ):/data -v ~/.cme:/root/.cme --name=crackmapexec --entrypoint=/bin/bash crackmapexec
```

Or, you can alias the above docker run command to just cme in your .bash_profile or .bashrc file, and just run 'cme' from the command line.
