# Osvědčené postupy pro GitHub Actions Workflow

Tento dokument popisuje osvědčené postupy pro vytváření a údržbu GitHub Actions workflows v repozitářích SQLoot.

## Obecné principy

### Bezpečnost na prvním místě
- ✅ Používejte připnuté verze akcí s SHA hashy pro produkci
- ✅ Omezte oprávnění workflow na minimum
- ✅ Nikdy nelogujte secrets nebo citlivá data
- ✅ Používejte vestavěnou správu secrets v GitHubu
- ✅ Povolte kontrolu závislostí pro workflows

### Výkon
- ✅ Vhodně cachujte závislosti
- ✅ Používejte souběžné joby, když je to možné
- ✅ Optimalizujte hloubku checkoutu (shallow clones)
- ✅ Rušte nadbytečné běhy workflow
- ✅ Používejte matrix strategie pro paralelní testování

### Udržovatelnost
- ✅ Používejte opakovaně použitelné workflows pro běžné úkoly
- ✅ Udržujte workflows DRY (Don't Repeat Yourself)
- ✅ Dokumentujte složité workflows
- ✅ Používejte konzistentní konvence pojmenování
- ✅ Pravidelně aktualizujte verze akcí

## Pokyny pro šablony Workflow

### CI Workflow

**Spouštěče:**
```yaml
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
```

**Povinné joby:**
- Linting a formátování
- Kontrola typů (pro TypeScript)
- Unit testy
- Integrační testy (pokud je to aplikovatelné)

**Optimalizace:**
```yaml
# Zrušit probíhající běhy pro PRs
concurrency:
  group: ${{ github.workflow }}-${{ github.event.pull_request.number || github.ref }}
  cancel-in-progress: true
```

### Security Workflow

**Spouštěče:**
```yaml
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
  schedule:
    - cron: '0 0 * * 0'  # Týdně v neděli
```

**Povinné kontroly:**
- Audit závislostí
- Skenování secrets
- SAST (Static Application Security Testing)
- Kontrola licencí

**Oprávnění:**
```yaml
permissions:
  security-events: write
  contents: read
```

### Release Workflow

**Spouštěče:**
```yaml
on:
  push:
    tags:
      - 'v*'
```

**Osvědčené postupy:**
- Automatické generování changelogu
- Sémantické verzování
- Podepisování assetů
- Publikování Docker image (pokud je to aplikovatelné)
- Publikování NPM balíčků (pokud je to aplikovatelné)

## Připnutí verzí akcí

### Vývoj
```yaml
# OK pro šablony a příklady
- uses: actions/checkout@v4
```

### Produkce
```yaml
# Doporučeno: Připnout na SHA s komentářem verze
- uses: actions/checkout@b4ffde65f46336ab88eb53be808477a3936bae11  # v4.1.1
```

**Proč:** 
- Zabraňuje útokům na dodavatelský řetězec
- Zajišťuje reprodukovatelné buildy
- Chrání před škodlivými aktualizacemi

## Model oprávnění

### Princip nejmenších privilegií

**Výchozí (pouze pro čtení):**
```yaml
permissions:
  contents: read
```

**Specifická oprávnění pouze v případě potřeby:**
```yaml
permissions:
  contents: write      # Pro releases, commity
  pull-requests: write # Pro PR komentáře
  issues: write        # Pro issue komentáře
  security-events: write # Pro CodeQL
```

**Nikdy nepoužívejte:**
```yaml
permissions: write-all  # ❌ Příliš permisivní
```

## Správa Secrets

### Co dělat
✅ Používejte GitHub secrets pro citlivá data
✅ Používejte secrets specifické pro prostředí
✅ Pravidelně obměňujte secrets
✅ Používejte OIDC pro autentizaci s cloud providery (AWS, Azure, GCP)
✅ Automaticky maskujte secrets v logách

### Co nedělat
❌ Nikdy neechujte secrets do logů workflow
❌ Nepoužívejte secrets v PR buildech z forků
❌ Neukládejte secrets do kódu nebo konfiguračních souborů
❌ Nepoužívejte slabé šifrování

### Příklad
```yaml
steps:
  - name: Deploy
    env:
      API_KEY: ${{ secrets.API_KEY }}
    run: |
      # API_KEY je automaticky maskován v logách
      ./deploy.sh
```

## Strategie cachování

### Bun/Node.js závislosti
```yaml
- name: Setup Bun
  uses: oven-sh/setup-bun@v2
  with:
    bun-version: latest

- name: Cache dependencies
  uses: actions/cache@v4
  with:
    path: ~/.bun/install/cache
    key: ${{ runner.os }}-bun-${{ hashFiles('**/bun.lockb') }}
    restore-keys: |
      ${{ runner.os }}-bun-
```

### Build artefakty
```yaml
- name: Cache build
  uses: actions/cache@v4
  with:
    path: |
      dist/
      .tsbuildinfo
    key: ${{ runner.os }}-build-${{ hashFiles('src/**') }}
```

## Testování s Matrix

### Multi-platformní testování
```yaml
strategy:
  matrix:
    os: [ubuntu-latest, windows-latest, macos-latest]
    node-version: [18, 20, 21]
    exclude:
      # Vyloučit specifické kombinace v případě potřeby
      - os: windows-latest
        node-version: 18
```

### Fail-fast vs Continue on Error
```yaml
strategy:
  fail-fast: false  # Pokračovat v běhu ostatních jobů i když jeden selže
  matrix:
    # ...
```

## Ochrana prostředí

### Produkční nasazení
```yaml
environment:
  name: production
  url: https://example.com
```

**Konfigurace na GitHubu:**
- Požadovaní revieweři
- Časovač čekání
- Deployment větve (pouze main)

## Opětovná použitelnost Workflow

### Opakovaně použitelný Workflow
```yaml
# .github/workflows/reusable-test.yml
on:
  workflow_call:
    inputs:
      node-version:
        required: true
        type: string

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: ${{ inputs.node-version }}
      - run: npm test
```

### Volání opakovaně použitelného Workflow
```yaml
# .github/workflows/ci.yml
jobs:
  test:
    uses: ./.github/workflows/reusable-test.yml
    with:
      node-version: '20'
```

## Zpracování chyb

### Continue on Error
```yaml
- name: Optional check
  continue-on-error: true
  run: npm run optional-check
```

### Podmíněné kroky
```yaml
- name: Deploy
  if: github.ref == 'refs/heads/main' && success()
  run: ./deploy.sh
```

## Monitorování a notifikace

### Status odznaky
```markdown
![CI](https://github.com/SQLoot/repo/workflows/CI/badge.svg)
```

### Notifikace
- Používejte vestavěné notifikace GitHubu
- Zvažte Slack/Discord webhooky pro kritická selhání
- Nastavte emailové upozornění pro bezpečnostní problémy

## Časté úskalí

### ❌ Nedělat
```yaml
# Hardcodované secrets
env:
  API_KEY: "sk_live_12345..."

# Příliš permisivní
permissions: write-all

# Žádné připnutí verze
- uses: actions/checkout@latest

# Nebezpečná injekce skriptu
run: echo "${{ github.event.issue.title }}"
```

### ✅ Dělat
```yaml
# Používejte GitHub secrets
env:
  API_KEY: ${{ secrets.API_KEY }}

# Minimální oprávnění
permissions:
  contents: read

# Připnuté verze
- uses: actions/checkout@v4

# Bezpečné používání proměnných
run: |
  TITLE="${{ github.event.issue.title }}"
  echo "${TITLE}"
```

## Ladění Workflows

### Povolit Debug logování
Nastavte repository secrets:
- `ACTIONS_STEP_DEBUG`: `true`
- `ACTIONS_RUNNER_DEBUG`: `true`

### Lokální testování
Použijte [act](https://github.com/nektos/act) pro lokální testování workflows:
```bash
act -j test  # Spustit 'test' job
```

## Optimalizace výkonu

### Shallow Clones
```yaml
- uses: actions/checkout@v4
  with:
    fetch-depth: 1  # Stáhnout pouze nejnovější commit
```

### Sparse Checkout
```yaml
- uses: actions/checkout@v4
  with:
    sparse-checkout: |
      src/
      tests/
```

### Job artefakty
```yaml
- name: Upload artifact
  uses: actions/upload-artifact@v4
  with:
    name: build-output
    path: dist/
    retention-days: 7  # Vyčistit po 7 dnech
```

## Compliance a Auditování

### Audit logy
- Pravidelně kontrolujte běhy workflow
- Monitorujte neobvyklé vzory
- Kontrolujte neoprávněný přístup

### Požadované kontroly stavu
Konfigurace v ochraně větví:
- Všechny testy musí projít
- Bezpečnostní skeny musí projít
- Žádné high/critical zranitelnosti

## Kontrolní seznam kvality šablony

- [ ] Jasný, popisný název workflow
- [ ] Správně nakonfigurované spouštěče
- [ ] Minimální oprávnění udělena
- [ ] Závislosti cachovány
- [ ] Verze připnuty (pro produkci)
- [ ] Secrets bezpečně zpracovány
- [ ] Implementováno zpracování chyb
- [ ] Zahrnuta dokumentace
- [ ] Testováno izolovaně
- [ ] Dodržuje konvence SQLoot

## Zdroje

- [GitHub Actions dokumentace](https://docs.github.com/en/actions)
- [GitHub Actions bezpečnostní osvědčené postupy](https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions)
- [Awesome Actions](https://github.com/sdras/awesome-actions)

---

**Poznámka**: Udržujte workflows jednoduché, bezpečné a udržovatelné. Pravidelné revize a aktualizace jsou nezbytné.
