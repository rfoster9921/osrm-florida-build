# osrm-florida-build

Standalone, generic build of an [OSRM](https://project-osrm.org/) (Open
Source Routing Machine) routing graph for the state of Florida, USA, from
the public [Geofabrik](https://download.geofabrik.de/) OpenStreetMap
extract.

**This repository contains no proprietary code.** It is a self-contained
build tool: download a public OSM extract, run the standard
`osrm-extract` → `osrm-partition` → `osrm-customize` pipeline via the
official `osrm/osrm-backend` Docker image, and publish the resulting
`.osrm` graph files as a GitHub Release asset.

## Why this exists

Building this graph requires several GB of RAM during preprocessing.
Running it on a small, resource-constrained host (e.g. a production server
that already runs other workloads) risks starving that host of memory.
GitHub-hosted Actions runners for **public repositories** are free and
provide more headroom (7GB RAM / 2 cores) than many small production
hosts, with zero contention against anything else. This repo exists so
the one-time graph build can happen there instead, with only the small
finished artifact (not the build process) ever reaching any other host.

## Usage

1. Go to the **Actions** tab → **Build OSRM Florida Graph** → **Run workflow**.
2. Wait for the run to complete (several minutes).
3. Download the `.osrm` graph files from the run's associated **Release**
   (tagged `osrm-florida-<run id>`) — `SHA256SUMS.txt` is included for
   integrity verification.
4. Run `osrm-routed` against the downloaded graph wherever you need real
   driving-distance routing. This repo does not deploy or run the routing
   server itself — build only.

## License / data attribution

Built from OpenStreetMap data, © OpenStreetMap contributors, available
under the [Open Database License (ODbL)](https://www.openstreetmap.org/copyright).
Any public-facing use of the resulting routing data must carry the same
attribution.
