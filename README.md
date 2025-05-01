# **em-docker-compose**

This project sets up a **.NET Core** , **Laravel** and **Nuxt.js (Vue.js)** application using **Docker Compose** and serves them via **Nginx**. Also has official **.NET Runtime, MYSQL Official, SQL Server and Elasticsearch** images. Below are the details on how to set up and use this environment.

---

👤 Maintainer

Ali Emin Çomoğlu

This guide should help you set up and run the em-docker-compose project. If you encounter any issues, feel free to open an issue on the GitHub repository.
I'd be happy to help you any way i can.

## **📁 Project Structure**

```
em-docker-compose/
├── dotnet-core-app/
│   ├── Dockerfile
│   └── ... (other .NET project files)
├── laravel-app/
│   ├── Dockerfile
│   └── ... (other Laravel project files)
├── vue-app/
│   ├── Dockerfile
│   ├── .output/
│   │   └── public/
│   └── ... (other Nuxt.js files)
├── nginx/
│   ├── dotnet/
│   │   └── default.conf
│   ├── laravel/
│   │   └── default.conf
│   └── vue/
│       └── default.conf
├── docker-compose.yml
└── LICENSE
```

- **dotnet-core-app/**: Contains the .NET Core application.
- **laravel-app/**: Contains the Laravel application.
- **vue-app/**: Contains the Nuxt.js application.
- **nginx/**: Stores the Nginx configuration files.
    - **dotnet/default.conf**: Nginx configuration for the Dotnet app.
    - **laravel/default.conf**: Nginx configuration for the Laravel app.
    - **vue/default.conf**: Nginx configuration for the Nuxt.js app.
- **docker-compose.yml**: Defines the services for Docker Compose.
- **LICENSE**: MIT license information.

---

## **🔧 Prerequisites**

- Install [Docker](https://www.docker.com/) and [Docker Compose](https://docs.docker.com/compose/)

---

## **🚀 Setup & Usage**

### **1️⃣ Clone the Repository**

```bash
git clone https://github.com/Emincmg/em-docker-compose.git
cd em-docker-compose
```

### **2️⃣ Prepare Laravel & Nuxt.js Apps**

#### **.NET Core**
- Build the **SQL Database (named: sqlserver)** image and provide necessary details to build the image correctly. Check out the documentation of SQL Server installation and configuration [here](https://learn.microsoft.com/en-us/sql/linux/quickstart-install-connect-ubuntu).
- Structure your laravel app under `dotnet-core-app` folder.

#### **Laravel**
- Create the **.env** file in `laravel-app/` and configure it.
- Structure your laravel app under `laravel-app` folder.

#### **Nuxt.js**
- Create the **.env** file in `vue-app/` and configure it.
- Build the production output. It's pre-configured for Nuxt SSR. Build it with `npm run build`.

### **3️⃣ Start Docker Services**

```bash
docker compose up -d --build
```

This command builds and runs the services in the background.

## **⚠️ Notes**

- **Database:** This setup does not include database services. Be sure to add them as required. SQL Server & MYSQL was added by default.
- **SSL/TLS:** By default, HTTP is used. See Nginx configs if SSL/TLS is needed.
- **Restarting Services After Updates:**

```bash
docker compose down
docker compose up -d --build
```

---

## **📜 License**

This project is licensed under the MIT License. See the [LICENSE](https://github.com/Emincmg/em-docker-compose/blob/main/LICENSE) file for details.

---

This guide should help you set up and run the **em-docker-compose** project. If you encounter any issues, feel free to open an issue on the [GitHub repository](https://github.com/Emincmg/em-docker-compose/issues). 🚀

