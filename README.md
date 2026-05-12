# NFH Fabric Devkit

This repository is Network for Humanity (NFH)'s reference devkit for working with the NFH fabric test environment.

It helps you get started in stages:

1. Direct experience: use an existing sample kit and test immediately.
2. Guided setup: copy sample payloads and config from a sample kit, then test locally.
3. Create your own kit: bring your own payloads and Postman collection, and update config only when needed.

If you do not yet have your own payloads, start with one of the sample kits already included here.

## What Is Included

- Docker Compose setup for local BAP, BPP, Redis, and sandbox services
- Base adapter and routing config in `config/`
- Sample kits in `sampleKits/`
- Runtime payload directory in `payloads/`
- Base Postman collections in `postman/`

## Repository Structure

```text
fabric-devkit/
├── config/                  # Base adapter and routing configuration
├── install/                 # Docker Compose setup
├── payloads/                # Runtime payloads loaded by sandbox services
├── postman/                 # Base Postman collections
└── sampleKits/              # Ready-to-use sample kits
    ├── 01-DDM/
    │   └── postman/
    └── 02-local-retail/
        ├── config/
        ├── postman/
        └── responses/
```

## Before You Start

Clone the repository:

```bash
git clone https://github.com/Networks-for-Humanity/fabric-devkit
cd fabric-devkit
```

Start the stack:

```bash
cd install
docker compose -f docker-compose-adapter.yml up --build
```

This starts:

- `onix-bap` on `http://localhost:8081`
- `onix-bpp` on `http://localhost:8082`
- `sandbox-bap` on `http://localhost:3001`
- `sandbox-bpp` on `http://localhost:3002`
- `redis` on `localhost:6379`

For first-time testing, start your journey from `select` onwards.

Use this basic path first:

- `select`
- `init`
- `confirm`

If you are creating your own payloads and want to test `discover`, first publish your catalog using the `catalog/publish` call to the Catalog service.

## Option 1 - Direct Experience With DDM

Use this when you want to experience the devkit immediately without preparing your own payloads.

What is already available:

- Sample payloads for `nfh.global/testnet-ddm` are already present under `payloads/`
- Matching Postman collections are available in `sampleKits/01-DDM/postman/`

How to use it:

1. Start the Docker stack.
2. Import these Postman collections:
   - `sampleKits/01-DDM/postman/BAP-DDM-rain-probability.postman_collection.json`
   - `sampleKits/01-DDM/postman/BPP-DDM-rain-probability.postman_collection.json`
3. Start testing from `select`, then continue to `init` and `confirm`.

This is the fastest way for a beginner to feel the devkit before preparing anything custom.

## Option 2 - Guided Setup With Local Retail

Use this when you want to take an existing sample kit, copy its payloads and config, and run it locally.

### Step 1 - Create the runtime payload folder

```bash
mkdir -p payloads/nfh.global/testnet/response
```

### Step 2 - Copy the sample response payloads

Copy the files from:

```text
sampleKits/02-local-retail/responses/
```

into:

```text
payloads/nfh.global/testnet/response/
```

### Step 3 - Copy the sample config

Copy these files:

- `sampleKits/02-local-retail/config/local-simple-bap.yaml`
- `sampleKits/02-local-retail/config/local-simple-bpp.yaml`

into:

- `config/local-simple-bap.yaml`
- `config/local-simple-bpp.yaml`

### Step 4 - Import the Postman collection

Import:

- `sampleKits/02-local-retail/postman/local-retail.postman_collection.json`

### Step 5 - Start testing

Start from:

- `select`
- `init`
- `confirm`

You can extend later to other actions such as `status`, `track`, `support`, `update`, `cancel`, and `rate`.

## Option 3 - Create Your Own Kit

Use this when you are ready to use your own payloads and your own Postman collection.

### Step 1 - Create the payload folder

For your own devkit setup, use the standard NFH path:

```bash
mkdir -p payloads/nfh.global/testnet/response
```

### Step 2 - Add your response payloads

Place your payload JSON files inside:

```text
payloads/nfh.global/testnet/response/
```

### Step 3 - Import or create your Postman collection

Use your own Postman collection, or start from one of the existing collections and adapt it.

### Step 4 - Update config only if required

You typically need config changes only when:

- you are moving to a different network setup
- you have a new Dedi entry
- you need different participant or endpoint configuration

## Payload Naming Rules

These rules are important.

- The folder name must be exactly `response`
- Do not use `responses`
- Payload file names must exactly match the action name
- Use names like `on_select.json`, `on_init.json`, `on_confirm.json`
- Do not use names like `01-on_confirm.json`
- Do not use names like `on_confirm_use1.json`

The sandbox looks up payloads by exact action name, so wrong folder names or extra prefixes and suffixes can make responses fail to load.

## How To Observe Responses

When testing BAP-side action calls:

- check the responses through the BAP sandbox service

When testing BPP-side calls:

- check the acknowledgement coming back from the BAP side

Also keep the Docker logs open while testing:

```bash
cd install
docker compose -f docker-compose-adapter.yml logs -f
```

## Later: Replace Sandbox With Your Business App

The sandbox layers are only for early testing.

Once you are ready, you can replace the sandbox services with your own business-side application and consume the same responses there as part of your real integration flow.

## Base Collections In This Repo

The repo also includes base collections in `postman/`:

- `BAP - Fabric Devkit.postman_collection.json`
- `BPP - Fabric Devkit.postman_collection.json`

These are useful as starting points if you want to create or customize your own kit.

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
