To share current directory over anonymous webdav on port 80:

`docker run -p 80:80 -v "${PWD}":/srv/data/share rflathers/webdav`

Credit to @ropnop. I copied this from https://github.com/ropnop/dockerfiles/tree/master/webdav
