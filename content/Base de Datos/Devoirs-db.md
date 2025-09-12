services:

  db:
```

    build: ./db
    restart: always
    environment:
      POSTGRES_PASSWORD: 1234
    ports:
      - 5432:5432
    volumes:
      - ./db/data:/var/lib/postgresql/data
```
 
(es el docker-compose.yml)
 
y el fichero que hay que tener para tener automáticamente la base de datos y el usuario creado
 
cat db/docker-entrypoint-initdb.d/1-database-and-user-creation.sh
#!/bin/bash
set -e
psql -v ON_ERROR_STOP=1 --username "$POSTGRES_USER" --dbname "$POSTGRES_DB" <<-EOSQL
 
        CREATE USER st WITH PASSWORD 'stdemo';

        CREATE DATABASE dvdrental OWNER st;

        \connect dvdrental;

        GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO st;

        GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO st;

EOSQL
 