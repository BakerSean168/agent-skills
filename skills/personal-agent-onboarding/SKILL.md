---
name: personal-agent-onboarding
description: Onboard or migrate a user-approved, high-trust Personal Agent such as Muse or Sesame into the owner's Personal Digital System (PDS) with the shortest verified path. Use for 接入个人 Agent、Personal Agent 初始化、Muse 式接入、Sesame 迁移、给新 Agent 连接 Tailscale/Oracle2/SSH、恢复 Agent 访问、或验证 PDS/Mnemosyne/Thought Forest/GitHub 能力。Reuse existing Tailscale identity, SSH, repository and MCP integrations instead of creating dedicated proxy services, duplicate memory stores, or new developer environments.
---

# Trusted Personal Agent onboarding

**Goal:** Get an *owner-approved, high-trust* Personal Agent from a new VPS/terminal to a working PDS entry with the fewest moving parts. Follow the existing **Muse pattern**; never rebuild the infrastructure as part of onboarding.

## Boundaries first

- Ask for the owner's explicit approval **before** authorizing a new machine or public key for an existing privileged SSH account. A shared development account may have `sudo`, Docker and access to host credentials; Tailscale GitHub login is not sufficient authorization for SSH or MCP.
- When trust is limited or the machine is third-party controlled without owner approval, **do not apply this high-trust path**. Retain existing limited access and report the trust decision needed; do not invent a per-Agent gateway as a workaround.
- Do not ask the Agent to reveal, copy, print, export or send tokens/SSH private keys. The owner completes browser OAuth; the Agent supplies its **public** SSH key. Existing host-side client credentials may be used only through their normal configured tools and with permission.
- Do not create separate Linux accounts, clones, MCP proxies, firewalls, persistent processes, containers, databases, memory copies or additional always-on services unless the owner separately requests them. No production service changes during onboarding.

## 1. Discover the existing authority, not a new architecture

1. Locate the latest `PERSONAL_AGENT.md` in the owner's [PDS repository](https://github.com/BakerSean168/personal-digital-system/blob/main/PERSONAL_AGENT.md). Treat its host/catalog details as runtime documentation, not as values to copy permanently into this Skill.
2. Get four inputs from the current request, previous context or a verified owner source: agent/device name, intended execution host, desired capabilities, and confirmation this is a *trusted* agent. Request only the one missing critical input, if any.
3. Check whether the device and its SSH public key have *already* been authorized; preserve working access and avoid replacing unrelated users' keys, ACLs, or Git worktrees.

## 2. Join the existing Tailscale network

1. On the Agent's own machine, inspect the current Tailscale identity/status and settings. Reuse an existing authorized user identity if healthy.
2. For a new or previously tagged device, have the **owner** complete the GitHub-backed Tailscale sign-in in the owner's browser. A tagged device cannot simultaneously act as a user-owned device. Preserve any explicitly configured DNS, routes, SSH and exit-node settings; avoid blind `tailscale up --reset` or `tailscale logout` while it is the only recovery path.
3. Verify `tailscale status`, the machine's current private address, the intended user identity and connectivity to the target host. Check Tailnet ACLs before changing them; don't overwrite the policy or broaden access to other nodes.
4. Device Key Expiry is an optional owner-controlled policy decision for a persistently online trusted device, not a setup prerequisite.

## 3. Reuse existing SSH, Git and project tooling

1. The owner verifies the target server's SSH host fingerprint through an independent known-good record. Keep `StrictHostKeyChecking=yes`; never accept a changed fingerprint automatically.
2. Register only the Agent's own ED25519 **public** key with the owner's explicitly approved, existing SSH identity. Reuse the existing high-trust host account if authorized; don't silently elevate a former read-only account. Restrict key origins using the existing Tailnet policy where supported, and verify changes do not break other agents.
3. Test a real SSH shell and a non-mutating command, for example `whoami`, `pwd`, `git --version`, then read the PDS `PERSONAL_AGENT.md`. Do not claim success based only on an SSH banner or TCP port reachability.
4. For GitHub, test the CLI **on the SSH host first** (`gh api user --jq .login`; read a repository). An already-authorized on-host `gh` can create Issues/PRs without a second OAuth on the Agent VPS. A working `git fetch` alone does **not** prove push/PR permissions.
5. For coding, inspect the repo's `AGENTS.md`, active branches/worktrees and `git status` before changes. Use the owner's existing worktree practice and create a task-scoped branch/worktree **only when an actual coding task is authorized**. Never modify production, deploy or overwrite shared `main` during onboarding.

## 4. Reuse Mnemosyne and Thought Forest where they already run

- Read PDS for the current provider/transport. Prefer the **existing on-host MCP client/configuration**; do not assume a remote/cloud MCP client can see a private Tailnet endpoint.
- On approved existing tools, verify `mnemosyne_recall_canonical` for one non-secret canonical slot, and `get_vault_overview`, `search_knowledge`, then `read_note` for one ordinary note. A tool must be **called successfully** before being reported working.
- A reachable provider port is not a token. If client authentication is missing, report the precise gate and request owner approval; do **not** search arbitrary credential files, read host-wide secrets or build another gateway to make it work.
- Do not connect privileged Agent Fabric Worker commands, memory write tools or note write tools simply because discovery succeeded. Grant and test only capabilities needed for a concrete user request.

## 5. Verify and finish; do not keep configuring

Use [references/verification.md](references/verification.md) for a small evidence-based checklist and known failure cases. Finish when **identity → SSH → PDS** is verified; list optional MCP/GitHub capabilities separately as *verified*, *pending authorization* or *not needed*. Do not block all onboarding on optional integrations.

Update only the Agent's **local lightweight entry note** (e.g., `personal-digital-system-entry.md`) with current URLs/host aliases, how to reconnect, the PDS source of truth, and verified capabilities. Do not persist an exported personal memory, token, copied vault or stale private IP as a universal truth.

Deliver a short report: identity and host fingerprint verified, SSH and PDS actual results, optional tool-call evidence, any unresolved owner action, and confirmation no production state was changed. Once complete, **stop**; future Coding Agents need only the project/task context, not this entire onboarding.
