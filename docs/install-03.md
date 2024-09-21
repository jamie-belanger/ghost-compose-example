# Installation, Part 3: Crowdsec
Crowdsec integration is optional.

The purpose of this integration point is to add crowdsourced threat assessments to your Ghost blog, so that your host machine can automatically reject incoming connections from known bad IP addresses and detect new threats&mdash;and block them, and contribute that knowledge back to the crowd.


## Configuring Crowdsec
Start by setting up your account with them at: https://app.crowdsec.net/

### CAPI Key
Their free accounts have limitations, but nothing too annoying for a small self-hoster like me. What you want to do is add a new "Security Engine" using the button here:

![Crowdsec Console](./images/crowdsec-1.jpg)

This will bring you to a screen telling you how to enroll your security engine. The part you really need is the bash command near the bottom. Something like:
```bash
sudo cscli console enroll -e context (key)
```
You'll need this, as it's a key that will link your Crowdsec installation to your Crowdsec account. For now, just put this somewhere safe. I haven't yet tried to make this step automatic. Sorry.

This is your CAPI key&mdash;the secret value that is used for communicating from your host to their **Central API**.

> NOTE: The rest of the Crowdsec config is not too difficult. You don't have to worry about most of it because this config handles loading default rules for both Traefik and a Linux host. But you should review the Blocklists. They let you subscribe to three as a free user. Pick any that seem good to you.


## LAPI Key
You also need a LAPI Key&mdash;for communicating via a **Local API**. This is super easy because you have total control over it. There are two places this value has to go, and I've wired up the environment variable to both for you.

Look at the Crowdsec section of the environment file:
```ini
# Part 3: Crowdsec
## TODO: CAPI key, for your account

## Traefik plugin version (so you don't have to manually edit traefik-config.yml)
## Check https://github.com/maxlerebourg/crowdsec-bouncer-traefik-plugin
BOUNCER_VERSION="v1.3.3"

## Bouncer key for Traefik's local API
## Generate something like a GUID for this. It's used internally by Traefik
## to querying Crowdsec to see if incoming traffic should be bounced.
BOUNCER_KEY_TRAEFIK=(generate)
```

You can type anything here, but it will be more secure if you generate a GUID. In Linux or Mac, use:
```bash
uuidgen
```
and on Windows / Powershell, you can use
```powershell
New-Guid
```
Paste the value in and you're set.


## Removing This Integration
The two Traefik config files handle the entirety of this integration. You can comment or delete any sections that have the word `crowdsec` in them.
