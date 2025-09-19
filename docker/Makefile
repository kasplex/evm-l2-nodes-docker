# Kasplex Syncer Makefile
# Management tool for Kasplex synchronization service (testnet only)

# Docker Compose command
DOCKER_COMPOSE ?= docker compose

# Docker Compose configuration
COMPOSE_DIR = ../docker-compose
ENV_DIR = ${COMPOSE_DIR}/envs

# Fixed configuration for syncer on testnet
COMPOSE_FILE = ${COMPOSE_DIR}/syncer.yml
ENV_FILE = ${ENV_DIR}/syncer.testnet.env

COMPOSE_CMD = ${DOCKER_COMPOSE} -f ${COMPOSE_FILE} --env-file ${ENV_FILE} --project-directory $(PWD)

.PHONY: help update-jwt start stop restart logs clean status

# Default target
help:
	@echo "Kasplex Syncer Management Tool (Testnet Only)"
	@echo ""
	@echo "Usage: make <command>"
	@echo ""
	@echo "Available commands:"
	@echo "  make update-jwt    - Update JWT secret file"
	@echo "  make start         - Start syncer service"
	@echo "  make stop          - Stop syncer service"
	@echo "  make restart       - Restart syncer service"
	@echo "  make logs          - View service logs"
	@echo "  make status        - Check service status"
	@echo "  make clean         - Clean data directory and JWT file"
	@echo "  make help          - Show this help message"

# Update JWT secret file
update-jwt:
	@echo "🔄 Updating JWT secret file..."
	@if [ -f "./jwt.txt" ]; then \
		echo "⚠️  Warning: jwt.txt file already exists and will be overwritten"; \
		read -p "Continue? (y/N): " confirm; \
		if [ "$$confirm" != "y" ] && [ "$$confirm" != "Y" ]; then \
			echo "❌ Operation cancelled"; \
			exit 1; \
		fi; \
	fi
	@openssl rand -hex 32 > ./jwt.txt
	@chmod 600 ./jwt.txt
	@echo "✅ JWT secret updated: ./jwt.txt"
	@echo "🔒 File permissions set to 600"

# Start syncer service
start:
	@echo "🚀 Starting kasplex syncer service (testnet)..."
	@if [ ! -f "./jwt.txt" ]; then \
		echo "⚠️  Warning: jwt.txt file does not exist, generating..."; \
		make update-jwt; \
	fi
	@${COMPOSE_CMD} up -d
	@echo "✅ kasplex syncer service started (testnet)"
	@echo "📊 View logs: make logs"

# Stop syncer service
stop:
	@echo "🛑 Stopping kasplex syncer service (testnet)..."
	@${COMPOSE_CMD} down
	@echo "✅ kasplex syncer service stopped (testnet)"

# Restart syncer service
restart:
	@echo "🔄 Restarting kasplex syncer service (testnet)..."
	@make stop
	@make start

# View service logs
logs:
	@echo "📋 Viewing kasplex syncer service logs (testnet)..."
	@${COMPOSE_CMD} logs -f

# Clean data directory and JWT file
clean:
	@echo "🧹 Cleaning data directory and JWT file..."
	@if [ -d "./data" ]; then \
		echo "⚠️  Warning: About to delete data directory ./data"; \
		read -p "Continue? (y/N): " confirm; \
		if [ "$$confirm" = "y" ] || [ "$$confirm" = "Y" ]; then \
			rm -rf ./data; \
			echo "✅ Data directory deleted"; \
		else \
			echo "❌ Data directory deletion cancelled"; \
		fi; \
	fi
	@if [ -f "./jwt.txt" ]; then \
		echo "⚠️  Warning: About to delete JWT file ./jwt.txt"; \
		read -p "Continue? (y/N): " confirm; \
		if [ "$$confirm" = "y" ] || [ "$$confirm" = "Y" ]; then \
			rm -f ./jwt.txt; \
			echo "✅ JWT file deleted"; \
		else \
			echo "❌ JWT file deletion cancelled"; \
		fi; \
	fi
	@echo "✅ Cleanup completed"

# Check service status
status:
	@echo "📊 kasplex syncer service status (testnet):"
	@${COMPOSE_CMD} ps
