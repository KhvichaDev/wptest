# WordPress Development Environment on GitHub Codespaces

This repository provides a ready-to-use development environment for WordPress on GitHub Codespaces. It uses Docker to create a consistent and isolated environment, allowing you to develop and test WordPress plugins and themes efficiently.

Features

WordPress with the latest version

MySQL database

PhpMyAdmin for database management

WP-CLI pre-installed for command-line WordPress management

Useful development tools (curl, less, vim, git)

Automatic WordPress installation after starting the environment

Setup

Prerequisites

To use this setup, you need access to GitHub Codespaces. Ensure you have the appropriate permissions and a valid GitHub account.

Step-by-Step Installation

Clone the repository to your GitHub account.

Open the project in GitHub Codespaces.

Codespaces will automatically build the environment based on the provided configuration files.

Configuration Files

.devcontainer/Dockerfile

The Dockerfile is based on the latest WordPress image and includes the following tools:

curl

less

mariadb-client

unzip

vim

git

Additionally, it installs WP-CLI for easy WordPress management.

.devcontainer/devcontainer.json

This configuration file sets up the Codespace environment with the following settings:

Forwarded Ports: 80 (WordPress) and 8080 (PhpMyAdmin)

Post-create command to set directory permissions

Post-start command to check the database and install WordPress if not already installed

Visual Studio Code extensions for PHP and formatting

.devcontainer/docker-compose.yml

The Docker Compose file defines three services:

wordpress: Uses the custom Dockerfile, binds to port 80, and connects to the database.

db: MySQL 5.7 with database credentials.

phpmyadmin: Runs on port 8080, linked to the MySQL database.

Known Issue

Currently, the WordPress site running on port 80 gets redirected to port 443, while the admin pages remain unaffected. Despite extensive troubleshooting and trying various methods for two days, the issue persists. If anyone has a solution, please share it in the comments, and I will update the configuration accordingly. This notice is to avoid confusion and potential negative feedback related to this redirection issue.

Usage

After the Codespace is built, visit the WordPress site at: https://<CODESPACE_NAME>-80.app.github.dev

Access PhpMyAdmin at: https://<CODESPACE_NAME>-8080.app.github.dev

Credentials

WordPress Admin Username: admin

WordPress Admin Password: admin123

PhpMyAdmin Root Password: rootpass

Troubleshooting

If you encounter issues during setup, check the Codespaces logs or restart the environment.

Contributing

Feel free to fork this repository and submit pull requests for improvements or bug fixes.
