# Windows RDP through Tailscale

This repository runs a temporary Windows Server 2022 machine with GitHub Actions. Connect to its desktop using Tailscale and Windows App (Remote Desktop). Each run starts a fresh machine; its files disappear when the job ends.

## One-time setup

1. In Tailscale's admin console, generate a **reusable, ephemeral, tagged** auth key. Preapprove it if your tailnet requires device approval. Restrict access to that tag so only your own devices can reach TCP 3389.
2. In this repository, open **Settings → Secrets and variables → Actions → New repository secret**. Add:
   - `TS_AUTHKEY`: the auth key from Tailscale.
   - `RDP_PASSWORD`: a unique Windows password of at least 16 characters. Keep it somewhere you can retrieve it; GitHub does not show secret values again.
3. Install Tailscale on the device where you use Windows App, and sign into the same tailnet.

## Start and connect

1. Open **Actions → Temporary Windows RDP → Run workflow**.
2. Wait for **Configure private RDP** to complete. Open the run's **Summary** and copy **Tailscale IP**.
3. In Windows App, add a PC using that IP, username `.\rdpuser`, and the password stored as `RDP_PASSWORD`.
4. Cancel the workflow run when finished. It otherwise stops after about 5½ hours. GitHub-hosted jobs have a six-hour maximum.

The RDP account is not an administrator. The workflow enables Network Level Authentication and adds a firewall rule limited to the Tailscale IPv4 address and source range. It does not print the password. It does not provide persistent storage or an always-on Windows VM.
