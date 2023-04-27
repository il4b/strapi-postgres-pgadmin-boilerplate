# Strapi 4, PostgreSQL, and pgAdmin4 Boilerplate with Docker Compose

This open-source boilerplate provides a minimal setup to start a project using Strapi 4, PostgreSQL, and pgAdmin4 with Docker Compose. 

The project includes:
* a PostgreSQL database, 
* a pgAdmin4 for managing the database, 
* and a Strapi 4.10 backend service. 

The setup ensures that you can begin your project with minimal configuration.

## Prerequisites

- Make sure you have [Docker](https://www.docker.com/) and [Docker Compose](https://docs.docker.com/compose/) installed on your system. 

This boilerplate is designed to work with Docker and does not require a separate installation of Node.js.

## Services

### PostgreSQL Database (database)

The PostgreSQL database service is configured to store its data in the `./database/.data` directory, which is mounted as a persistent volume. This allows the database to persist between container restarts. To relaunch the installation process, you must remove the `database/.data` folder. For security reasons, the `database/.data` directory is not added to the repository and is explicitly excluded in the `.gitignore` file. If you rename the directory, make sure to update the references in the `.gitignore` file and any other locations where it is mentioned.

[Official PostgreSQL Documentation](https://www.postgresql.org/docs/)

### pgAdmin4 (pgadmin)

pgAdmin4 is a web-based administration tool for managing PostgreSQL databases. The pgAdmin4 service is configured to use a non-persistent volume, as it is not necessary to maintain its state between container restarts in most cases. You can access pgAdmin4 at `http://localhost:8080` and log in using the email and password set in the `.env` file.

[Official pgAdmin4 Documentation](https://www.pgadmin.org/docs/pgadmin4/)

### Strapi Backend Service (backend)

The Strapi backend service is a Node.js application built using Strapi 4. The service uses Node.js version 18.3.16 due to a compatibility issue with Strapi, which does not support Node.js versions higher than 18.3.16. The backend service is accessible at `http://localhost:1337`, which serves as the API endpoint that can be used in a frontend app or with a tool like Postman. The Strapi admin panel can be accessed at `http://localhost:1337/admin`.

[Official Strapi Documentation](https://strapi.io/documentation/developer-docs/latest/)

## Steps to set up the project

1. **Fork the repository**: Click the "Fork" button at the top right corner of the repository page to create a copy of the repository in your own GitHub account.

2. **Clone the forked repository**: Clone the forked repository to your local machine using the command `git clone https://github.com/<your_username>/strapi-postgres-pgadmin-boilerplate.git`, replacing `<your_username>` with your GitHub username.

3. **Set up environment variables**: The project uses environment variables to configure the services. You'll find a `.env.dist` file in the root directory of the project. Create a copy of this file and name it `.env`. Update the values of the variables in the `.env` file as needed.

4. **Configure the default database and server for pgAdmin4**: The `database` directory contains a `servers.json.dist` file. Create a copy of this file and name it `servers.json`. Update the values in the `servers.json` file to configure the default database and server for pgAdmin4. This will allow you to access the database without having to manually set it up in pgAdmin4.

5. **Set up Strapi 4**: To set up Strapi 4, run the following commands using Docker Compose to start the Strapi installation CLI:

```bash
docker-compose run --rm backend npx create-strapi-app@latest ./
```

Follow the prompts:

1. Choose 'yes' to proceed.
2. Choose 'custom (manual settings)' for the installation type.
3. Choose between 'JavaScript' and 'TypeScript' for your preferred language.
4. Choose 'postgres' as your default database client.
5. Set the database name to the same value as in the `.env` file.
6. Set the host to 'database', the name of the service in the Docker Compose file.
7. Leave the port as '5432' or change it if you modified it in the Docker Compose file.
8. Set the username and password to the same values as in the `.env` file.
9. Choose 'N' for SSL connection, as it is for a development environment only.

Do not put the `--quickstart` option because this won't allow you to do the manual install to change the database, as Strapi goes with SQLite by default.

For more options, refer to the [create-strapi-app documentation](https://www.npmjs.com/package/create-strapi-app?activeTab=code).

No prompt example :
```bash
docker-compose run --rm backend npx create-strapi-app@latest ./ --use-npm --ts --dbclient postgres --dbhost database --dbport 5432 --dbname backend --dbusername admin --dbpassword admin --dbssl no
```

The script will install the project and its dependencies.

6. **Start the services**: Run the following command in the terminal from the root directory of the project to start the services:

```bash
docker-compose up -d
```

This command will start the PostgreSQL database, pgAdmin4, and the Strapi 4 backend service. The `-d` flag is used to run the services in detached mode, allowing you to continue using the terminal.

7. **Access the services**: After starting the services, you can access them using the following URLs:

   - **pgAdmin4**: Open your web browser and go to `http://localhost:8080`. Log in using the email and password you set in the `.env` file.
   
   - **Strapi 4 backend service**: Open your web browser and go to `http://localhost:1337`. This is the API endpoint for the backend service, which can be used in a frontend app or with a tool like Postman.

   - **Strapi admin panel**: Open your web browser and go to `http://localhost:1337/admin`. If the service is running for the first time, you'll be prompted to create an admin user.

## Stopping the services

To stop the services, run the following command in the terminal from the root directory of the project:

```bash
docker-compose down
```

This command will stop and remove all containers, networks, and volumes defined in the `docker-compose.yml` file.

## Permission Issues

If you experience any issues with file permissions or accessing files, it might be because Docker creates them with a different user. To fix this issue, run the following command at the root repository:

```bash
sudo chown -R $USER:$USER ./*
```

This command will change the ownership of all files and directories in the current folder to the current user and user group.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

That's it! You're now ready to start using this boilerplate to build your project with Strapi, PostgreSQL, pgAdmin4.
