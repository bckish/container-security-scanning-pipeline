# Container Security Scanning Pipeline

An original reference pipeline that builds a small container image, scans it with Trivy, produces an SBOM, and blocks releases with critical vulnerabilities.

## Local checks

```bash
docker build -t portfolio/secure-api:local .
docker run --rm -p 8080:8080 portfolio/secure-api:local
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy:latest image --severity HIGH,CRITICAL --exit-code 1 portfolio/secure-api:local
```

## CI behavior

The GitHub Actions workflow runs on pull requests and pushes. It builds the image, scans it, and uploads a CycloneDX SBOM artifact.

## Design choices

- Non-root image user
- Pinned base image tag
- Health endpoint for runtime checks
- Vulnerability gate for high and critical findings

Security results should be triaged, not blindly ignored. False positives must be documented with an expiry date.
