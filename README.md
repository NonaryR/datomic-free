# Datomic Free Edition

Non-official Docker image for [Datomic Free Edition][1] based on the official
[Java Image][2].

The use of Datomic Free Edition is governed by the terms of the Datomic Free 
Edition License which you can find [here][3]. By using this Docker image, you 
are agreeing to those terms.

## Usage

To start a container with Docker running at localhost, the following command
is sufficient

    docker run -d -p 4334-4336:4334-4336 --name datomic-free -v /Users/nonaryr/Documents/clojure/datomic-volume/data:/data -v /Users/nonaryr/Documents/clojure/datomic-volume/log:/log  datomic:1

Create database

    docker exec -ti datomic-free /bin/bash
    bin/repl
    user=> (require '[datomic.api :as d])
    user=> (d/create-database "datomic:free://192.168.99.100:4334/demos")
    user=> (def conn (d/connect "datomic:free://192.168.99.100:4334/demos"))
    user=> (def db (d/db conn))

You can access your databases through this URIs

    datomic:free://localhost:4334/<DB_NAME>
    or
    datomic:free://192.168.99.100:4334/

If your Docker host differs from localhost, you have to specify the hostname or
IP through the environment variable ALT_HOST

    docker run -d -p 4334-4336:4334-4336 -e ALT_HOST=<DOCKER_HOST> --name datomic-free datomic:1

and access your databases through the URIs

    datomic:free://<DOCKER_HOST>:4334/<DB_NAME>

The image exposes two volumes, one `/data` and one `/log` volume. If you give
your containers names like in the commands above, you don't have to bind 
something to the `/data` volume. Your data will be preserved over container
restarts. But it is recommended to use a volume container or host directory as
described in [Managing Data in Containers][4].

[1]: <https://my.datomic.com/downloads/free>
[2]: <https://registry.hub.docker.com/u/library/java/>
