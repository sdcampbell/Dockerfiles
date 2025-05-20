Build:  `docker build -t autopsy .`
Buildx: `docker buildx build -t autopsy .`

Run: `docker run --rm -it -v /path/to/images:/images -p 9999:9998 autopsy`

**Change /path/to/images to the path that stores all your images or just utilize `-v $(pwd):/images` if you would like to use your current working directory**

Access Autopsy Through [http://localhost:9999/autopsy](http://localhost:9999/autopsy)
