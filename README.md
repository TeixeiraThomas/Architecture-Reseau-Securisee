# Secure Multi-Site Network Architecture

Technical architecture document for a secure multi-site network designed for a mid-sized company with approximately 100 users. The project covers segmentation, secure remote access, site-to-site connectivity and controlled public services.

## Scope

- Two interconnected sites with distinct addressing plans
- VLAN segmentation for users, servers, management, guest Wi-Fi and DMZ services
- Site-to-site IPsec VPN and remote-access VPN with MFA
- DMZ exposure limited to HTTPS services
- Guest Wi-Fi isolated from internal networks
- Centralized firewalling, logging and infrastructure monitoring
- Risk analysis and security controls aligned with common security practices

## Network Design

The documentation describes logical zones, VLAN allocation, RFC1918 addressing, firewall boundaries, VPN pools and the expected traffic flows between sites. It is intentionally vendor-neutral so that the design can be implemented with different network platforms.

## Deliverable

The complete technical architecture is available in [DAT.pdf](DAT.pdf). It includes the assumptions, security objectives, segmentation model, addressing principles, VPN design and risk-reduction measures.

## References

The design takes into account GDPR principles, ISO/IEC 27001 and 27002, and ANSSI security guidance.

## Author

Thomas Teixeira

## Status

Academic architecture project completed as part of the ETNA curriculum.