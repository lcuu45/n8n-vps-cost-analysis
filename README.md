# vps for n8n: Match the Plan to Your Workflow Load, Avoid the Webhook Trap, and Know What Renewal Actually Costs

Self-hosting n8n comes down to three decisions: how much RAM your workflows need, what the server costs in year two (not just year one), and whether setup is a weekend project or a five-minute click. Short version: a 2 GB RAM / 2 vCPU machine handles personal automation comfortably, and RackNerd's KVM VPS specials fit that at **$35.99/year** — with the renewal priced the same, because the company's own FAQ states these specials are "lifetime recurring." Heavier setups, queue mode, or local AI models push you to the 4 GB and 6 GB tiers, at $59.99 and $89.99 a year respectively.

> **Quick pick:** Most people should start with the **2 GB KVM VPS at $35.99/year** (2 vCPU, 35 GB SSD, 5 TB monthly traffic). It meets RackNerd's own stated requirement for its one-click n8n deployment, leaves headroom for Docker, and renews at the same rate. If you already know you'll run PostgreSQL alongside n8n or push past 50 workflows, jump to the 4 GB plan instead. 👉 [Check the current RackNerd VPS specials](https://bit.ly/RacKnerd)

## How much server does n8n actually need?

Less than hosting marketing implies, but more than the absolute minimum a VPS can boot with. Independent sizing guides and n8n's own community land on roughly the same tiers:

| Usage | Workflows | RAM | vCPU | Storage |
| --- | --- | --- | --- | --- |
| Hobby | under 20 | 2 GB | 1 | 20 GB |
| Standard | 20–50 | 4 GB | 2 | 40 GB |
| Pro / queue mode | 50+ | 8 GB | 4 | 80 GB |

Two things quietly push you up a tier. The first is PostgreSQL: running it on the same box is fine at the standard level, but the database eats RAM alongside n8n, so the "2 GB is plenty" advice really assumes light usage. The second is queue mode — n8n plus workers plus Redis plus PostgreSQL on one machine — which is where 8 GB starts to make sense.

Execution frequency matters as much as workflow count. A workflow that runs twice a day and a webhook endpoint getting hit every few seconds are different animals, even with identical workflow counts. Most automations spend their lives idle, though, which is why 2 GB feels comfortable for the majority of self-hosters.

One detail worth flagging before you buy the cheapest plan available: RackNerd's ctrl control panel lists n8n as a one-click app that's "comfortable from 2 GB." A 1 GB VPS can technically run n8n, but you'd be doing a manual Docker install with no headroom for the OS, and the panel won't deploy the app for you there. The gap between the 1 GB plan ($21.99/year) and the 2 GB plan ($35.99/year) is $14 a year. That's the cheapest insurance in this whole purchase. 👉 [Compare the 1 GB and 2 GB plans on the specials page](https://bit.ly/RacKnerd)

## RackNerd's full KVM VPS lineup

RackNerd's current specials page lists five KVM VPS plans, all built for self-hosting and AI workloads. Every plan includes full root access, one IPv4 address, a 1Gbps port, KVM virtualization, and the ctrl panel, and can be deployed across RackNerd's locations in North America and Europe (Los Angeles, Dallas, New York, Chicago, Seattle, San Jose, Atlanta, Ashburn, Toronto, Amsterdam, London, Dublin, Strasbourg, and others — the site claims 20 datacenter locations total).

| Plan | RAM / vCPU | SSD | Monthly traffic | Price (billed yearly) | Order |
| --- | --- | --- | --- | --- | --- |
| 1 GB KVM | 1 GB / 1 vCPU | 20 GB | 3 TB | $21.99/yr | [ Order the 1 GB plan](https://bit.ly/RacKnerd) |
| 2 GB KVM | 2 GB / 2 vCPU | 35 GB | 5 TB | $35.99/yr | [ Order the 2 GB plan](https://bit.ly/RacKnerd) |
| 4 GB KVM | 4 GB / 3 vCPU | 60 GB | 7 TB | $59.99/yr | [ Order the 4 GB plan](https://bit.ly/RacKnerd) |
| 6 GB KVM | 6 GB / 6 vCPU | 100 GB | 12 TB | $89.99/yr | [ Order the 6 GB plan](https://bit.ly/RacKnerd) |
| 8 GB KVM | 8 GB / 7 vCPU | 150 GB | 20 TB | $119.99/yr | [ Order the 8 GB plan](https://bit.ly/RacKnerd) |

Mapped to the sizing table above: hobby setups land on the 2 GB plan, standard deployments on the 4 GB, and queue-mode or AI-heavy builds on the 6 GB or 8 GB. Even the top tier works out to $10 a month, which is relevant context when you look at what managed n8n hosting charges.

## What it actually costs over three years

The number that decides this purchase is the renewal price, and this is where most hosting comparison articles go vague. RackNerd's specials FAQ is specific: asked whether a special can be renewed at the same rate, the answer is yes — the specials are "lifetime recurring," renewable "at the same rate for as long as you decide to keep the service."

So the math is simple. The 2 GB plan costs $35.99 in year one, year two, and year three: **$107.97 total, roughly $3/month**. Compare that to the common budget-host pattern of a steep first-year discount followed by a full-price renewal, where the same three years can run two to three times as much. Third-party long-term users report renewal invoices matching their original purchase price exactly, which matches the stated policy.

Three billing details to know before ordering:

- **No auto-charge by default.** Renewal invoices are generated about two weeks before expiry and paid manually; there's an optional Auto-Recharge feature you can enable in the client area if you'd rather not think about it.
- **Upgrading has a catch.** If you upgrade from a yearly special to a regular monthly plan, renewal pricing follows the regular plan — the locked special rate doesn't carry over. Pick the size you'll want for the year, not the one you'll "outgrow" in a month.
- **Refunds.** The specials page doesn't advertise a 30-day money-back guarantee, and third-party guides note there's no standard one. Treat the purchase as a year-long commitment. If you want the cheapest possible trial, the 1 GB plan at $21.99 limits the downside — just remember it's below the one-click n8n threshold. 👉 [Review the plan details and current specials](https://bit.ly/RacKnerd)

## Getting n8n running: one-click or Docker, and the webhook step people miss

Setup takes one of two paths, and both end at the same place.

The short path: after your VPS provisions (RackNerd states provisioning happens right after payment), open the ctrl panel, go to One-Click Apps, pick n8n, and credentials appear in-panel when it's ready. The same catalog includes Docker + Portainer, Coolify, Ollama, AnythingLLM, and LibreChat — worth knowing if your roadmap includes a local LLM next to your automations, since those AI apps want 4 GB minimum and OpenClaw wants 8 GB.

The manual path works on any plan and any OS choice (Ubuntu, Debian, AlmaLinux, Rocky, Fedora, or CentOS at order time): SSH in, install Docker, write a compose file for the `n8nio/n8n` image on port 5678 with a persistent volume at `/home/node/.n8n`, and run `docker compose up -d`.

Either way, there's one step that separates "n8n is running" from "n8n is usable," and it's the part most quick-start guides skip: **webhooks**. To receive callbacks from Slack, Stripe, GitHub, or anything else, n8n needs to be reachable over HTTPS with its environment variables pointing at your real domain — `N8N_HOST`, `N8N_PROTOCOL=https`, and `WEBHOOK_URL=https://n8n.yourdomain.com/`. Skip `WEBHOOK_URL` and n8n generates callback URLs pointing at `localhost:5678`, which no external service on earth can reach.

The fix is a reverse proxy, and Caddy is the lowest-effort option because it issues and renews SSL certificates automatically. The whole Caddyfile is two lines:


n8n.yourdomain.com {
    reverse_proxy localhost:5678
}


Point a DNS record at your VPS IP, reload Caddy, and you have automatic HTTPS, restart-proof startup, and persistent data. Budget an extra 20 minutes for this after the one-click install.

One expectation to set honestly: RackNerd VPS is **unmanaged**. You get a working server, a capable panel (reinstalls, HTML5 VNC console, firewall rules, root password resets), and 24/7 support for infrastructure problems — but nobody configures your n8n environment variables for you.

## How this compares to the rest of the n8n hosting market

A few alternatives, for context on where RackNerd fits:

- **Hostinger** markets n8n VPS plans with a preconfigured Docker template that deploys in minutes, plus weekly backups and a malware scanner. Its own guidance suggests 8+ vCPU cores and 16+ GB RAM for serving many clients — appropriate for heavy commercial loads, and priced well above RackNerd's specials.
- **Hetzner** offers EU-based KVM VPS instances starting around €4–5 per month in the 2 GB class. Good network quality and European locations, but a fully manual Docker setup.
- **DigitalOcean's** $6/month droplet (1 GB RAM) can run n8n at a testing tier, with the same 1 GB limitations described earlier.
- **n8n Cloud**, the official hosted option, starts around €20/month and removes all server work — but that's €240 a year, every year, and it never drops.

RackNerd's pitch is price with a locked renewal, and the one-click n8n deployment closes most of the convenience gap with template-based hosts. What none of the budget DIY options provide is an SLA or migration assistance; if your automations are business-critical to the minute, managed hosting is a different conversation.

## FAQ

**Will my renewal price go up?** No. RackNerd's specials FAQ states these prices are lifetime recurring — you renew at the purchase rate for as long as you keep the service.

**Which OS should I pick for n8n?** Ubuntu or Debian are the safe defaults; they're what most n8n Docker guides assume, and both are selectable at checkout. You can reinstall later from the ctrl panel if you change your mind.

**Where should my VPS be located?** Close to whatever your workflows talk to. RackNerd covers North America and Europe across roughly 20 locations; webhook latency adds up when every automation round-trips across an ocean.

**Is the VPS managed?** No. RackNerd VPS is unmanaged — full root access and 24/7 infrastructure support, but you own the software stack.

**Does the VPS come with an IPv4 address?** Yes, every special includes one IPv4 address and a 1Gbps port.

## The bottom line, by situation

Running personal automations under 20 workflows: the 2 GB plan at $35.99/year with one-click n8n deployment is the whole decision, and the locked renewal rate is why it stays the right call in year three. In the 20–50 workflow range with PostgreSQL on the same box: the 4 GB plan at $59.99/year. Planning queue mode or a local LLM alongside n8n: go straight to 6 GB or 8 GB, because upgrading mid-year moves you off the special pricing. And if a 30-day refund window matters more to you than a multi-year discount, a pricier managed provider is the honest choice — RackNerd's own pricing model assumes you're committing for a year.

Before you order, confirm two things: the datacenter location matches where your webhooks originate, and you're on a yearly special rather than a monthly equivalent, since the lifetime rate only applies to the specials. After that it's provision, click, deploy. 👉 [Pick your RackNerd VPS plan and get n8n running today](https://bit.ly/RacKnerd)
