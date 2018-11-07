# SilverStripe on Platform.sh

Starter kit for a new [SilverStripe](http://silverstripe.org) 4.X project on [Platform.sh](https://platform.sh)

## 1. Git setup

Clone this repo locally and remove the origin.

```bash
git clone git@github.com:ericgsmith/platformsh-template-silverstripe.git project-name
cd project-name
git remote rm origin
```

Create a new Platform.sh project and when asked if you want to create a blank site from a template or import your existing code. Select "Import your existing code".

Add the platform remote from the information displayed in the UI.

## 2. Local setup

The SilverStripe installer needs write access to files that should be read only on the server, so you will need do a fresh install locally first.

Install dependencies locally by running `composer install` and then `composer vendor-expose`.

Follow the standard SilverStripe installation process on your local machine.

Once the process have finished and the `app/_config.php` file has been created by the installer, add the following to the start of the file.

```php
require_once __DIR__ . '/_platformsh.php';
```

This will set the database connection information. 

Add and commit this change the to repo and push to Platform.sh.

## 3. Import DB

Create a DB dump from your local install.

### With Platform CLI

```bash
platform sql < dbfile.sql
```

### Without the Platform CLI

1. SCP the file to the Platform environment.

2. SSH into the Platform environment.

3. Import the DB.
```bash
mysql -h database.internal -P 3306 -u user main < dbfile.sql
```

## 4. Platform.sh Variables

### Admin User

Set [Environment Variables](https://docs.platform.sh/configuration/app/variables.html#variables) via Platform to set the default username and password:

- SS_DEFAULT_ADMIN_USERNAME
- SS_DEFAULT_ADMIN_PASSWORD

### Platform CLI

The Platform CLI needs to run on the Platform environment to create snapshots and redeploy to generate the SSL certs.

Follow the instructions on [Automating the CLI on a Platform.sh environment](https://docs.platform.sh/gettingstarted/cli/api-tokens.html#automating-the-cli-on-a-platformsh-environment) to generate and save the token.
