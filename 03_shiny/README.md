### Use case: Host shiny apps

* Create apps in an environment like this one:

```bash
docker run -d --name app \
    -e ROOT=true \
    -e PASSWORD=123 \
    -p 8787:8787 \
    rocker/verse
```

* Move the apps to /srv/shinyapps/

```bash
# https://hub.docker.com/r/rocker/shiny
docker run --rm -d -p 3838:3838 \
    -v /srv/shinyapps/:/srv/shiny-server/ \
    -v /srv/shinylogs/:/var/log/shiny-server/ \
    rocker/shiny
```

Access the apps at <http://ipv4:3838/appdir>, e.g.:

* <http://174.138.4.109:3838/app1/>
* <http://174.138.4.109:3838/app2/>

Resources:

* <https://hub.docker.com/r/rocker/shiny>
