
# Anubis

Anubis is a **proof-of-work reverse proxy** designed to protect web services from bots. It sits between your reverse proxy (like Nginx) and the target service (API, website...).

---

Setup with Docker Compose

### Project Structure
```
.
├── anubis-config
├── default.conf
├── docker-compose.yml
├── README.md
└── www
    └── index.html

```


### `docker-compose.yml` File

[docker-compose.yml](Anubis/docker-compose.yml) file configuring the Anubis and Nginx services.

### Nginx Configuration (`default.conf`)

The Nginx configuration file allows to listen on port 80 ([default.conf](Anubis/default.conf)) and should contain the following directives to function as a reverse proxy to Anubis:

- listens on port 8923

- forwards requests to Anubis (port 8923)

- injects the required headers (X-Real-IP and X-Forwarded-For)


### Starting the Services

Ensure that Docker and Docker Compose are installed on your system.

In the directory containing your `docker-compose.yml` file, run the following command to start the services in the background: 

```bash
docker compose up -d
docker compose logs -f # Following logs during test
```


---

### Accessing the Service

Once the services are up and running, you can access your application through the browser at the following address:

```
http://localhost:8923
```

Requests will go through Nginx, then through Anubis, before reaching the target service.

## Simulate a simulated attack with `nikto`

**Nikto** is a web vulnerability scanner that can be used to conduct a security audit on your web server. It can simulate a series of attacks, such as injection attempts or malicious requests, allowing you to test Anubis's ability to block these attacks.

#### Prerequisites:

- **Install Nikto** on your local machine if it is not already installed.

  ```bash
  sudo apt install nikto
  ```

#### Using Nikto:

 **Run a Nikto scan** against your local Anubis instance.

   Example command to scan the URL `http://localhost:8923`:
   ```bash
   nikto -h http://localhost:8923
   ```

   This command will send various HTTP requests simulating attacks (SQL injection, XSS, etc.) and observe how the server reacts.


Test using `curl`

```sh
curl -i -A "AmazonBot" http://localhost:8923 
curl -i -A "ChatGPT-User" http://localhost:8923
curl -i -A "Bot" http://localhost:8923 
```  



   ---


*Copyright Hélène Finot - Formation DevOps 2025*