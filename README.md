<p align="center">
    <img width="25%" src="./logo.svg">
</p>

# ACME Proxy

This repository contains the configuration of my ACME (Automatic Certificate Management Environment) proxy that forwards ACME HTTP-01 challenge requests on specific domains to other hosts on my local network. It also automatically bans all IPs of clients that request anything else than the ACME challenges. That setup allows me to automatically issue Let's Encrypt certificates on any host in my local network and access those hosts over HTTPS with valid certificates without having to expose them to the internet directly.

## How does it work?

The setup consists of mutliple docker containers:

- The [Nginx](https://hub.docker.com/_/nginx) docker container acts as reverse proxy that redirects all requests on port 80 to the configured upstream hosts if the domain name matches. It also logs any access to a shared logfile.
- The [Fail2ban](https://hub.docker.com/r/crazymax/fail2ban) docker container monitors the shared logfile and blocks IPs that request anything else than a ACME challenge.
- Finally, the [Watchtower](https://hub.docker.com/r/containrrr/watchtower) docker container keeps all the running containers up-to-date by automatically restarting them when a new image of a container is available.

## How to get started?

My setup consists of two hosts in the local network that are available over two different domains. If you want a similar setup, all you have to do is add the domain names and correspoding IP addresses to a file called `.env` in the root of the repository (there is an exmaple file called `.env.example` to get you started). If you want to change that setup and add more local hosts, just edit the configuration of Nginx directly (found in `nginx/config/default.conf.template`).

After that you can start the containers by either running `docker-compose up`.

You can also configure slack webhook urls for the Fail2ban and Watchtower containers in the `.env` file. By doing so the containers will post a message to the slack channels when a IP address is blocked or a docker container is updated.

## How to contribute?

If you found a bug or have an idea on how to improve the setup, feel free to send me a pull request or open an issue. Same if you have a question or need help with the setup. And if you would like to support me, you can [buy me a beer](https://www.buymeacoffee.com/raeffs).

</br>
<p align="center">
    <a href="https://www.buymeacoffee.com/raeffs">
        <img width="10%" src="./beer.svg">
    </a>
</p>

## Attributions

Icon made by <a href="http://www.freepik.com/" title="Freepik">Freepik</a> from <a href="https://www.flaticon.com/" title="Flaticon">www.flaticon.com</a>
