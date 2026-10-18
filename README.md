==================================================
n8n Self-Hosted Local Stack (Ubuntu Server + Docker)
==================================================

OVERVIEW:
This stack runs n8n locally using Docker Compose with a dedicated 
PostgreSQL database, code execution sandboxes, task runners, and 
SearXNG for web-searching AI agents.

DIRECTORY STRUCTURE:
~/n8n/
├── .env                  # Environment variables & secrets
├── compose-n8n.yml       # Docker Compose service definitions
└── searxng-settings.yml  # Configuration for SearXNG web search

--------------------------------------------------
1. CONFIGURATION SUMMARY (.env)
--------------------------------------------------
The environment file contains version tags, runner authentication tokens, 
sandbox service keys, and PostgreSQL database connection variables:
  - Database User: sample
  - Database Password: sample
  - Database Name: sample
  - Connection Host: postgres (Docker internal network)

--------------------------------------------------
2. DOCKER COMPOSE ARCHITECTURE (compose-n8n.yml)
--------------------------------------------------
Services included in the stack:
  - postgres: PostgreSQL 18 container for robust workflow state storage
  - n8n: Core workflow automation engine (Port 5678)
  - sandbox-api & sandbox-runner-1: Secure code execution sandboxes
  - runners: Dedicated task runners for n8n Code nodes
  - searxng: Local web-search backend for AI workflows

--------------------------------------------------
3. DAILY OPERATIONS & MANAGEMENT
--------------------------------------------------

* Navigate to your project directory on the server:
  cd ~/n8n

* Start the stack in the background:
  docker compose -f compose-n8n.yml up -d

* View live container logs:
  docker compose -f compose-n8n.yml logs -f

* Stop the stack:
  docker compose -f compose-n8n.yml down

* Stop the stack AND wipe database/data volumes (WARNING: Deletes all data):
  docker compose -f compose-n8n.yml down -v

--------------------------------------------------
4. ACCESSING THE n8n WEB INTERFACE
--------------------------------------------------
Since n8n enforces strict secure cookie policies over local network IPs, 
use SSH Local Port Forwarding from your workstation to access it via localhost:

* Run this command on your workstation terminal:
  ssh -L 5678:localhost:5678 server@ip

* Open your workstation browser and navigate to:
  http://localhost:5678/setup
