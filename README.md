* Requirements database:
  * MySQL (MySQL >= 5.7)
  * PostgreSQL (PostgreSQL >= 9.5)
  * SQLite3 (SQLite >= 3.11)
  * SQLServer (SQLServer >= 2012)
## Step 1: Create an empty database and accompanying user

### MySQL
```sql
CREATE DATABASE redmine CHARACTER SET utf8mb4;
CREATE USER 'redmine'@'localhost' IDENTIFIED BY 'my_password';
GRANT ALL PRIVILEGES ON redmine.* TO 'redmine'@'localhost';
```
### PostgreSQL
```sql
CREATE ROLE redmine LOGIN ENCRYPTED PASSWORD 'my_password' NOINHERIT VALID UNTIL 'infinity';
CREATE DATABASE redmine WITH ENCODING='UTF8' OWNER=redmine;
```
## Step 2: Config database
```bash
cp database.yml.example database.yml
```
```R
production:
  adapter: postgresql
  database: redmine
  host: localhost
  port: 5432
  pool: 5
  username: postgres
  password: 654321

development:
  adapter: postgresql
  database: redmine
  host: localhost
  port: 5432
  pool: 5
  username: postgres
  password: 654321

test:
  adapter: postgresql
  database: redmine
  host: localhost
  port: 5432
  pool: 5
  username: postgres
  password: 654321

```
## Step 3: Dependencies installation
```bash
bundle install
```
## Step 4: Session store secret generation
```bash
bundle exec rake generate_secret_token
```
## Step 5: Database schema objects creation
```bash
set RAILS_ENV=production
bundle exec rake db:migrate
```
## Step 6: Database default data set
```bash
set RAILS_ENV=production
set REDMINE_LANG=en
bundle exec rake redmine:load_default_data
```
## Step 7: Test the installation
```bash
bundle add webrick
bundle exec rails server -u webrick -e production
```
## Step 8: Kill port used
```bash
netstat -ano | findstr :<PORT>
taskkill /PID <PID> /F
```
## Notes: Setting PurpleMine2
https://github.com/mrliptontea/PurpleMine2
<br />
To install PurpleMine, just download .zip and unpack it to your Redmine's public/themes folder.<br />
Then go to Redmine > Administration > Settings > Display and select PurpleMine2 from the list and save the changes.
