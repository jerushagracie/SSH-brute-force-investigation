# SSH Brute Force Investigation

## Summary
Analysis of a Linux SSH authentication log (auth.log) showing a successful
brute-force attack. The attacker IP 203.0.113.50 made 6 failed logins in
8 seconds, then logged in successfully as root.

Note: the log file is a small simulated dataset created for practice.

## Commands Used
Count failed logins:
    grep -c "Failed password" auth.log

Count failed logins per IP, ranked:
    grep "Failed password" auth.log | awk '{print $11}' | sort | uniq -c | sort -rn

See everything one IP did:
    grep "203.0.113.50" auth.log

## Findings
| IP | Failed logins |
|---|---|
| 203.0.113.50 | 6 |
| 198.51.100.7 | 1 |

## Timeline
- 10:01:12 to 10:01:20: 6 failed SSH logins from 203.0.113.50 against the
  root and admin accounts, alternating between them. The speed suggests an
  automated brute-force tool.
- 10:07:02: successful SSH login as root from the same IP.
- The logs do not show how the attacker obtained the password.

## Impact
The attacker has root access. The server should be treated as fully
compromised.

## Actions
1. Escalate to the incident response team.
2. Contain: end the session, block the IP at the firewall, reset the password.
3. Investigate: commands run, new users, changed files, deleted logs.
4. Recover: key-based SSH login, fail2ban.
5. Document the incident.
