```terminal

docker run ubuntu
docker ps
# empty
docker ps -a
... STATUS Exited
... container exits after task completed
```

## Hardcoded params
```terminal

docker run ubuntu sleep 5
docker ps
```

```terminal

FROM ubuntu
CMD sleep 5
# or CMD ["sleep", "5"]
```

## Entrypoint
```terminal
FROM ubuntu
ENTRYPOINT ["sleep"]
CMD ["5"] # default value
```

```terminal
docker build -t ubuntu-sleeper .
docker run ubuntu-sleeper 10
docker run --entrypoint sleep2.0 ubuntu-sleeper 10
```