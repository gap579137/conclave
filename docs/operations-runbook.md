# Conclave Operations Runbook

This runbook is for operators who maintain availability, security posture, and incident response.

Last verified: **February 18, 2026**.

## Scope

Use this document for day-2 operations:

- starting and supervising runtime
- health checks and diagnostics
- safe rollout and rollback
- incident triage and recovery

For first-time installation, start from [one-click-bootstrap.md](one-click-bootstrap.md).

## Runtime Modes

| Mode | Command | When to use |
|---|---|---|
| Foreground runtime | `conclave daemon` | local debugging, short-lived sessions |
| Foreground gateway only | `conclave gateway` | webhook endpoint testing |
| User service | `conclave service install && conclave service start` | persistent operator-managed runtime |

## Baseline Operator Checklist

1. Validate configuration:

```bash
conclave status
```

2. Verify diagnostics:

```bash
conclave doctor
conclave channel doctor
```

3. Start runtime:

```bash
conclave daemon
```

4. For persistent user session service:

```bash
conclave service install
conclave service start
conclave service status
```

## Health and State Signals

| Signal | Command / File | Expected |
|---|---|---|
| Config validity | `conclave doctor` | no critical errors |
| Channel connectivity | `conclave channel doctor` | configured channels healthy |
| Runtime summary | `conclave status` | expected provider/model/channels |
| Daemon heartbeat/state | `~/.conclave/daemon_state.json` | file updates periodically |

## Logs and Diagnostics

### macOS / Windows (service wrapper logs)

- `~/.conclave/logs/daemon.stdout.log`
- `~/.conclave/logs/daemon.stderr.log`

### Linux (systemd user service)

```bash
journalctl --user -u conclave.service -f
```

## Incident Triage Flow (Fast Path)

1. Snapshot system state:

```bash
conclave status
conclave doctor
conclave channel doctor
```

2. Check service state:

```bash
conclave service status
```

3. If service is unhealthy, restart cleanly:

```bash
conclave service stop
conclave service start
```

4. If channels still fail, verify allowlists and credentials in `~/.conclave/config.toml`.

5. If gateway is involved, verify bind/auth settings (`[gateway]`) and local reachability.

## Safe Change Procedure

Before applying config changes:

1. backup `~/.conclave/config.toml`
2. apply one logical change at a time
3. run `conclave doctor`
4. restart daemon/service
5. verify with `status` + `channel doctor`

## Rollback Procedure

If a rollout regresses behavior:

1. restore previous `config.toml`
2. restart runtime (`daemon` or `service`)
3. confirm recovery via `doctor` and channel health checks
4. document incident root cause and mitigation

## Related Docs

- [one-click-bootstrap.md](one-click-bootstrap.md)
- [troubleshooting.md](troubleshooting.md)
- [config-reference.md](config-reference.md)
- [commands-reference.md](commands-reference.md)
