# Kasplex Syncer Makefile
# Management tool for Kasplex synchronization service

# Docker Compose command
DOCKER_COMPOSE ?= docker compose

# Docker Compose configuration
COMPOSE_DIR = ../docker-compose
ENV_DIR = ${COMPOSE_DIR}/envs

# Default network type (mainnet or testnet)
NETWORK_TYPE ?= testnet

# Set env file based on network type
ifeq ($(NETWORK_TYPE),mainnet)
    ENV_FILE = ${ENV_DIR}/syncer.mainnet.env
else
    ENV_FILE = ${ENV_DIR}/syncer.testnet.env
endif

COMPOSE_FILE = ${COMPOSE_DIR}/syncer.yml
COMPOSE_CMD = ${DOCKER_COMPOSE} -f ${COMPOSE_FILE} --env-file ${ENV_FILE} --project-directory $(PWD)

.PHONY: help update-jwt start stop restart logs clean status

# Default target
help:
	@echo "Kasplex Syncer Management Tool"
	@echo ""
	@echo "Usage: make [NETWORK_TYPE=mainnet|testnet] <command>"
	@echo "Default NETWORK_TYPE is testnet"
	@echo ""
	@echo "Examples:"
	@echo "  make start                                    - Start syncer service (testnet)"
	@echo "  make NETWORK_TYPE=mainnet start               - Start syncer service (mainnet)"
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
	@echo ""
	@echo "Network Types:"
	@echo "  mainnet  - Production network"
	@echo "  testnet  - Test network (default)"

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
	@echo "🚀 Starting kasplex syncer service ($(NETWORK_TYPE))..."
	@if [ ! -f "./jwt.txt" ]; then \
		echo "⚠️  Warning: jwt.txt file does not exist, generating..."; \
		make update-jwt; \
	fi
	@${COMPOSE_CMD} up -d
	@echo "✅ kasplex syncer service started ($(NETWORK_TYPE))"
	@echo "📊 View logs: make logs"

# Stop syncer service
stop:
	@echo "🛑 Stopping kasplex syncer service ($(NETWORK_TYPE))..."
	@${COMPOSE_CMD} down
	@echo "✅ kasplex syncer service stopped ($(NETWORK_TYPE))"

# Restart syncer service
restart:
	@echo "🔄 Restarting kasplex syncer service ($(NETWORK_TYPE))..."
	@make stop
	@make start

# View service logs
logs:
	@echo "📋 Viewing kasplex syncer service logs ($(NETWORK_TYPE))..."
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
	@echo "📊 kasplex syncer service status ($(NETWORK_TYPE)):"
	@${COMPOSE_CMD} ps
