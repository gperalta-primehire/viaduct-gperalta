# Copybara Starwars Sync

---

Syncs demoapps/starwars from this repo to viaduct-graphql/starwars on branch sync using Copybara. The job validates the config, builds Copybara, runs the sync, then verifies source vs destination SHAs.

## Triggers

On push to main

Only when files under demoapps/starwars/** change

## Requirements

Secrets

- DEPLOY_KEY_SYNC_STARWARS: SSH private key with write access to the destination repo. Add the public key as a Deploy key in the destination and enable Allow write access.

Repository Variables (Settings → Actions → Variables)

Vars

- COPYBARA_VERSION (e.g., v20250818)

- COPYBARA_SUBCOMMAND (optional; e.g., version)

## Copybara file

.github/copybara/copy.bara.sky defining workflow sync_to_dest, e.g. moving demoapps/starwars to the destination root.

## What the workflow does

- Checkout.

- Install Java 11 and Bazelisk. Enable Bazel cache.

- Clone and build Copybara at COPYBARA_VERSION.

- Set Git identity and SSH (uses DEPLOY_KEY).

- copybara validate the .sky file.

- Run Copybara:

    copybara $COPYBARA_SUBCOMMAND .github/copybara/copy.bara.sky sync_to_dest $COPYBARA_FLAG_COMMAND

## Troubleshooting

Ensure branch sync exists or Copybara is allowed to create it.

Confirm core.move("demoapps/starwars", "") in copy.bara.sky.

Check that COPYBARA_VERSION is a valid tag in google/copybara.
