# New Standard Procedure to create Docker images of a SWISH server

As of Christmas 2022, we no longer use custom docker images, as in fact the standard SWISH image built with https://github.com/SWI-Prolog/docker-swish  suffices for our needs, ergo the deprecation of the older dockerfiles.

To ensure a specific version we use image logicalcontracts/swish-xmas2022-multi, which is simply the above built for SWISH version b6a80911995e809ac92cf75c1df7c07385caae6f (Dec 15, 2022) and with:

    docker buildx build --push --platform linux/arm/v7,linux/arm64/v8,linux/amd64 -t logicalcontracts/swish-xmas2022-multi .

You may have to execute this first:

    docker buildx create --use

Cf. https://www.docker.com/blog/multi-arch-build-and-images-the-simple-way/

# Testing a server in a developer's machine

Running for testing a server in developer's machine: 

    HACK: before the next command, set up some dir in your host for swish data, and do this once (notice that the source dir does NOT exist on your host, but it will exist inside the container):
    
    ln -s /app/swish/config-enabled /Users/mc/TaxKB_swish_data

    docker run -it -p 8082:3050 -v /Users/mc/TaxKB_swish_data:/data  -v /Users/mc/git/LogicalEnglish:/app -e LOAD_KB=true  --memory="300m" logicalcontracts/swish-xmas2022-multi

Add --bash to the end if you want to peek inside the container.

On Ubuntu server:

    chown -R 1000:1000 /home/ubuntu/TaxKB_swish_data

In  particular, it's important that the data subdirectory's user is not root, because it is used to run SWISH and thus interact with the filesystem

And finally:

    docker run -it -p 8082:3050 -v /home/ubuntu/TaxKB_swish_data:/data:rw -v /home/ubuntu/LogicalEnglish:/app:rw -e LOAD_KB=true --memory="300m"  logicalcontracts/swish-xmas2022-multi

    docker run -d --restart always -p 8082:3050 -v /home/ubuntu/TaxKB_swish_data:/data:rw -v /home/ubuntu/LogicalEnglish:/app:rw -e LOAD_KB=true --memory="300m"  logicalcontracts/swish-xmas2022-multi


