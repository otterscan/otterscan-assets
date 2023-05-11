# Otterscan Assets

## Introduction

This repo builds a Docker image containing all static assets used by Otterscan.

The image purpose is:

- It is published on [Docker Hub](https://hub.docker.com/repository/docker/otterscan/otterscan-assets/general) in order to serve as basis for the official Otterscan production image.
- It can be run locally serving the assets when running development version of Otterscan from the sources.

## Usage

Everything is self contained in the `Dockerfile`. There is a github actions workflow that builds and publishes it on Docker Hub.

In case you want to build it locally, there is a `build.sh` exemplary script to build it.

The image itself is a nginx image serving the assets on internal port 80. There is an exemplary script at `run.sh`.

## Image structure

It extracts, processes and merges data from several external sources linked here as git submodules as follows:

- `4bytes`: serves the method signature for a given 4byte selector (without the `0x` prefix) at `/signatures/<selector>`. Ex: get the ERC20 transfer method with: `http localhost:5175/signatures/a9059cbb`
- `topic0`: serves the event signature for a given topic selector (without the `0x` prefix) at `/topic0/<selector>`. Ex: get the ERC20 Transfer event with: `http localhost:5175/topic0/ddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef`.
- `assets`: serves token icons (sourced from trustwallet opensource icons; resized to 32x32) at `/assets/<chain-id>/<token-address>/logo.png`. Includes mainnet/polygon/BNB. Ex: download DAI logo with: `wget localhost:5175/assets/1/0x6B175474E89094C44Da98b954EedeAC495271d0F/logo.png`.
- `chains`: serves metadata for a certain chain at `/chains/eip155-<chain-id>.json`. Ex: get mainnet chain info with: `http localhost:5175/chains/eip155-1.json`.
