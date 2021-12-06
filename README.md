# SCTR Orchestra

This repository is part of a group project done for the ['pattern' course](https://www.bth.se/utbildning/program-och-kurser/kurser/20232/BR4QJ/) at Blekinge Institute of Technology.

Apart from docker setup-related files, it includes code from all other project-related repositories as [Git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules). For more information on each of these, see their respective repositories:

* [pattern-db](https://github.com/joki20/pattern-db)
    + note that this submodule is nested within the pattern-backend submodule
* [pattern-backend](https://github.com/datalowe/pattern-backend)
* [pattern-customerapp](https://github.com/fahlstrm/pattern-customerapp)
* [pattern-adminweb](https://github.com/fahlstrm/pattern-adminweb)
* [pattern-customerweb](https://github.com/fahlstrm/pattern-customerweb)
* [pattern-scooter](https://github.com/jannikarlsson/pattern-scooter)

Note that, in order to include the submodule's code, when cloning the repository you need to run `git clone --recurse-submodules git@github.com:datalowe/pattern-backend.git`. If you accidentally ran a 'normal' `git clone` without the recursion flag, running `git submodule update --init --recursive` should help. In order to get the very latest version/commit of every submodule, you can run `git submodule foreach 'git pull'`. Note however that this might take you 'out of sync' with this repository, as it has each submodule 'pinned' at a certain commit.

## Build and run with Docker
```bash
cd /path/to/this/dir

# depending on your host OS, you might need/prefer to use `docker compose up` instead.
# note the '--build' flag which ensures that images are always built, rather than
# containers being run based on pre-existing, possibly outdated, images.
docker-compose up --build
```
Note that this command runs in 'attached' mode, meaning the terminal will show output logs from both the database and the Laravel/backend containers. This is useful to keep track of when all containers (especially the database) have finished loading and you should be able to start using the system. If you want to run in detached mode instead, use `docker-compose -d up --build`.

To bring containers down and delete them, run (after hitting 'ctrl+C'/'Cmd+C' if you ran in 'attached mode')
```bash
docker-compose down
```

If you want to tidy up and remove all built/pulled images as well, run
```bash
docker-compose down --rmi all
```

Once everything has started up (again, especially the database), you can reach the different parts of the project as follows:
* `localhost:8000`: direct access to backend (e. g. `localhost:8000/api/scooters`)
* `localhost:4201`: customer app (for hiring scooters) TODO: deactivated at the moment, see docker-compose.yml
* `localhost:4202`: customer 'web' interface (for tasks like reviewing previous trips)
* `localhost:4203`: administrator web interface
* `localhost:5000`: only make _a single request_ to this endpoint in order to trigger scooter simulation (after a short delay, you should be able to see changes happening in admin/customer interface maps)
