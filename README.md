# MM24 Blog

## Setup on Windows

Install these programs first:

1. [Visual Studio Code](https://code.visualstudio.com/Download)
2. [Git for Windows](https://git-scm.com/download/win)
3. [PHP 8.3–8.x](https://windows.php.net/download/) — download the x64 ZIP, extract it (for example, to `C:\php`), and add that folder to your Windows `Path`. Search for **Edit environment variables for your account**, open `Path`, and add the PHP folder.
4. [Composer](https://getcomposer.org/download/) — run `Composer-Setup.exe` and select the PHP executable when asked.
5. [Node.js LTS](https://nodejs.org/en/download/) — this installs npm, which is used when the app runs.
6. Bun — open PowerShell and run:

   ```powershell
   powershell -c "irm bun.sh/install.ps1 | iex"
   ```

7. [Docker Desktop](https://www.docker.com/products/docker-desktop/) — install and start it. The database runs in Docker.

Open a new PowerShell window and check that the tools are available:

```powershell
git --version
php -v
composer -V
node -v
npm -v
bun --version
docker compose version
```

### 1. Clone the project

```powershell
git clone https://github.com/Jannuy/learnlaravel.git
cd learnlaravel
```

### 2. Turn on PHP extensions

Find the `php.ini` file used by PHP:

```powershell
php --ini
```

Open it in Visual Studio Code. If the file does not exist, copy `php.ini-development` from the PHP folder and name the copy `php.ini`. Check that `extension_dir` points to the `ext` folder. Then enable these extensions by removing `;` at the start of each line, or add the lines if they are missing:

```ini
extension=curl
extension=fileinfo
extension=mbstring
extension=openssl
extension=pdo_mysql
```

Save the file, restart PowerShell, and check the extensions:

```powershell
php -m
```

The list should include `curl`, `fileinfo`, `mbstring`, `openssl`, and `pdo_mysql`. Laravel also needs `ctype`, `dom`, `PDO`, `session`, `tokenizer`, and `xml`.

### 3. Install dependencies

Run these commands in the project folder:

```powershell
composer install
bun install
```

### 4. Set up the environment

Copy `.env.example` to `.env` and create the app key:

```powershell
Copy-Item .env.example .env
php artisan key:generate
```

The example `.env` is set up to use the MariaDB database from Docker Compose.

### 5. Start the database and run migrations

Make sure Docker Desktop is running, then start MariaDB:

```powershell
docker compose up -d
```

Wait for the database to respond:

```powershell
docker compose exec db mariadb-admin --host=127.0.0.1 --user=root --password=example --wait=30 ping
```

Create the database tables:

```powershell
php artisan migrate
```

### 6. Run the project

```powershell
composer run dev
```

Leave it running and visit [http://localhost:8000](http://localhost:8000). When finished, stop the database with:

```powershell
docker compose down
```
