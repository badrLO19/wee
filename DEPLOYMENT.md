# Deployment troubleshooting guide

Use this guide when a deployment fails or a newly deployed release is unhealthy.
It provides a concise checklist you can run through quickly, plus detailed steps
for collecting the information you need to resolve the issue.

## Quick checklist
1. **Confirm the build succeeded.** Review CI/CD logs for build failures before
   investigating runtime issues.
2. **Verify configuration.** Ensure required environment variables are set and
   match the expected values for the target environment.
3. **Check dependency installation.** Validate package manager logs to confirm
   all dependencies installed successfully.
4. **Inspect runtime logs.** Look for startup errors, stack traces, and binding
   issues (e.g., the service failing to listen on the expected port).
5. **Validate health checks.** Hit the health or status endpoint directly to see
   whether the service is reporting healthy.
6. **Confirm database and external services.** Make sure network access and
   credentials are correct for databases, caches, and third‑party APIs.
7. **Clear caches and rebuild.** When in doubt, run a clean build to rule out
   stale artifacts.

## Collecting diagnostics
- **Build logs:** Save the full CI/CD build output. Note the commit SHA and
  whether the build used cached layers.
- **Runtime logs:** Capture the first 200 lines of logs after startup. Include
  timestamps and the instance/host identifier.
- **Configuration:** Record the values (or at least presence) of critical
  environment variables—redact secrets.
- **Version info:** Note language runtime versions, package manager versions,
  and OS image details.
- **Network checks:** Document results of connectivity tests to databases or
  external services (e.g., `ping`, `nc -z`, or `curl` to service endpoints).

## Common fixes
- **Port already in use:** Update the service to listen on the platform‑assigned
  port (often provided via an environment variable like `PORT`).
- **Missing environment variables:** Add required variables to your deployment
  config or secrets store; redeploy once populated.
- **Mismatched dependency versions:** Pin versions in your lockfile and rebuild
  to ensure consistent environments across deployments.
- **Insufficient resources:** Increase memory/CPU limits or reduce concurrency
  until the service stabilizes.
- **Database migrations not applied:** Run migrations as part of the release
  process before flipping traffic to the new version.

## Escalation checklist
1. Capture and share the diagnostics listed above.
2. Identify whether the failure occurs during build, deployment, or runtime.
3. Determine the first version where the deployment started failing.
4. Prepare a minimal reproduction if the issue is deterministic.
5. Share the error message, stack trace, and reproduction steps with the team or
   support channel.

Following this checklist should help you isolate the cause of most deployment
issues and provide enough context for a fast resolution.
