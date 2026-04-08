# Kasplex Blockchain Node Deployment Project

## Project Overview

This is a Docker-based Kasplex blockchain node deployment project, designed for deploying Syncer mode in both mainnet and testnet environments:
- **Operation Mode**: Syncer mode - Lightweight node for blockchain data synchronization
- **Network Types**: 
  - **Mainnet** - Production environment network
  - **Testnet** - Testing environment network (default)

## Project Architecture

### Directory Structure
```
.
├── docker-compose/          # Docker Compose configuration files
│   ├── envs/               # Environment variable files
│   │   ├── syncer.mainnet.env   # Mainnet syncer environment configuration
│   │   └── syncer.testnet.env   # Testnet syncer environment configuration
│   └── syncer.yml          # Syncer service orchestration
├── docker/                 # Docker management tools
│   └── Makefile           # Automated deployment scripts
├── .gitignore             # Git ignore file
└── README.md              # Project documentation
```

### Core Components

#### Syncer Mode
Contains the following services:
- **geth**: Lightweight blockchain node, running on ports 28545/28546/28551/30305
- **syncer**: Data synchronization service, running on port 6060

## Features

- 🐳 **Docker Containerized Deployment**: Using Docker Compose for service orchestration
- 🔧 **Automated Management**: Providing Makefile scripts to simplify deployment and management
- 🔐 **JWT Security Authentication**: Automatic generation and management of JWT keys
- 🔄 **Data Synchronization**: Support for checkpoint synchronization of blockchain data
- 🌐 **Network Configuration**: Support for P2P network connections and bootstrap node configuration

## Quick Start

### Requirements

- Docker and Docker Compose
- At least 4GB available memory
- At least 100GB available disk space

### Deployment Steps

1. **Clone the project**
```bash
git clone <repository-url>
cd deploy
```

2. **Enter Docker directory**
```bash
cd docker
```

3. **Start the service**

Start syncer mode (testnet, default):
```bash
make start
```

Start syncer mode (mainnet):
```bash
make NETWORK_TYPE=mainnet start
```

## Service Management

### Management Commands

```bash
# View help
make help

# Start service (testnet)
make start

# Start service (mainnet)
make NETWORK_TYPE=mainnet start

# Stop service
make stop

# Stop service (mainnet)
make NETWORK_TYPE=mainnet stop

# Restart service
make restart

# Restart service (mainnet)
make NETWORK_TYPE=mainnet restart

# View logs
make logs

# View logs (mainnet)
make NETWORK_TYPE=mainnet logs

# Check status
make status

# Check status (mainnet)
make NETWORK_TYPE=mainnet status

# Clean data
make clean

# Update JWT key
make update-jwt
```

## Configuration

### Testnet Configuration (Default)
- Bootstrap node: `enode://8e20c6dfcf1c356339311dfee6823cdfec613d92a62aa23657ad701c57e95b4fbe6ad08f2bd40175e90756c5b028e6f52e72f189230580cc8423ed24eef77405@54.154.73.46:30303`
- Checkpoint sync: `ws://54.154.73.46:38546`
- Environment file: `syncer.testnet.env`
- Chain ID: 167012

### Mainnet Configuration
- Bootstrap node: `enode://eff0708be09f41859dcbf59134fae73117dc98ab6e3af21d522e942d692857860ec89d7ae3f1c0783840bdc583426afaa5dbb1b6a36776cee3fa3d3a1ff98978@158.220.127.109:30305`
- Checkpoint sync: `ws://158.220.127.109:28545`
- Environment file: `syncer.mainnet.env`
- Chain ID: 202555

### Environment Variables

#### General Configuration
- `GETH_DATA_PATH`: Geth data storage path
- `JWT_FILE_PATH`: JWT key file path
- `SYNCMODE`: Sync mode (snap/full)

#### Syncer Configuration
- `BOOTNODES`: P2P bootstrap node list
- `P2P_CHECKPOINT_SYNC_URL`: Checkpoint sync URL
- `CHAINID`: Blockchain chain ID

### Port Mapping

| Service | Port | Description |
|---------|------|-------------|
| geth | 28545 | RPC interface |
| geth | 28546 | WebSocket interface |
| geth | 28551 | Engine API (requires JWT authentication) |
| geth | 30305 | P2P network |
| syncer | 6060 | Syncer API |

## Security Considerations

1. **JWT Key Security**: JWT file permissions set to 600, ensuring only owner can read/write
2. **Data Backup**: Regularly backup important data in `./data` directory
3. **Network Security**: Ensure firewall is properly configured, only open necessary ports
4. **Key Management**: Regularly update JWT keys to avoid key leakage
5. **Authenticated API Access**: Engine API (port 28551) requires JWT authentication, recommend limiting access scope

## Troubleshooting

### Common Issues

1. **Port Conflicts**
   - Check if ports are occupied by other services
   - Modify port mappings in docker-compose file

2. **Insufficient Disk Space**
   - Clean old data: `make clean`
   - Check disk space: `df -h`

3. **JWT Key Issues**
   - Regenerate JWT: `make update-jwt`
   - Check file permissions: `ls -la jwt.txt`

4. **Network Connection Issues**
   - Check network configuration
   - Verify bootstrap node connectivity

### Log Viewing

```bash
# View all service logs
make logs

# View specific service logs
docker compose -f ../docker-compose/syncer.yml logs geth
docker compose -f ../docker-compose/syncer.yml logs syncer
```

## Development

### Custom Configuration

1. Modify environment variables: Edit `docker-compose/envs/syncer.testnet.env` file
2. Update service orchestration: Modify `docker-compose/syncer.yml` file

### Extending Functionality

- Add new service components
- Configure load balancing
- Integrate monitoring systems
- Add backup strategies

## License

This project follows the corresponding open source license.

## Contributing

Welcome to submit Issues and Pull Requests to improve the project.

## Contact

For questions or suggestions, please contact through:
- Submit GitHub Issues
- Send emails to project maintainers

---

**Note**: This is a blockchain node deployment project. Please ensure you fully understand the relevant technical requirements and security considerations before deployment. Please test all configurations and functions before deployment.
