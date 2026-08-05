---
title: "RDS Database"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

### Provision Amazon RDS PostgreSQL Database

In this section, you will configure and launch an Amazon RDS PostgreSQL database instance named `aws-c8n`. This database will store the backend application's relational data. You will configure security rules, enable Password and IAM database authentication, and test connectivity.

The main goals of this section are:
* Create a PostgreSQL database instance using the AWS Free Tier.
* Configure credentials and enable both password and IAM database authentication.
* Map connectivity to a dedicated security group allowing secure access.
* Download the AWS global trust bundle and verify the connection.

---

### Step 1: Create PostgreSQL Database Instance

1. Navigate to the **Amazon RDS console** and click **Create database**.
2. Select **Standard create** (or **Full configuration**) as the database creation method.
3. For **Engine options**, select **PostgreSQL**.
4. Under **Templates**, select **Free tier** to avoid unnecessary charges.
5. In **Availability and durability**, select **Single-AZ DB instance deployment** (default for Free Tier).

![Create PostgreSQL Database](/images/5-Workshop/5.3-RDS/01-create-db-postgres.png)

---

### Step 2: Configure Instance Identifier and Credentials

1. Set the **DB instance identifier** to `aws-c8n`.
2. Set the **Master username** to `postgres`.
3. In **Credentials management**, select **Self managed**.
4. Set a strong **Master password** and confirm it.
5. Under **Database authentication options**, select **Password and IAM database authentication** (or ensure IAM authentication is enabled post-creation).
6. Under **Instance configuration**, select burstable classes (e.g. `db.t4g.micro` or `db.t3.micro` depending on availability).

![Database Credentials and Auth](/images/5-Workshop/5.3-RDS/02-db-settings.png)
![Database Authentication and Class](/images/5-Workshop/5.3-RDS/03-db-auth-instance.png)

---

### Step 3: Configure Connectivity & Security Group Rules

Once the database is created, navigate to the database details page to view the endpoint and configure connection security rules:

1. Locate the database endpoint: `aws-c8n.czsenqug2xnh.us-west-2.rds.amazonaws.com` on port `5432`.
2. Under **Connectivity & security**, verify that **IAM Authentication** is **Enabled**.
3. Under **Security group rules**, configure the assigned security group `rds-ec2-1` to allow:
   * Inbound PostgreSQL traffic (port 5432) from your administrator IP address (e.g., `14.169.24.57/32`).
   * Inbound PostgreSQL traffic (port 5432) from the EC2 instance security group (`sg-0c7a938ee4252d8e7` / `ec2-rds-1`) so the Spring Boot application can connect.

![RDS Endpoint and Connectivity](/images/5-Workshop/5.3-RDS/04-db-connectivity.png)
![RDS Security Group Rules](/images/5-Workshop/5.3-RDS/05-db-security-groups.png)

---

### Step 4: Verify Database Connection using psql

You can test connectivity to the PostgreSQL database from a terminal using the following steps:

1. Download the AWS RDS global SSL certificate trust bundle:
   ```bash
   curl -o global-bundle.pem https://truststore.pki.rds.amazonaws.com/global/global-bundle.pem
   ```
2. Define the host endpoint variable:
   ```bash
   export RDSHOST="aws-c8n.czsenqug2xnh.us-west-2.rds.amazonaws.com"
   ```
3. Generate the temporary IAM database authentication token and connect using `psql`:
   ```bash
   psql "host=$RDSHOST port=5432 dbname=postgres user=postgres sslmode=verify-full sslrootcert=./global-bundle.pem password=$(aws rds generate-db-auth-token --hostname $RDSHOST --port 5432 --username postgres --region us-west-2)"
   ```
