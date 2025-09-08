# Copybara Starwars Sync

---

## Requirements

For the sync user perform the following steps:
1)  Create a single sync user with read access to the source repo (https://github.com/airbnb/viaduct) and write access to the target repo (https://github.com/viaduct-graphql/starwars).
2)  Generate an SSH key pair for the sync user on their machine, then add the public key in GitHub: GitHub → Profile → Settings → SSH and GPG keys → New SSH key.
3)  In the source repo settings (https://github.com/airbnb/viaduct/settings), go to Secrets and variables → Actions and create:
3.1) Secret DEPLOY_KEY_SYNC_STARWARS → the SSH private key of the sync user.
3.2) Variable COPYBARA_VERSION → e.g., v20250818.

## Triggers

On push to main

Only when files under demoapps/starwars/** change

## Copybara file

.github/copybara/copy.bara.sky defining workflow sync_to_dest, e.g. moving demoapps/starwars to the destination root.

## What the workflow does

- Checkout.

- Install Java 11 and Bazelisk. Enable Bazel cache.

- Clone and build Copybara at COPYBARA_VERSION.

- Set Git identity and SSH (uses DEPLOY_KEY_SYNC_STARWARS).

- copybara validate the .sky file.

- Run Copybara:

    copybara  .github/copybara/copy.bara.sky sync_to_dest

## Troubleshooting

Ensure branch sync exists or Copybara is allowed to create it.

Confirm core.move("demoapps/starwars", "") in copy.bara.sky.

Check that COPYBARA_VERSION is a valid tag in google/copybara.
