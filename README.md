# Mumax+ Docker

Docker configuration for Mumax+, a GPU-accelerated micromagnetic simulator.

## Setup

1. Clone the repo with submodules:
   ```bash
   git clone --recursive <repo-url>
   cd <repo-name>
   ```

2. If you already cloned without --recursive:
   ```bash
   git submodule update --init --recursive
   ```

3. Copy `.env.example` to `.env` and adjust values:
   ```bash
   cp .env.example .env
   ```

## Building

Build the Docker image:
```bash
docker-compose build
```

## Running

Run an example:
```bash
docker-compose run --rm mumaxplus conda run -n mumaxplus python examples/standardproblem4.py
```

## Updating Mumax+

To update the mumax+ submodule to the latest version:
```bash
cd src/mumaxplus
git pull origin master
cd ../..
git add src/mumaxplus
git commit -m "Update mumax+ submodule"
```

Any issues, just [email me](mailto:ahern669@ucr.edu)