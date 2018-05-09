### Bash running
```bash

// First Build image
cd Build

docker build -t php56-apache:201805091312 .

mkdir app && cd app

docker-compose up -d

```