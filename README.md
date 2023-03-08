## Gremcat Application

This document walks through the process of using [Docker Compose](https://docs.docker.com/compose/install) to run a database backend and application front-end for [GremCat](https://github.com/CAT-SDK/GremCat).  It is based on the tutorial [here](https://wkrzywiec.medium.com/how-to-run-database-backend-and-frontend-in-a-single-click-with-docker-compose-4bcda66f6de).

To try it out, after installing docker compose, clone our demo repository and run docker compose.
```
git clone gremApp
cd gremApp
docker compose up -d
docker logs gremcat-app
```
Then point a browser to `http://<hostname>:4200/lab?token=<token>` to interact.  The last command above shows the gremcat-app container logs, which contain the token required for login.

Internally, this works by composing a "standard" mysql server with a modified [jupyter-notebook](https://jupyter-docker-stacks.readthedocs.io/en/latest/index.html).  Our modifications use the `gremcat-mysql/Dockerfile` to install the `ideas-uo` package, which makes its python packages (`gitutils` and `patterns`) available inside the container.

Inside the ipython notebook, try fetching a project from the database using
```
from patterns.fetcher import Fetcher
F = Fetcher('ideas-uo', project_url="https://github.com/HPCL/ideas-uo.git")
F.fetch()
```

Note that the above command fails because the docker container does not come pre-populated with data.

To add data, HPCL/ideas-uo suggests using a command-line interface inside gitutils:
```
python3 -m src.gitutils.db_interface --add_project "https://github.com/hypre-space/hypre.git"
```

However, this is also broken at present due to some incorrect import paths (continaining src/) within `src/gitutils/`.


To stop and clean up all images:
```
docker compose down --rmi all
```
The `--rmi` flag will delete the machine images, which is necessary if you have modified the Dockerfiles or docker-compose.yml file.

The mysql database saves its data to a persistent docker volume, even if you delete the image.  If you have modified the mysql database environment or want to start it up again with a clean database, you'll need to delete the volume containing your database:12
```
docker volume rm gremcat-docker_gremcat-data
```
