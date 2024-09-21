# Installation, Part 6

## Crowdsec: Connecting to the CAPI
If you set up everything for the Crowdsec integration, then you'll want to run that command you saved:
```bash
sudo cscli console enroll -e context (key)
```
But first you need to realize that you need to run this inside the Docker container, not on the host.

To do that, we're going to exec into the container by name:
```bash
sudo docker exec crowdsec cscli console enroll XXXXX
```
where `XXXXX` is your key from above. If all works, then you should be able to see the enrollment attempt in your Crowdsec console. Accept it and everything should be good to go.


## Crowdsec: Updating
In addition to periodically updating the Crowdsec container and Traefik plugin, you also have to manually invoke ruleset updates inside the container. Thankfully this can be accomplished pretty easily with a simple cron job.

Assuming you are on a modern Linux host, you should be able to create a cron job with:
```bash
crontab -e
```
Choose an editor if you haven't already set one up, and then paste this at the bottom of the file:
```bash
0 0 * * * docker exec crowdsec cscli hub update && docker exec crowdsec cscli hub upgrade
```
This will run the command at midnight every day. I'm not sure how often this should be run, but I've been running it daily and it completes in a few seconds, but typically only updates something once or twice a week.

