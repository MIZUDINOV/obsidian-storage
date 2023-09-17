[статья на stackoverflow](https://stackoverflow.com/questions/62441307/how-can-i-change-the-location-of-docker-images-when-using-docker-desktop-on-wsl2/63752264#63752264)

The WSL 2 docker-desktop-data vm disk image would normally reside in: `%USERPROFILE%\AppData\Local\Docker\wsl\data\ext4.vhdx`

Follow the following to relocate it to other drive/directory, with all existing docker data preserved (tested against Docker Desktop 2.3.0.4 (46911), and continued to work after updating the 3.1.0 (51484)):

First, shut down your docker desktop by right click on the Docker Desktop icon and select Quit Docker Desktop

Then, open your command prompt:

```
wsl --list -v
```

You should be able to see, make sure the STATE for both is Stopped.(`wsl --shutdown`)

```
  NAME                   STATE           VERSION
* docker-desktop         Stopped         2
  docker-desktop-data    Stopped         2
```

Export docker-desktop-data into a file

```
wsl --export docker-desktop-data "D:\Docker\wsl\data\docker-desktop-data.tar"
```

Unregister docker-desktop-data from wsl, note that after this, your `ext4.vhdx` file would automatically be removed (so back it up first if you have important existing image/container):

```
wsl --unregister docker-desktop-data
```

Import the docker-desktop-data back to wsl, but now the ext4.vhdx would reside in different drive/directory:

```
wsl --import docker-desktop-data "D:\Docker\wsl\data" "D:\Docker\wsl\data\docker-desktop-data.tar" --version 2
```

Start the Docker Desktop again and it should work

You may delete the `D:\Docker\wsl\data\docker-desktop-data.tar` file (NOT the `ext4.vhdx` file) if everything looks good for you after verifying