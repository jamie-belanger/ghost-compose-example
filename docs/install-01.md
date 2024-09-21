# Installation, Part 1
Start by cloning this repository. Then make a copy of the `.env-default` file and name it `.env`. This will be automatically included by the compose file and injected into every container as a set of environment variables. All the various containers and config files use settings in this, so it's important to get this set up right.

The config file is simple enough, using a basic INI type syntax
```ini
# General config
TIME_ZONE=America/New_York
```

Any line starting with a `#` is commented out. The rest are key/value pairs. You can see how they are used throughout this project by searching for the key value.

I've taken the liberty of setting up this file organized with Steps to match these installation docs. Start with the **Step 1** area and fill it out. There are currently three variables here:
```ini
# Step 1: General config
## Your server's time zone. Ghost uses this, or falls back to UTC
TIME_ZONE=America/New_York
## Blog / site's domain (used in Cloudflare challenges)
BLOG_DOMAIN=example.com
## Actual endpoint for your blog (ie "blog.site.com" or "www.site.com")
BLOG_URL=www.example.com
```

The rest of the installation files in this folder will walk you through what info to insert for the various variables. Even if a step is noted as optional, you should still look at the file (and scroll to the bottom where I'll have a section with notes on disabling that integration).


> NOTE: Also, sorry in advance if I get any of this wrong. The vast majority of this repository was assembled from memory.
