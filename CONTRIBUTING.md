# Contributing

Thanks for contributing! Keep changes focused and documented.

## Start
1. Fork the repo
2. Create a branch: `git checkout -b feature/your-change`
3. Commit: `git commit -m "Add ..." `
4. Push and open a PR

## Style
- YAML: 2-space indent
- Add comments for any non-obvious pin/behavior changes
- New features: include instructions in README

## Testing
- Validate ESPHome YAML: `esphome config esphome/fpr_nuki.yaml`
- Test on bench (USB) before OTA

## Security
- Never commit secrets (Wi-Fi, API keys, OTA passwords). Use Home Assistant secrets or `.secrets.yaml`.
