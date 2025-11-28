# Peer-to-Peer File Sharing System

A Java implementation of a BitTorrent-like peer-to-peer file sharing system with sophisticated choking/unchoking mechanisms and distributed file distribution capabilities.

## Overview

This project implements a P2P file sharing software similar to BitTorrent. The system features the choking–unchoking mechanism, piece-level distribution, full logging, and optional multi-machine deployment. It has been updated to work on **modern Java versions** and now includes **detailed debug logging of handshake / bitfield exchanges** for demonstration clarity.

## Key Features

- **Choking/Unchoking Mechanism**: Dynamic peer selection based on download rates  
- **Optimistic Unchoking**: Fair resource allocation among peers  
- **Piece-based File Sharing**: Efficient file distribution using BitSet tracking  
- **Multi-machine Deployment (optional)**: SSH wrapper for distributed peer startup  
- **Detailed Logging (NEW)**:  
  - Handshake send/receive logs  
  - Bitfield send/receive logs  
  - Piece request/piece transfer debug logs  
- **Modern Java Compatibility (NEW)**:  
  - Removed deprecated `Thread.stop()`  
  - Replaced with safe `interrupt()`–based shutdown  
  - Clean thread termination and file handle management

---

## Program Setup

### Prerequisites
- Java Development Kit (JDK 15+ recommended)
- (Optional) SSH access to remote machines for multi-node testing

---

## Running the Project Locally (Single Machine Demo)

### **1. Clean any previous build**
```bash
del /S *.class
```

### **2. Compile all Java source files**
```bash
javac src\*.java PeerProcess.java
```

### **3. Ensure `Common.cfg` and `PeerInfo.cfg` are in the root folder**
Example configuration (used in testing):

**Common.cfg**
```
NumberOfPreferredNeighbors 3
UnchokingInterval 5
OptimisticUnchokingInterval 10
FileName testfile.txt
FileSize 42
PieceSize 16384
```

**PeerInfo.cfg**
```
1001 localhost 7001 1
1002 localhost 7002 0
1003 localhost 7003 0
```

### **4. Start peers in separate terminals**
```bash
java PeerProcess 1001
java PeerProcess 1002
java PeerProcess 1003
```

---

## What to Verify (Demo Evidence)

✔ `testfile.txt` should appear in all peer folders  
✔ Peers should log handshake, bitfield, piece requests  
✔ No `UnsupportedOperationException` or thread errors  
✔ Logs should contain lines like:

```
[DEBUG] Peer [1002] successfully completed handshake with [1001].
[DEBUG] Peer [1003] received the 'bitfield' message from [1002].
```

---

## Optional — Multi-Machine Deployment

1. Generate SSH keys  
   `ssh-keygen`
2. Copy keys (no passphrase)  
   `ssh-copy-id -i <key> <user>@machine`
3. Edit `Ssh.java` with:
   - Username
   - Remote path
   - SSH key location
4. Run:
```bash
javac Ssh.java
java Ssh
```

---

## Architecture

| File | Purpose |
|------|--------|
| PeerProcess.java | Main entry point |
| PeerAdmin.java | Oversees peer state & shutdown |
| PeerHandler.java | Connection & message handler |
| ChokeHandler.java | Regular choking logic |
| OptimisticUnchokeHandler.java | Unchoke strategy |
| PeerLogger.java | Log writer |
| TerminateHandler.java | Final shutdown |

---

## Summary of Fixes / Improvements

| Problem | Fix |
|---------|-----|
| Deprecated `Thread.stop()` | Replaced with `interrupt()` |
| No handshake logging | Added debug log messages |
| Hard to demo | Logs now clearly show protocol messages |
| Shutdown issues | Clean thread termination logic |

---

## Final Status
This project **successfully runs end-to-end** on modern Java and demonstrates full P2P file distribution with correct piece transfer + comprehensive protocol logging.

Ready for submission / GitHub release   
