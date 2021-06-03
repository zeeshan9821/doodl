# Intial Setup 

## Repo Structure 

```
enable-dev/
  dev.nginx.conf
  docker-compose.yml
  enable-data-application/
  enable-svc-conference/
  enable-svc-messaging/
  enable-web-app/
```

## Clone the following GIT Repos

```
git clone https://github.com/AltaisCorp/enable-web-app.git
git clone https://github.com/AltaisCorp/enable-svc-messaging.git
git clone https://github.com/AltaisCorp/enable-svc-conference.git
git clone https://github.com/AltaisCorp/enable-data-application.git
```

## Install Docker

https://www.docker.com/get-started

### Execute Docker Sample Image

Verify docker is running via systray by starting the sample hello world docker image.

```
docker run hello-world
```

## Install IntelliJ Community

https://www.jetbrains.com/idea/download/

Open Project -> enable-data-application

## Install OpenJDK 15.0.2

File -> Project Structure -> SDKS -> Download JDK - 15.0.2

Run Services in IntelliJ

src/main/java/com/altais/enable/data/EnableDataApplication.java

Start Front End

change to Altais root directory

```
docker-compose -f docker-compose.yml up
```

Front-End URL

https://localhost:3000/login

## Test Application 

The following test account are available to test the Enable application.

Test User Login Accounts

- superuser@altais.com
- orgadmin@orgadmin.com
- patient@patient.com

all with the password: P@ssw0rd1

Trillio Test Users

- superuser@altais.com -> test_user
- orgadmin@orgadmin.com -> test_user_2
- patient@patient.com -> chris-test


# Useful Links

- http://media.twiliocdn.com/sdk/js/conversations/releases/1.1.0/docs/Conversation.html
- https://www.docker.com/get-started
- https://jdk.java.net/15/
- https://www.jetbrains.com/idea/download/
 

# Running enable-web-app locally with dev env services

For the gateway service block inside the `docker-compose.yml` file, replace the env var that reference local services with dev env.  

`docker-compose.yml`
```yaml
  gateway:
    ...
    environment:
      MESSAGING_API: enable-svc-messaging.devplatform.uw2.alth.us:443
      CONFERENCE_API: enable-svc-conference.devplatform.uw2.alth.us:443
      ADMIN_API: enable-svc-admin.devplatform.uw2.alth.us:443
      USERS_API: enable-svc-user.devplatform.uw2.alth.us:443
      PATIENT_API: enable-svc-patient.devplatform.uw2.alth.us:443
      REGISTRATION_API: enable-svc-registration.devplatform.uw2.alth.us:443
#      MESSAGING_API: host.docker.internal:8081
#      CONFERENCE_API: host.docker.internal:8082
#      ADMIN_API: host.docker.internal:8085
#      USERS_API: host.docker.internal:8094
#      PATIENT_API: host.docker.internal:8083
#      REGISTRATION_API: host.docker.internal:8097
      PORTAL_WEB: http://portal:3000
```

For each `location` block, make sure its `proxy_pass` directive is using `https`

`dev.nginx.conf`
```nginx
...
	location /api/user/ {
		proxy_set_header Host ${USERS_API};
#		proxy_pass http://users-api;
		proxy_pass https://users-api;
	}
...
```
