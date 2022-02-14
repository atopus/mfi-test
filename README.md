# README

This is a test project, combining the following stacks:

* django / django-rest-frawework / postgres
* React
* Leaflet / React-Leaflet
* docker, docker-compose

## Data

The [peak sources github repo](https://github.com/open-peaks/data) has been used to populate the database.

## Development

See the respective README.md in the `backend` and `front` submodules.

### Frontend

From the command line, move to the `front` folder, and start the front server with:

```sh
$ cd front
$ npm start
```

## Deployment

_Requirements: docker & docker-compose


1. Clone the code from github with its `backend` and `front` submodules:

```
$ git clone https://github.com/atopus/mfi-test.git --recursive
```

2. Set the right url for the front app: in `docker-compose.yml`, set the REACT_API_HOST and the REACT_API_PORT according to the public url from which your application will be exposed. E.g.

```yaml
  environment:
    - REACT_APP_API_HOST=peak.mysite.com
    - REACT_APP_API_PORT=80
```

Then run the docker-compose command as a daemon:

```
$ docker-compose up -d
```

3. Add a proxy-pass to the `nginx` container. The application comes with an `nginx` container which takes care of the redirection to the `backend` and the `front` containers, as well as the static files.

For instance, if an nginx server is used in front of your server and your public url is peak.mysite.com, you may use:

```
server {
    listen 80;

    server_name peak.mysite.com;

    access_log /var/log/nginx/peaks.log;
    error_log /var/log/nginx/peaks.log;

    location / {
        proxy_pass http://localhost:8080/;
    }
}

```

> In order to change to the port to anything other than `8080` - for instance to `8091`, please update also the port settings in `docker-compose` to "8081:8080".

## TODO

* [x] Debug the git submodule cloning;
* [ ] Debug the popup when creating a peak: for now, the markup created with a single clic on the map needs to be clicked again twice in order to display the popup;
* [ ] Review the peak displaying for improved stability;
* [ ] Add validators and error catching;
* [ ] Add success feedback messages when creating, updating or deleting a peak;
* [ ] Give access to the django admin portal;
* [ ] Tag the releases;
* [ ] Show data credits from the map or the popup.
