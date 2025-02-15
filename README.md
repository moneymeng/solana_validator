# Solana Validator Setup Guide

This guide provides step-by-step instructions on how to set up and run a Solana validator.

## Prerequisites

Before setting up your validator, ensure you meet the following requirements:

- A dedicated server or VPS with the following minimum specifications:
  - CPU: 12 cores / 24 threads, 2.8GHz or faster
  - RAM: 128GB
  - Storage: 1TB NVMe SSD (Recommended: 2TB for future growth)
  - Network: 1 Gbps connection
  - Operating System: Ubuntu 22.04 LTS (or equivalent Linux distro)
- Solana CLI installed
- A Solana wallet with funds for staking and transaction fees
- Basic knowledge of Linux and command-line usage

## Installation Steps

### 1. Update and Install Dependencies

```sh
sudo apt update && sudo apt upgrade -y
sudo apt install -y git curl wget tmux
```

### 2. Install Solana CLI

```sh
sh -c "$(curl -sSfL https://release.solana.com/stable/install)"
export PATH="$HOME/.local/share/solana/install/active_release/bin:$PATH"
solana --version
```

### 3. Configure the Validator

```sh
solana config set --url https://api.mainnet-beta.solana.com
solana config set --keypair ~/.config/solana/validator-keypair.json
```

### 4. Generate Keypairs

```sh
solana-keygen new --outfile ~/.config/solana/validator-keypair.json
solana-keygen new --outfile ~/.config/solana/vote-account-keypair.json
```

### 5. Create a Vote Account

```sh
solana create-vote-account ~/.config/solana/vote-account-keypair.json ~/.config/solana/validator-keypair.json
```

### 6. Start the Validator

```sh
solana-validator \
  --identity ~/.config/solana/validator-keypair.json \
  --vote-account ~/.config/solana/vote-account-keypair.json \
  --rpc-port 8899 \
  --entrypoint entrypoint.mainnet-beta.solana.com:8001 \
  --entrypoint entrypoint2.mainnet-beta.solana.com:8001 \
  --entrypoint entrypoint3.mainnet-beta.solana.com:8001 \
  --limit-ledger-size \
  --no-untrusted-rpc \
  --log ~/solana-validator.log
```

## Monitoring and Maintenance

### Check Validator Status
```sh
solana catchup
```

### Monitor Logs
```sh
tail -f ~/solana-validator.log
```

### Restart Validator
```sh
pkill -9 solana-validator
solana-validator --restart
```

## Staking Your Validator
To earn rewards, you need to delegate SOL to your validator:

```sh
solana delegate-stake --stake-account ~/.config/solana/stake-account-keypair.json --vote-account ~/.config/solana/vote-account-keypair.json
```

## Additional Resources
- [Solana Validator Documentation](https://docs.solana.com/running-validator)
- [Solana Discord Community](https://discord.com/invite/solana)

## License
This project is open-source and available under the MIT License.

