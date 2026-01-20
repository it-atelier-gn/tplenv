# 🔐 DesktopSecrets

DesktopSecrets is a lightweight, secure utility that centralizes secret management for developers and power users on the desktop. It integrates with KeePass and local user-provided secrets to make retrieving credentials simple, scriptable, and safe, while minimizing repeated password prompts through configurable caching. Designed for workflows that require environment templating and command-line automation, DesktopSecrets helps you keep sensitive data out of source files and streamline local development and deployment tasks.

## ✨ Features

- 🔑 **KeePass Integration** – Seamlessly fetch secrets from KeePass vaults.
- 💾 **Smart Caching** – Unlocked vaults stay accessible for a configurable duration.
- ⚙️ **Easy Configuration** – GUI settings menu in the taskbar icon.

---

## 🔌 Secret Providers

Secret providers are components that allow DesktopSecrets to retrieve secrets from various sources.

### 🔑 KeePass Provider

Ask the user to provide the master password of the given vault and retrieve the given secret.

#### Link Format

```properties
SECRET_NAME=keepass(VAULT|SECRET_TITLE)
```

* VAULT = A keepass database file. 
* SECRET_TITLE = Title of the secret inside the keepass database. 

### 👤 User Provider

Ask the user to provide a password with the given title.

#### Link Format

```properties
SECRET_NAME=user(TITLE)
```
## 🚀 Commands

### 📄*tplenv*

A command-line client for DesktopSecrets that allows to retrieve secrets defined in one or more `.env` template files.

####  Usage Example 

Create a `.env.tpl` file that contains references to secrets:

```properties
DATABASE_URL=postgresql://localhost:5432/mydb
API_SECRET=keepass($USERPROFILE\Credentials.kdbx|dev-api-secret)
LOG_LEVEL=debug
```

Use `tplenv` to output the combined contents of all resolved `.env.tpl*` files in the current working directory, so that the output can be sourced into a shell. Learn how to source the environment with the command: `tplenv --apply-one-liner`. More options are available. See `tplenv --help` to get a list.

Use `tplenv run` to run a command with the content of all resolved `.env.tpl*` files as environment. 

### 🛠️ *getsec*

A command-line client for DesktopSecrets that allows to retrieve secrets defined as command-line arguments.

####  Usage Example

```shell
getsec "API_SECRET=keepass($USERPROFILE\Credentials.kdbx|dev-api-secret)"
```

## 🧑‍🎓 Advanced topics

### ⚙️ Configuration

Access settings via the **taskbar icon menu**.

**Configuration Location:**
- The `DESKTOP_SECRETS_CONFIG_FILE` environment variable, or if not present,
- The executable directory file name `config.yaml`.

### Aliases

Define system-wide aliases for cleaner configuration in the file `aliases.yaml`:

Example:

```yaml
cloud: C:\Project\ABC\Vaults\cloud-secrets.kdbx
local: C:\Users\User\Vaults\local-secrets.kdbx
```

**Alias Configuration Locations:**
- The `DESKTOP_SECRETS_ALIASES_FILE` environment variable, or if not present,
- The executable directory file name `aliases.yaml`.


### Chaining

The KeePass provider supports opening a KeePass database using a secret retrieved from another KeePass database.

**Example:**
```properties
SECRET_NAME=keepass(VAULT_A[keepass(VAULT_B|SECRET_TITLE_B)]|SECRET_TITLE_A)
```

This prompts the user for the master password of VAULT_B, uses the value of SECRET_TITLE_B from VAULT_B as the master password to unlock VAULT_A, and then retrieves SECRET_TITLE_A from VAULT_A.


## 🛠️ Build

### Prerequisites

- Ensure Go is installed and set up.

### Build from Source

**Standard build:**

Windows

```pwsh
go build -o tplenv.exe ./cmd/tplenv
go build -o getsec.exe ./cmd/getsec
```

Linux 

```shell
go build -o tplenv ./cmd/tplenv
go build -o getsec ./cmd/getsec
```
