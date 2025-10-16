# Docker Compose Lab – Postgres + Redis + Adminer

Repo pentru tema: orchestrare multi-serviciu cu Docker Compose.

## Servicii
- **db** – `postgres:16-alpine`  
  - ENV: `POSTGRES_USER=qa_user`, `POSTGRES_PASSWORD=qa_pass`, `POSTGRES_DB=qa_db`  
  - Volum: `db-data:/var/lib/postgresql/data`  
  - Healthcheck: `pg_isready -U $POSTGRES_USER -d $POSTGRES_DB`
- **cache** – `redis:7-alpine`
- **adminer** – `adminer:latest` pe `http://localhost:8888` (container 8080)  
  - `depends_on` pe `db` cu `condition: service_healthy`

## Rulare
~~~bash
docker compose up -d
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"
docker inspect -f "{{.State.Health.Status}}" qa_db   # așteaptă healthy
~~~

## Conectare Adminer
- URL: <http://localhost:8888>  
- System: **PostgreSQL**  
- Server: **db**  
- Username: **qa_user**  
- Password: **qa_pass**  
- Database: **qa_db**

## Test SQL
În Adminer → **SQL command**:
~~~sql
CREATE TABLE tests (id INT);
~~~

Screenshot: [`proof/Adminer.png`](proof/Adminer.png)

## Reset / Curățare
~~~bash
docker compose down -v
~~~
`-v` șterge și volumul bazei (`db-data`), astfel încât fiecare rulare pornește dintr-un mediu curat.

## Troubleshooting
- **Port ocupat (5432/8888):** schimbă mapările în `compose.yaml` (ex. `5433:5432`, `8889:8080`) și rulează din nou `up -d`.
- **`db` nu devine healthy:** `docker logs qa_db`.
- **Docker engine nu pornește:** repornește Docker Desktop sau `wsl --shutdown`.
