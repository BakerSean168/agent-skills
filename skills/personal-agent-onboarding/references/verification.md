# Verification and recovery — trusted PDS onboarding

Read when verifying a new connection or diagnosing a failed onboarding stage. Use current PDS/host facts; commands are examples, not authorization to change identity, ACLs or secrets.

## Minimum non-mutating evidence

| Gate | Check | Success evidence | If it fails |
| --- | --- | --- | --- |
| Tailscale | `tailscale status` on Agent host; `tailscale whois <target-private-ip>` where supported | Online device, expected owner identity, host reachable | Verify owner OAuth, tags, ACL and preserved DNS/routes; do not disable security controls |
| SSH host identity | Compare fingerprint against owner-verified host record, not an unverified scan | Pinned fingerprint matches | Stop and ask owner to verify change |
| SSH shell | `ssh -o StrictHostKeyChecking=yes <approved-user>@<target-private-ip> 'whoami; pwd; git --version'` using Agent's own key | Expected account and working remote command | Check approved pubkey, account, Tailnet ACL and `ForceCommand`; do not create a service |
| PDS | `git -C <pds-path> status --short --branch` and read `PERSONAL_AGENT.md` / generated catalog | Source readable; catalog current | Refresh guide via owning repository, not memory dump |
| GitHub optional | On approved host: `gh api user --jq .login`; `gh repo view <owner>/<repo>` | Correct authorized identity and repo | Do not borrow/copy tokens; ask owner for proper GitHub authorization if needed |
| Mnemosyne optional | Call `mnemosyne_recall_canonical` with one known non-secret slot | Tool returns an actual matching record | Report missing client/credential; don't inspect token stores without approval |
| Thought Forest optional | Call `get_vault_overview`, `search_knowledge` and `read_note` for a harmless documentation path | Valid overview, search results and note body | Verify existing MCP connection/auth on host; don't create per-Agent proxy |

## Common distinctions

- **GitHub OAuth for Tailscale ≠ SSH authorization ≠ GitHub repository permissions ≠ MCP authentication.** Validate them separately, but do not require separate OAuth where the existing *on-host* CLI is already authorized.
- **MagicDNS disabled:** use the current Tailscale private IP from the owner/device status; do not substitute public IPv6 or disable SSH host-key checking.
- **Formerly tagged device:** arrange owner-assisted reauthentication to a personal Tailscale identity. Re-read device status and SSH source restriction after the change; IP and device name are not permanent identifiers.
- **Host has `ubuntu` or equivalent sudo/docker privileges:** explicitly note the trust impact. A prompt telling the Agent to be read-only cannot enforce read-only against such an account. Avoid sensitive operations without a task-specific instruction.
- **SSH works, hosted MCP client fails:** the client may be outside the Tailnet. Check owner-approved MCP integrations on the SSH host first.
- **PDS counts change:** use the catalog output at the time of the check; don't freeze past domain or repository counts in the Skill.
- **Existing Git worktrees are active:** don't reset/rebase/pull shared branches as part of onboarding. Inspect first; use an isolated worktree only for a later authorized coding task.

## Completion report template

```text
Personal Agent: <name>; trust: owner-approved high-trust
Tailscale: <identity> — verified / pending
SSH: <host alias> / <account> — verified fingerprint and command / pending
PDS: <guide revision and catalog status> — read successfully / pending
GitHub: <identity and repo read check> — verified / optional / pending
Mnemosyne: <actual tool call> — verified / optional / pending
Thought Forest: <actual tool calls> — verified / optional / pending
Unresolved owner action: <only if needed>
Changes: <explicit list, or none>
Next: stop onboarding; receive a real task
```
