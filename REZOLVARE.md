\# Rezolvare – Docker Compose Lab



\- Servicii:

&nbsp; - `db` – `postgres:16-alpine` cu volum `db-data` și `healthcheck` (`pg\_isready`)

&nbsp; - `cache` – `redis:7-alpine`

&nbsp; - `adminer` – `adminer:latest` (port 8888→8080)

\- `adminer` depinde de `db` cu `condition: service\_healthy`.



\## Pași de rulare

1\. `docker compose up -d`

2\. Verificare health:

&nbsp;  - `docker ps`

&nbsp;  - `docker inspect -f "{{.State.Health.Status}}" qa\_db` → `healthy`

3\. Adminer: http://localhost:8888  

&nbsp;  Server: `db`, User: `qa\_user`, Pass: `qa\_pass`, DB: `qa\_db`

4\. SQL:

&nbsp;  ```sql

&nbsp;  CREATE TABLE tests (id INT);



