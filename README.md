# Peer-to-Peer File Sharing System

A Java implementation of a BitTorrent-like peer-to-peer file sharing system with sophisticated choking/unchoking mechanisms and distributed file distribution capabilities.

## Overview

This project implements a P2P file sharing software similar to BitTorrent. The system features the choking-unchoking mechanism, which is one of the most important features of BitTorrent protocol. The implementation follows a modified version of the original BitTorrent protocol optimized for educational purposes.

## Key Features

- **Choking/Unchoking Mechanism**: Dynamic peer selection based on download rates
- **Optimistic Unchoking**: Fair resource allocation among peers  
- **Piece-based File Sharing**: Efficient file distribution using BitSet tracking
- **Multi-machine Deployment**: SSH wrapper for distributed peer startup
- **Extensive Logging**: Comprehensive monitoring of peer interactions

## Program Setup

### Prerequisites
- Java Development Kit (JDK)
- SSH access to remote machines (if using distributed deployment)
- Network connectivity between peer machines

### Running the Project

1. **Compile the code**:
   ```bash
   javac PeerProcess.java
   ```

2. **For distributed deployment**:
   - Generate SSH keys: `ssh-keygen` (no passphrase)
   - Copy keys to remote machines: `ssh-copy-id -i <your_key> <username>@<remote_machine>`
   - Test SSH connections to avoid fingerprint prompts
   - Update `Ssh.java` with your username, project path, and SSH key path
   - Compile and run the SSH wrapper: `javac Ssh.java && java Ssh`

3. **Configuration Files**:
   - Copy `Common.cfg` and `PeerInfo.cfg` to peer directories
   - Place the file to be shared in the appropriate peer directory

## Architecture

- **PeerProcess.java**: Main entry point
- **PeerAdmin.java**: Central coordinator for peer operations
- **PeerHandler.java**: Individual peer connection management
- **ChokeHandler.java**: Implements choking/unchoking algorithms
- **OptimisticUnchokeHandler.java**: Manages optimistic unchoking
