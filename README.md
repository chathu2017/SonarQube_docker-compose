# SonarQube with Docker Compose

## By Niwantha Wickramasingha
[LinkedIn Profile](https://www.linkedin.com/in/niwantha-wickramasingha)

---

## Setting Up SonarQube

### Step 1: Install Docker on Linux

Follow these steps to install Docker on a Linux system:

1. **Update Package Information**  
   ```bash
   sudo apt update
   sudo apt upgrade -y
   ```

2. **Install Required Packages**  
   ```bash
   sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
   ```

3. **Add Docker's Official GPG Key**  
   ```bash
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   ```

4. **Add Docker Repository**  
   ```bash
   echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

5. **Install Docker Engine**  
   ```bash
   sudo apt update
   sudo apt install -y docker-ce docker-ce-cli containerd.io
   ```

6. **Start and Enable Docker**  
   ```bash
   sudo systemctl start docker
   sudo systemctl enable docker
   ```

7. **Verify Docker Installation**  
   ```bash
   docker --version
   ```

8. **(Optional) Manage Docker as a Non-root User**  
   Add your user to the Docker group:  
   ```bash
   sudo usermod -aG docker $USER
   ```  
   Log out and log back in for the changes to take effect.

---

### Step 2: Install Docker Compose

1. **Download the Latest Docker Compose**  
   ```bash
   sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   ```

2. **Apply Executable Permissions**  
   ```bash
   sudo chmod +x /usr/local/bin/docker-compose
   ```

3. **Verify Docker Compose Installation**  
   ```bash
   docker-compose --version
   ```

---

### Step 3: Set Up SonarQube with Docker Compose

#### Docker Compose File

```yaml
version: "3"

services:
  sonarqube:
    image: sonarqube
    ports:
      - "9000:9000"  # Expose port 9000 correctly
    networks:
      - sonarnet
    environment:
      SONAR_JDBC_URL: jdbc:postgresql://db:5432/sonar
      SONAR_JDBC_USERNAME: sonar
      SONAR_JDBC_PASSWORD: sonar
    volumes:
      - /home/sonarqube/sonarqube_conf:/opt/sonarqube/conf
      - /home/sonarqube/sonarqube_data:/opt/sonarqube/data
      - /home/sonarqube/sonarqube_extensions:/opt/sonarqube/extensions
      - /home/sonarqube/sonarqube_bundled-plugins:/opt/sonarqube/lib/bundled-plugins

  db:
    image: postgres
    networks:
      - sonarnet
    environment:
      POSTGRES_USER: sonar
      POSTGRES_PASSWORD: sonar
    volumes:
      - /home/postgres/postgresql:/var/lib/postgresql  # Fixed directory typo
      - /home/postgres/postgresql_data:/var/lib/postgresql/data
    container_name: postgresql  # Fixed container_name

networks:
  sonarnet:
```

---

## Credits
Created by **Niwantha Wickramasingha**  
[LinkedIn Profile](https://www.linkedin.com/in/niwantha-wickramasingha)

Feel free to reach out via LinkedIn for any support or collaboration opportunities!

