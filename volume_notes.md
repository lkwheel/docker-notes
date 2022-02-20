# Volume Notes

Persisting the data in your docker container uses Container Volumes

Two types:
- Named Volumes
- Bind Mounts



## Named Volumes

Creating a volume and attaching (mounting) it to the directory it is
stored in, we persist the data.

With named volumes Docker maintains the physical location on the disk
and you only need to remember the name of the volume. Every time you
use the volume, Docker will make sure the correct data is provided

Command to create the volume

```bash
docker volume create todo-db
```

Command to start the container with the volume mounted

```bash
docker run -dp 3000:3000 -v todo-db:/etc/todos docker-101
```

Where is Docker storing the data?

type:

```bash
docker volume inspect todo-db
```

You may see something like this

```bash
$ docker volume inspect todo-db
[
    {
        "CreatedAt": "2022-02-20T16:19:50Z",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/todo-db/_data",
        "Name": "todo-db",
        "Options": {},
        "Scope": "local"
    }
]
```

I think that Docker mostly uses by default the **/var** directory of your
local machine so make sure that you acquire enough storage for that
local filesystem.

## Bind mounts

You can not only store the location of where you want the data located,
but also provide data into the container when expected.

The following will take the a node container (version: 10-alpine) and
install the dependencies and then run the code (in dev mode).

```bash
docker run -dp 3000:3000 \
    -w /app -v $PWD:/app \
    node:10-alpine \
    sh -c "yarn install && yarn run dev"
```