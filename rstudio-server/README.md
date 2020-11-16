# How to build
WARNING: Don't use variables in a production environment. Used args in build command can be seen with the docker history command.
```bash
docker build . --build-arg r_username=<username> --build-arg r_password=<password> --tag rstudio-server:latest
```