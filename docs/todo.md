# TODO: Known remaining work

## Fix BOUNCER_VERSION
It's apparently impossible to feed an environment variable into the `traefik-config.yml` file. So there's no way to automate updates to that, yet. I have some ideas but so far they all involve writing a shell script as a new entry point for Traefik that can query the version and manually update the yaml file using sed.

It's also possible this is just a quirk of this config or the program versions I'm using.

