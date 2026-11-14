# Hermes Telegram agent setup

Rhys's setup for a Telegram assistant built with [NousResearch's Hermes Agent](https://github.com/NousResearch/hermes-agent). The messaging gateway runs as a Linux user service; terminal tools run inside persistent Docker sandboxes.

This public repository contains a source reference, configuration examples, an active planning skill, and a historical sandbox image recipe. Credentials, Telegram user/channel identifiers, server addresses, private account paths, conversation history, memories, databases, runtime metadata, and archived skills are excluded.

## Contents

- `hermes-agent/`: upstream Git submodule pinned to `4b60979dc188655eb4fb81abf292890147ec2d4c`.
- `patches/server-changes.patch`: changes to the dependency manifest and lockfile used with that source version.
- `profile/`: configuration examples, the default personality, and the active planning skill.
- `systemd/hermes-gateway.service`: a user service template using `%h` for the account's home directory.
- `docker/Dockerfile`: the sandbox image recipe captured from the October 2026 setup.
- `source.json`: source provenance and publication exclusions.

The framework and skill retain their upstream attribution; see `UPSTREAM-LICENSE` and the skill's license metadata. This repository records a particular setup and is not an upstream Hermes release.

## Get the source

```sh
git clone --recurse-submodules https://github.com/Rhys-Lindmark/hermes-ops.git
cd hermes-ops
git -C hermes-agent apply --check ../patches/server-changes.patch
git -C hermes-agent apply ../patches/server-changes.patch
```

Follow the pinned upstream `hermes-agent/README.md` installation instructions on a fresh Linux host with Python and Docker. The examples use OpenRouter and Telegram. Dependency installation and a full recovery run were not performed when preparing this repository.

## Configure the agent

After installing Hermes, copy `profile/config.yaml.example` to your Hermes home's `config.yaml` and `profile/.env.example` to `.env`. Restore `SOUL.md` and the planning skill if desired. Merge these into an existing installation rather than overwriting its profile.

Fill in the blank values locally:

- `OPENROUTER_API_KEY`
- `TELEGRAM_BOT_TOKEN`
- `TELEGRAM_ALLOWED_USERS`
- `TELEGRAM_HOME_CHANNEL`

Set `.env` permissions to `0600`, restrict Telegram access to the intended users, and keep populated credentials out of Git. Review environment overrides, provider/model availability, paths, and sandbox resource limits for your machine.

The user service template assumes Hermes is installed at `~/.hermes/hermes-agent` with a virtual environment named `venv`, and `HERMES_HOME` is `~/.hermes`. Adjust those paths for your installation before installing the service.

## Sandbox image

The profile references `hermes-security:20261021`. The Dockerfile is a historical recipe, not a claim about current security updates. Its build context requires these separately downloaded files:

- `node-v24.22.1-linux-x64.tar.xz`, verified against the official Node distribution's `SHASUMS256.txt`.
- `certifi-2026.10.19-py3-none-any.whl`.
- `pip-26.2.2-py3-none-any.whl`.

Verify publisher checksums before building. Place the inputs next to the Dockerfile, then run:

```sh
docker build -t hermes-security:20261021 .
```

Downloaded dependencies and Docker image layers are not stored in this repository.
