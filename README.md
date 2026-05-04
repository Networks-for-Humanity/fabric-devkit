# NFH Fabric Devkit

This repository is Network for Humanity (NFH)'s reference devkit for working with the NFH fabric test environment. It includes:

- Docker Compose setup for local BAP, BPP, and sandbox services
- Config files for the adapters
- Postman collections for BAP and BPP flows
- A `payloads/` directory for custom JSON payloads

## Clone The Repository

```bash
git clone https://github.com/Networks-for-Humanity/fabric-devkit
cd fabric-devkit
```

## Repository Structure

```text
fabric-devkit/
├── config/      # Adapter and routing configuration
├── install/     # Docker Compose setup
├── payloads/    # Custom payloads loaded into the sandbox services
└── postman/     # Postman collections for BAP and BPP testing
```

## Quick Start

### Step 1 - Clone the devkit

Clone the repository using the NFH GitHub URL shown above.

### Step 2 - Add your payloads

Inside `payloads/`, create the NFH testnet folder structure and add your payload JSON files there.

```bash
mkdir -p payloads/nfh.global/testnet/response
```

Place your payload JSON files inside:

```text
payloads/nfh.global/testnet/response/
```

The sandbox services copy everything from `payloads/` into their runtime payload directory when the containers start, so if you add or change payloads later, restart the stack to reload them.

### Step 3 - Update the Postman collections

The repo ships with two collections in `postman/`:

- `BAP - Fabric Devkit.postman_collection.json`
- `BPP - Fabric Devkit.postman_collection.json`

Import these collections into Postman, then replace the placeholder request bodies with your NFH payload JSON files.

### Step 4 - Start the stack

```bash
cd install
docker compose -f docker-compose-adapter.yml up --build
```

This starts the local adapter and sandbox services used by the devkit.

### Step 5 - Start testing

Once the stack is running:

1. Open the imported Postman collections.
2. Use your updated request payloads in the relevant BAP and BPP APIs.
3. Run the requests in sequence based on your flow.

Typical flow examples:

- BAP: `discover` -> `select` -> `init` -> `confirm`
- BPP: `publish` -> `on_select` -> `on_init` -> `on_confirm`

## Postman Notes

- `BAP - Fabric Devkit.postman_collection.json` is for BAP caller flows.
- `BPP - Fabric Devkit.postman_collection.json` is for BPP caller flows.
- Keep your payload files aligned with the requests you plan to trigger from Postman.

## Useful Commands

Check running services:

```bash
cd install
docker compose -f docker-compose-adapter.yml ps
```

View logs:

```bash
cd install
docker compose -f docker-compose-adapter.yml logs
```

Stop the stack:

```bash
cd install
docker compose -f docker-compose-adapter.yml down
```
