# F5 BIG-IP Health Monitor Troubleshooting

## Overview

F5 BIG-IP health monitors continuously verify the availability of application servers and pool members. When a health monitor fails, BIG-IP can mark a pool member unavailable and stop sending production traffic to it.

## Health Monitor Types

- ICMP
- TCP
- HTTP
- HTTPS
- DNS
- Gateway ICMP

## Troubleshooting Workflow

1. Verify the virtual server status.
2. Check the associated pool.
3. Identify unavailable pool members.
4. Review the configured health monitor.
5. Validate the destination IP address and port.
6. Test connectivity from BIG-IP to the backend server.
7. Verify the monitor send and receive strings.
8. Review BIG-IP logs for failures.

## Useful Commands

```bash
tmsh show ltm virtual
tmsh show ltm pool
tmsh show ltm pool members
tmsh list ltm monitor
```

## HTTP/HTTPS Monitor Checks

Verify:

- Correct application URI
- Correct HTTP method
- Expected response string
- Backend server port
- SSL requirements
- Application response time

Example HTTP monitor request:

```text
GET /health HTTP/1.1
Host: application.example.com
Connection: close
```

## Common Failure Scenarios

### Backend Server Unreachable

Check routing, VLAN configuration, firewall policies, and server availability.

### Incorrect Monitor Port

Confirm that the monitor is testing the same service port used by the application.

### Receive String Mismatch

The application may be responding successfully while the configured receive string does not match the actual response.

### SSL/TLS Failure

For HTTPS applications, verify certificate compatibility and TLS settings.

### Application Response Delay

A slow backend application may exceed the configured monitor timeout.

## Validation Checklist

- Virtual server is available.
- Pool status is healthy.
- Required pool members are available.
- Health monitor is correctly associated.
- BIG-IP can reach backend servers.
- Application ports are listening.
- Send and receive strings are correct.
- Logs show no recurring monitor failures.

## Best Practices

- Use application-aware monitors instead of relying only on ICMP.
- Avoid excessively aggressive monitoring intervals.
- Use dedicated health-check endpoints where possible.
- Document expected application responses.
- Monitor pool-member state changes.
- Test monitor changes before production deployment.
