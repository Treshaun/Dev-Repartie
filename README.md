# 🏛️ La Salle des Ventes Distribuée (e-Auction)

> **Module :** Développement d'Applications Réparties / Systèmes Distribués  
> [![Java](https://img.shields.io/badge/Java-11%2B-orange)](https://www.oracle.com/java/)
> [![Maven](https://img.shields.io/badge/Maven-3.6%2B-blue)](https://maven.apache.org/)
> [![JavaFX](https://img.shields.io/badge/JavaFX-17.0.2-green)](https://openjfx.io/)

---

## 1. Description du Projet

Système d'enchères électroniques en temps réel. Acheteurs multiples via TCP + Multicast, administration distante via RMI, clients JavaFX et console (legacy).

## 2. Architecture Technique

```
┌─────────────────────────────────────────────────────────────────┐
│                    ARCHITECTURE e-AUCTION                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   ┌──────────────┐         TCP/Socket          ┌─────────────┐  │
│   │   Client     │◄───────────────────────────►│             │  │
│   │  Acheteur A  │                             │             │  │
│   └──────────────┘                             │             │  │
│          ▲                                     │   SERVEUR   │  │
│          │ Multicast (225.1.1.1:6000)          │  D'ENCHÈRES │  │
│          ▼                                     │  (Port 5000)│  │
│   ┌──────────────┐         TCP/Socket          │             │  │
│   │   Client     │◄───────────────────────────►│             │  │
│   │  Acheteur B  │                             │             │  │
│   └──────────────┘                             └──────┬──────┘  │
│                                                       │         │
│                                                       │ RMI     │
│                                                       │ (1099)  │
│                                                       ▼         │
│                                                ┌─────────────┐  │
│                                                │    Admin    │  │
│                                                │   (RMI)     │  │
│                                                └─────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

### Composants

| Composant        | Description                                 | Technologies             |
| ---------------- | ------------------------------------------- | ------------------------ |
| **Server**       | Serveur central d'enchères                  | TCP + Multicast + RMI    |
| **Client Buyer** | Clients acheteurs (JavaFX UI et console)    | TCP + Multicast Listener |
| **Client Admin** | Clients admin (JavaFX UI et console legacy) | RMI                      |
| **Common**       | Classes partagées (DTOs, interfaces)        | Java Serialization       |

---

## 3. Instructions d'Installation

### Prérequis

- Java JDK 11+
- Maven 3.6+
- JavaFX 17.0.2 (via dépendances Maven)

### Compilation

```powershell
# Cloner le dépôt
git clone https://github.com/ranicharradi/Dev-Repartie.git
Set-Location Dev-Repartie

# Compiler tous les modules
mvn -DskipTests clean install
```

## 4. Exécution

### Version console (legacy)

```powershell
# Serveur
Start-Process powershell -ArgumentList '-NoExit','-Command','cd .\server; mvn exec:java ''-Dexec.mainClass=com.auction.server.AuctionServer'''

# Admin (console)
Start-Process powershell -ArgumentList '-NoExit','-Command','cd .\client-admin; mvn exec:java ''-Dexec.mainClass=com.auction.admin.AdminClient'''

# Acheteur (console)
Start-Process powershell -ArgumentList '-NoExit','-Command','cd .\client-buyer; mvn exec:java ''-Dexec.mainClass=com.auction.client.BuyerClient'''
```

### Version JavaFX (UI)

```powershell
Start-Process powershell -ArgumentList '-NoExit','-Command','cd .\server; mvn exec:java ''-Dexec.mainClass=com.auction.server.AuctionServer'''
Start-Process powershell -ArgumentList '-NoExit','-Command','cd .\client-admin; mvn exec:java ''-Dexec.mainClass=com.auction.admin.AdminClientApp'''
Start-Process powershell -ArgumentList '-NoExit','-Command','cd .\client-buyer; mvn exec:java ''-Dexec.mainClass=com.auction.client.BuyerClientApp'''
```

---

## 5. Configuration Réseau

| Paramètre       | Valeur         | Description                |
| --------------- | -------------- | -------------------------- |
| TCP Port        | `5000`         | Port du serveur d'enchères |
| Multicast Group | `225.1.1.1`    | Adresse de diffusion       |
| Multicast Port  | `6000`         | Port multicast             |
| RMI Port        | `1099`         | Port du registre RMI       |
| RMI Service     | `AuctionAdmin` | Nom du service RMI         |

