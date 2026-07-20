# GridPilot Home

**Version 1.0.0** · **Updated 2026-07-20**

Self-hosted home energy management for Powerwall-class systems.  
Monitor power flow, history, and server-side automations (including Free Nights 9 PM–7 AM).  
**Not affiliated with Tesla, Inc., Netzero Labs, or any utility.**

**Live Tesla without public exposure:** OAuth uses localhost paste (`http://127.0.0.1:8998/callback`). GridPilot stays on your LAN. Docker outbound internet is enough.

---

## Unraid (recommended): one container

```bash
# Project path on Unraid:
cd /mnt/user/appdata/gridpilot-home

# Remove old 3-container templates if present:
bash deploy/unraid/remove-multi-container-templates.sh --remove-containers

# Build all-in-one image + install Unraid template (Edit + Fixed IP + WebUI):
bash deploy/unraid/install-single-container.sh
```

Then: **Docker → Add Container → gridpilot-home** → set Fixed IP / secrets → Apply.  

**Full directions (install → simulation → live):** [docs/COMPLETE_INSTALL_TO_LIVE.md](docs/COMPLETE_INSTALL_TO_LIVE.md)  

Also: [docs/README.md](docs/README.md) · [docs/UNRAID_SINGLE_CONTAINER.md](docs/UNRAID_SINGLE_CONTAINER.md) · [docs/USER_SETUP_GUIDE.md](docs/USER_SETUP_GUIDE.md)

Inside the container: **API + SPA + worker + SQLite**.

---

## Local development

```bash
python -m pip install -e "packages/backend-core[dev]"
cd apps/web && npm install && cd ../..

# API
$env:SIMULATION_MODE="true"
$env:ENABLE_LIVE_COMMANDS="false"
$env:DATABASE_URL="sqlite+aiosqlite:///./gridpilot-dev.db"
$env:DATA_DIR="./data"
cd packages/backend-core
python -m uvicorn gridpilot.api.main:app --host 0.0.0.0 --port 8080 --reload

# Web (dev)
cd apps/web && npm run dev
```

---

## Tests

```bash
# Backend (includes exhaustive mocked live Fleet tests)
cd packages/backend-core
python -m pytest -q

# Frontend (includes UI quality regressions — no raw JSON dumps)
cd apps/web
npm test
```

---

## Safety defaults

| Variable | Default |
|----------|---------|
| SIMULATION_MODE | true |
| ENABLE_LIVE_COMMANDS | false |
| READ_ONLY_DEFAULT | true |

Live Tesla (LAN-only OAuth): [docs/COMPLETE_INSTALL_TO_LIVE.md](docs/COMPLETE_INSTALL_TO_LIVE.md) · [docs/GO_LIVE.md](docs/GO_LIVE.md) · [docs/live-validation.md](docs/live-validation.md)

---

## License

MIT — see [LICENSE](LICENSE). Third-party and graphics: [THIRD_PARTY_LICENSES.md](THIRD_PARTY_LICENSES.md).
