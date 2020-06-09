# What it is

This is a laravel framework project which runs in Docker to enable fast local development. 

## Getting Started With Docker

Assuming you have docker installed and this project cloned.
In the root you will find a docker-compose.yaml file. In here you will see 
`MYSQL_DATABASE: laravel` and `MYSQL_ROOT_PASSWORD: LaravelP@ssword`

You should change the password and take note of the `MYSQL_DATABASE` value.

Copy **.env.example** from the root and save as  **.env** in the root and update the `DB_USERNAME` & `DB_PASSWORD` var 
information & ensure that `DB_DATABASE` matches the value in `MYSQL_DATABASE`

Now lets start the services by running the following command:

```
docker-compose up
```

Once the project is up and running, we need to generate the key for the app:

```
docker-compose exec app php artisan key:generate
```

If you visit `http://localhost` at this point you should see the app running.

So now we should add our DB user to be able to access the database and run migrations. In your terminal:

```
docker-compose exec db bash
```
You should now be in a bash shell, run the following:

```
mysql -u root -p
```
When prompted for the password, enter the one you used in the docker-compose file - `MYSQL_ROOT_PASSWORD`

Once logged in you should be able to run `show databases;` and see a list of databases, the name of your `MYSQL_DATABASE` 
should be seen. This should also be the same name in your env file for the key `DB_DATABASE`.

Replace `laraveluser` & `laravel_db_password` with the values you entered into the **.env** file earlier

```
GRANT ALL ON laravel.* TO 'laraveluser'@'%' IDENTIFIED BY 'laravel_db_password';
```

Now flush the DB privaleges

```
FLUSH PRIVILEGES;
```

and exit mysql

```
EXIT
```

and the container

```
exit
```

### Migrations

If you followed the above you can now run migrations:

```
docker-compose exec app php artisan migrate
```

### composer

You can use the container to install or update packages with composer using docker

```
docker-compose run --rm composer update
```
