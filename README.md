# Helm Charts Repository

Welcome to the Helm Charts repository for **thiagofbarros**. This repository hosts Helm charts for deploying various applications on Kubernetes, served via GitHub Pages.

Repository URL: **https://thiagofbarros.github.io/helm-charts/**

---

## 🚀 How to Use

To use the charts from this repository, add it to your Helm configuration:

```bash
# Add the repository
helm repo add thiagofbarros https://thiagofbarros.github.io/helm-charts/

# Update your local charts cache
helm repo update
```

### Installing a Chart

For example, to install the `vaultwarden` chart:

```bash
helm install my-vaultwarden thiagofbarros/vaultwarden
```

---

## 📦 Available Charts

| Chart Name | Description | Default Version | App Version |
| :--- | :--- | :--- | :--- |
| **[vaultwarden](charts/vaultwarden)** | A Helm chart for Vaultwarden (unofficial Bitwarden server) | `1.0.0` | `1.33.2` |

---

## 🛠️ Development & Automation

This repository uses [Chart Releaser GitHub Action](https://github.com/helm/chart-releaser-action) to automate packaging and releasing Helm charts.

### Project Structure

```text
.
├── .github/workflows/   # CI/CD Workflows (GitHub Actions)
│   └── release.yaml    # Re-packages charts and updates gh-pages on push to main
├── charts/             # Unpackaged Helm charts (Source Code)
│   └── vaultwarden/    # Vaultwarden Chart
└── LICENSE             # Repository License
```

### Adding or Updating Charts

1. **Add a new chart** under the `charts/` directory or make changes to an existing chart.
2. If updating an existing chart, make sure to **bump the `version`** field in `charts/<chart-name>/Chart.yaml`.
3. Commit and push your changes to the `main` branch.
4. The GitHub Actions workflow will automatically:
   - Package the modified charts (`.tgz`).
   - Create a GitHub Release with the packaged asset.
   - Update the `index.yaml` on the `gh-pages` branch.
