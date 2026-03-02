### 📄 Deployment Instructions: Apache NiFi 1.11.4 & n8n Stack

This guide explains how to deploy your **Apache NiFi 1.11.4** cluster and **n8n** automation tool using **Docker Compose** and **Traefik** as a reverse proxy.

---

### 🛠️ Step 1: Prepare Your Environment

1. **Create a Project Folder**: Create a new directory on your server for this project.
2. **Initialize Directories**: Run the following command to create the shared data folder used by both NiFi and n8n:
```bash
mkdir local-files

```


3. **Prepare Configuration Files**: Ensure you have the `compose.yaml`, `.env`, and `n8n-flow-nifi.json` files saved in this folder.

---

### 🚀 Step 2: Launch the Stack

Run the following command to start all services in detached mode:

```bash
docker compose up -d

```

**What happens next?**

* **Traefik** will start and begin requesting SSL certificates for n8n.
* **ZooKeeper** will initialize to coordinate the NiFi nodes.
* **NiFi 1.11.4** will start on port **9091**.
* *Note: NiFi can take 2-5 minutes to fully boot. You can track progress with:* `docker compose logs -f nifi`



---

### 🌐 Step 3: Accessing the Applications

| Application | Protocol | URL | Internal Port |
| --- | --- | --- | --- |
| **Apache NiFi** | **HTTP** | `http://nifi.yourdomain.com:9091` | `9091` |
| **n8n** | **HTTPS** | `https://n8n.yourdomain.com` | `5678` |

---

### 🤖 Step 4: Import the n8n Monitoring Flow

1. Log in to your **n8n** instance.
2. Go to **Workflows** > **Add Workflow**.
3. Select **Import from File** from the top-right menu and upload `n8n-flow-nifi.json`.
4. **Important Update**: Open the "Check NiFi Health" node and ensure the URL is set to `http://nifi:9091/nifi-api/system-diagnostics`.
5. Click **Execute Workflow** to test the connection.
6. Toggle the **Active** switch to enable the 5-minute status checks.

---

### 📋 Step 5: Maintenance Commands

* **View All Logs**: `docker compose logs -f`
* **Check Service Status**: `docker compose ps`
* **Restart a Specific Service**: `docker compose restart nifi`
* **Stop the Stack**: `docker compose down`

---