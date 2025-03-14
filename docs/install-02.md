# Installation, Part 2: Cloudflare
> Cloudflare configuration is optional.

The purpose of this integration point is to allow your stack to retrieve and renew Let's Encrypt SSL certificates automatically, by using Cloudflare's DNS challenge. There are other ways to retrieve SSL certificates, but I prefer this method as it gives the most flexibility&mdash;and makes it super easy to spin up additional containers and services on the host machine, because the SSL certificate you will get will be a wildcard that allows adding on whatever additional sub-domains you desire.

The good news is that a lot of the work for this is handled automagically by Traefik and the config file we give it! All you need to do is plug in a valid Cloudflare API key and it should just work.


## Configuring Cloudflare
First of all, I'm assuming you already use Cloudflare as your domain registrar / DNS service. If not, then I'm not sure if any of this would work. Note that you do NOT need a paid / Pro account; these instructions work just fine with a free tier account.

Log into your Cloudflare dashboard and go to your profile, at https://dash.cloudflare.com/profile

What you want is the key labelled `Global API Key`, circled in pink here:

![Cloudflare API Key](./images/cf-key.jpg)

You can change this value at any point (and probably should every few months) but for now just click the View button. You will be prompted for your password, as this is sensitive information. Once you see the popup with the key, copy it and paste it into the `.env` file.

The email address that goes with the key is the same address you use to log into your Cloudflare account. There is a second email address in this section called `ACME_EMAIL`. That does NOT have to match your Cloudflare account; it can be any valid email address. This is something Let's Encrypt uses to warn you about a certificate that's going to expire (which I've seen only once in however many years they've been issuing certs, and I believe they are discontinuing these soon if they haven't already).

The rest of the config needed is already done for you.


## Removing This Integration
If you don't want this integration, there are several files you'll have to modify. Start by commenting out these lines in the compose file:
```yaml
    environment:
      - CF_API_EMAIL=${CLOUDFLARE_API_EMAIL}
      - CF_API_KEY=${CLOUDFLARE_API_KEY}
```
Then delete or comment out the variables in your `.env` file.

Both of the Traefik yaml config files have a section that looks like this:
```yaml
    # Cloudflare IPs that we trust to forward the actual end-user's IP info
    ForwardedHeadersTrustedIPs:
    - 173.245.48.0/20
    - 103.21.244.0/22
    - 103.22.200.0/22
    - 103.31.4.0/22
    - 141.101.64.0/18
    - 108.162.192.0/18
    - 190.93.240.0/20
    - 188.114.96.0/20
    - 197.234.240.0/22
    - 198.41.128.0/17
    - 162.158.0.0/15
    - 104.16.0.0/12
    - 172.64.0.0/13
    - 131.0.72.0/22
```
which lists Cloudflare IP addresses that forward traffic as trusted.

There are two other sections in `traefik-config.yml` that have Cloudflare specific config. The TLS cert resolver and the config that backs that:
```yaml
    http:
      middlewares:
        - my-crowdsec-bouncer-traefik-plugin@file
      tls:
        certResolver: cloudflare
        domains:
          - main: ${BLOG_DOMAIN}
            sans:
              - "*.${BLOG_DOMAIN}"
# ...
certificatesResolvers:
  cloudflare:
    acme:
      email: ${ACME_EMAIL}
      storage: /acme/acme.json
      # comment this out once it works... and delete any certs you got
      # caserver: https://acme-staging-v02.api.letsencrypt.org/directory
      dnsChallenge:
        provider: cloudflare
        resolvers:
          - "1.1.1.1:53"
          - "1.0.0.1:53"
```
