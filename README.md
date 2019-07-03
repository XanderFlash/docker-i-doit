Docker build files to build an image for the open source CMDB [i-doit](https://www.i-doit.org).

These build files are only used to evaluate i-doit. It was not tested for a production installation.

# i-doit 1.13

## Build

```bash
sudo docker build -t i-doit:1.13 https://github.com/XanderFlash/docker-i-doit.git#:1.13
```

## Launch

Replace the port 8080 with a a free port at the system.

```bash
sudo docker run -p 8080:80 -t i-doit:1.13
```

## Work

Open `http://localhost:8080/i-doit/` in your browser and finish the installation.
Use default values and use **Next** until the installation is finished.