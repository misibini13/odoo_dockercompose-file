Odoo Docker Setup (Multiple Instances)

This repository contains a docker-compose.yml file for deploying two Odoo instances (odoo1 and odoo2) with a shared PostgreSQL database. It enables running multiple Odoo containers on different ports while sharing a common data volume.
📌 Features

✅ Deploys two Odoo instances (odoo1 and odoo2).
✅ Uses a shared PostgreSQL database (db).
✅ Persistent storage for Odoo and database data.
✅ Supports custom addons directory for extra modules.
✅ Exposes Odoo instances on different ports (8069 & 8070).

/your-project-directory
│-- addons/               # Custom Odoo addons (mounted in both containers)
│-- docker-compose.yml    # D

⚙️ How It Works

    odoo1 runs on port 8069 (http://localhost:8069).

    odoo2 runs on port 8070 (http://localhost:8070).

    Both Odoo instances connect to the same PostgreSQL database (db).

    addons/ folder is mounted in both Odoo containers for custom modules.

    Data for Odoo and PostgreSQL is persisted using Docker volumes.

🚀 Deployment Instructions
1️⃣ Clone the Repository

git clone https://github.com/your-username/your-repo.git
cd your-repo

2️⃣ Create Required Directories

mkdir -p addons

(Optional: Add your custom Odoo addons here)
3️⃣ Start the Containers

docker-compose up -d

    Runs Odoo 1st instance on http://localhost:8069

    Runs Odoo 2nd instance on http://localhost:8070

4️⃣ Access Odoo Instances
Instance	URL
Odoo 1	http://localhost:8069
Odoo 2	http://localhost:8070

Default Login Credentials:

    Email: admin

    Password: (set during Odoo installation)

5️⃣ Stop & Remove Containers

docker-compose down

This stops and removes the containers but keeps the database and Odoo data intact.
📌 Configuration Details
1️⃣ Odoo Services (odoo1 & odoo2)

  odoo1:
    image: odoo:latest
    container_name: odoo1
    depends_on:
      - db
    ports:
      - "8069:8069"
    volumes:
      - odoo-data:/var/lib/odoo
      - ./addons:/mnt/extra-addons
    environment:
      - HOST=db
      - USER=odoo
      - PASSWORD=odoo

    Runs Odoo instance odoo1 on port 8069.

    Mounts a persistent volume for Odoo data.

    Uses addons/ directory for custom modules.

  odoo2:
    image: odoo:latest
    container_name: odoo2
    depends_on:
      - db
    ports:
      - "8070:8069"
    volumes:
      - odoo-data:/var/lib/odoo
      - ./addons:/mnt/extra-addons
    environment:
      - HOST=db
      - USER=odoo
      - PASSWORD=odoo

    Runs Odoo instance odoo2 on port 8070.

    Uses the same PostgreSQL database (db).

2️⃣ Database Service (db)

  db:
    image: postgres:15
    container_name: odoo-db
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_USER=odoo
      - POSTGRES_PASSWORD=odoo
    volumes:
      - db-data:/var/lib/postgresql/data

    Uses PostgreSQL 15 as the database.

    Stores data in a persistent volume (db-data).

    Credentials: User: odoo | Password: odoo

3️⃣ Volumes for Data Persistence

volumes:
  db-data:
  odoo-data:

    db-data → Stores PostgreSQL data.

    odoo-data → Stores Odoo data.

🔧 Customization
Change Database Credentials

Modify the POSTGRES_USER and POSTGRES_PASSWORD values in the db service.
Add Custom Addons

Place additional Odoo modules inside the addons/ directory.
Change Ports

Modify the ports section of odoo1 or odoo2 to avoid conflicts.
🛠️ Troubleshooting

1️⃣ Database Connection Error?
Run:

docker-compose logs db

Check if PostgreSQL is running correctly.

2️⃣ Odoo Not Loading?
Try restarting the containers:

docker-compose restart

3️⃣ Remove Everything (Data Included)

docker-compose down -v

This will remove all data, including the database.
📜 License

This project is open-source. Feel free to modify and use it.
