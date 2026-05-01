# BuildDreams Pages

Static landing pages for BuildDreams and its product portfolio.

## Main Site

- `index.html` is the main BuildDreams homepage.
- Product pages live in their own folders, for example `Sutra/`, `FlowGuard/`, `InfraTrack/`, `Nivaran/`, `UnitPay/`, `VocalNode/`, `PartSight AI/`, `aspira/`, `FlatWala/`, and `visionAi/`.
- Shared brand assets are stored in `assets/images/`.

## Current Product Pages

- FlowGuard: controlled workflow and desktop monitoring for branch operations.
- InfraTrack: asset, fleet, inventory, maintenance, and approvals platform.
- Nivaran: complaint, request, document, and SLA ticketing workflow.
- PartSight AI: visual part lookup and inventory matching.
- Sutra: personal AI organizer for busy professionals and founders.
- UnitPay: QR reward, cashback, and payout workflow.
- VocalNode: voice AI for call answering and booking workflows.
- Aspira: white-label career ecosystem for institutions.
- FlatWala: real estate platform with buyer, seller, and admin portals.
- Vision AI: camera/photo-based decision and detection platform.

## CRM Integration

Lead forms submit to the BuildDreams CRM endpoint:

```txt
https://linearis.vercel.app/api/enquiries
```

Each product form should include product/page metadata so requests route cleanly inside the CRM.

## Deployment

This repository is static HTML/CSS/JS and can be deployed on Vercel or any static host.

Typical flow:

```bash
git add .
git commit -m "Update landing pages"
git push origin main
```

If product pages are deployed individually on subdomains, keep the homepage cards and CTAs aligned with the deployed URLs.

## Notes

- Keep product pages lightweight and self-contained.
- Prefer real product mockups, screenshots, or diagrams over generic stock imagery.
- Keep WhatsApp CTAs and INR pricing anchors visible on Indian B2B pages.
