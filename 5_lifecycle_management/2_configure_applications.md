```terminal

docker run ubuntu
docker ps
# empty
docker ps -a
... STATUS Exited
... container exits after task completed
```

```terminal

docker run ubuntu sleep 5
docker ps
```

```terminal

FROM ubuntu
CMD sleep 5
# or CMD ["sleep", "5"]
```