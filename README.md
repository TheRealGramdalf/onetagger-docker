## **Unofficial** docker image for [OneTagger](https://github.com/Marekkon5/onetagger)

*This repository is used to build the image at https://hub.docker.com/repository/docker/therealgramdalf/onetagger.*


> [!WARNING]
> This image is **very unstable**. It is meant to **work**, **NOT** to work **well** or **securely**. Run at your own risk!
> Please see the notes below for instructions on using this properly.

### Notes:

For both my sanity and yours, I built this image with the intention of running it as needed, rather than as a service running 24/7. This means that little to no effort was made in making this secure, so please run it with caution.

What I *did* intend it for is for it to run locally (similar to an application, just containerized), which is helpful if your distrobution doesn't package OneTagger natively yet (such as NixOS), and other installation methods fail. Or, to run as a program on a local or remote server - if this is the case, please remember that this image is **insecure by default!** Make sure to secure it in some other way, such as behind a reverse proxy with basic auth enabled.

***Always specify a static tag to run this image! `latest` will pull the most recent (potentially breaking) version, which could mess up your files!***

### Running the image

Here is an example `docker compose` file:
If you need `docker run` instead, please see https://decomposerize.com/

```yaml
services:
  onetag:
    image: therealgramdalf/onetagger:devel-ubuntu
    volumes:
      - ./config:/config # Persist configuration in the current subdirectory `./config`
      - /path/to/music:/data/music # Use this to bind mount your media into the container (so onetagger can tag it)
    # The ports section is only required if you use a bridge network (which is the default)
    ports:
      - 36913:36913 # HTTP and WebSocket. Currently only `devel-ubuntu` supports a single port, 1.7.0 and older require multiple
```

### Configuration

To configure the image, make use of the `command:` section in docker compose:
```yaml
services:
  onetag:
    # ...Other configuration here
    command:
      # The first five lines are the default setting - if you specify your own flags, add them...
      - onetagger # Run onetagger
      - --server # Run headlessly (No UI, serve app over HTTP instead)
      - --expose # Listen on 0.0.0.0:36913 (any ip:36913) instead of localhost:36913 (local machine only)
      - --path
      - /data/music
      # ...here:
      - --opt1
      - --opt2
      - opt2_value 
```
Please note that whitespace (` `) should be placed on a new line:
```yaml
# Invalid:
- --opt1 value
# Valid:
- --opt1
- value
# Also valid (whitespace escaped with a quote):
- --opt1\ value
# Also valid (if the program supports it):
- --opt1=value
```

~ TheRealGramdalf