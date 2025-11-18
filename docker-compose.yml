version: '3.8'

services:
  # Contenedor 1: SonarQube para análisis de seguridad OWASP Top 10
  sonarqube:
    image: sonarqube:community
    container_name: sonarqube-security
    ports:
      - "9000:9000"
    environment:
      - SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true
      - SONAR_JDBC_URL=jdbc:postgresql://sonarqube-db:5432/sonar
      - SONAR_JDBC_USERNAME=sonar
      - SONAR_JDBC_PASSWORD=sonarpass
    volumes:
      - sonarqube_data:/opt/sonarqube/data
      - sonarqube_extensions:/opt/sonarqube/extensions
      - sonarqube_logs:/opt/sonarqube/logs
    networks:
      - devsecops-network
    depends_on:
      - sonarqube-db
    restart: unless-stopped

  # Base de datos para SonarQube
  sonarqube-db:
    image: postgres:13
    container_name: sonarqube-postgres
    environment:
      - POSTGRES_USER=sonar
      - POSTGRES_PASSWORD=sonarpass
      - POSTGRES_DB=sonar
    volumes:
      - postgresql_data:/var/lib/postgresql/data
    networks:
      - devsecops-network
    restart: unless-stopped

  # Contenedor 2: Jenkins para análisis de calidad y buenas prácticas
  jenkins:
    image: jenkins/jenkins:lts
    container_name: jenkins-quality
    ports:
      - "8080:8080"
      - "50000:50000"
    environment:
      - JENKINS_OPTS=--prefix=/jenkins
    volumes:
      - jenkins_home:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock
    networks:
      - devsecops-network
    restart: unless-stopped
    user: root

networks:
  devsecops-network:
    driver: bridge

volumes:
  sonarqube_data:
  sonarqube_extensions:
  sonarqube_logs:
  postgresql_data:
  jenkins_home:
