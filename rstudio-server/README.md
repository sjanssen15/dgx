# How to build
WARNING: Don't use variables in a production environment. Used args in build command can be seen with the docker history command.
```bash
docker build . --tag rstudio-server:latest --build-arg r_username=<value> r_password=<value>
```

