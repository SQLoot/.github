# Doporučení nastavení repozitáře

Tento dokument popisuje doporučená nastavení pro repozitáře SQLoot k zajištění bezpečnosti, kvality a konzistence.

## Obecná nastavení

### Základní informace
- ✅ Přidat jasný popis
- ✅ Přidat relevantní témata/tagy
- ✅ Zahrnout URL webové stránky (pokud je to aplikovatelné)
- ✅ Přidat licenci (doporučeno MIT)
- ✅ Přidat README.md

### Funkce
- ✅ Povolit Issues
- ✅ Povolit Discussions (pro komunitní Q&A)
- ❌ Zakázat Wiki (místo toho použít README/docs složku)
- ❌ Zakázat Projects (použít GitHub Projects na úrovni organizace)
- ✅ Povolit Sponsorships (FUNDING.yml)

### Pull Requesty
- ✅ Povolit squash merging (doporučeno)
- ✅ Povolit merge commity
- ❌ Zakázat rebase merging (pro zachování historie)
- ✅ Automaticky mazat hlavní větve po merge
- ✅ Povolit PR auto-merge

## Pravidla ochrany větví

### Hlavní větev (`main`)

**Vyžadovat pull request před mergováním:**
- ✅ Požadovaná schválení: 1 (pro malé týmy) nebo 2 (pro kritické repozitáře)
- ✅ Zrušit zastaralá PR schválení při nových commitech
- ✅ Vyžadovat review od Code Owners (když existuje CODEOWNERS)
- ❌ Omezit kdo může zrušit pull request reviews (není potřeba pro malé týmy)

**Vyžadovat kontroly stavu před mergováním:**
- ✅ Vyžadovat úspěšné kontroly stavu
- ✅ Vyžadovat aktuální větve před mergováním
- Požadované kontroly:
  - CI / Quality Gates
  - CodeQL (pokud je povoleno)
  - Security Scan (pokud je to aplikovatelné)

**Další omezení:**
- ✅ Vyžadovat vyřešení konverzace před mergováním
- ✅ Vyžadovat podepsané commity (doporučeno pro bezpečnostně kritické repozitáře)
- ❌ Vyžadovat lineární historii (volitelné, závisí na workflow)
- ❌ Uzamknout větev (pouze pro archivované projekty)
- ❌ Omezit pushe (povolit všem s push přístupem)

**Pravidla aplikovaná na administrátory:**
- ✅ Zahrnout administrátory (doporučeno, ale může být zakázáno pro rychlejší nouzové opravy)

## Bezpečnost & Analýza

### Bezpečnost
- ✅ Povolit hlášení soukromých zranitelností
- ✅ Povolit Dependabot upozornění
- ✅ Povolit Dependabot bezpečnostní aktualizace
- ✅ Povolit Dependabot aktualizace verzí (přes dependabot.yml)

### Skenování kódu
- ✅ Povolit CodeQL analýzu
- ✅ Nakonfigurovat CodeQL pro všechny podporované jazyky
- ✅ Spustit při push a pull_request událostech
- ✅ Spustit týdenní naplánované skeny

### Skenování secrets
- ✅ Povolit skenování secrets
- ✅ Povolit push protection (zabraňuje náhodnému commitu secrets)

## Oprávnění Actions

### Obecné
- ✅ Povolit všechny akce a opakovaně použitelné workflows
- Alternativa: Povolit SQLoot akce a vybrané ne-SQLoot akce

### Oprávnění Workflow
- ✅ Číst obsah repozitáře a oprávnění balíčků (výchozí)
- Povolit "Read and write" pouze pro specifické workflows, které to potřebují

### Fork Pull Request Workflows
- ✅ Vyžadovat schválení pro prvotní přispěvatele
- ✅ Vyžadovat schválení pro všechny externí spolupracovníky

## Code Owners (CODEOWNERS)

Vytvořte soubor `CODEOWNERS` v kořenovém adresáři nebo `.github/`:

```
# Výchozí vlastníci
* @maintainer-username

# Specifické cesty
/docs/ @doc-team
/.github/ @devops-team
/security/ @security-team
```

## Webhooky & Integrace

### Doporučené integrace
- CodeQL (vestavěné)
- Dependabot (vestavěné)
- Volitelné:
  - Renovate Bot (alternativa k Dependabot)
  - Codecov (pro pokrytí testy)
  - Sentry (pro sledování chyb)

## GitHub Apps

### Doporučené aplikace
- **GitHub Actions** - CI/CD automatizace
- **Dependabot** - Správa závislostí
- **CodeQL** - Bezpečnostní analýza

## Rulesets (Beta)

Zvažte použití Repository Rulesets pro pokročilejší scénáře:
- Vynutit podepisování commitů
- Omezit změny cest souborů
- Omezit commity od neověřených uživatelů
- Vyžadovat úspěšné workflows

## Šablony

Ujistěte se, že existují tyto šablony:
- ✅ Issue šablony (hlášení bugů, žádosti o funkce)
- ✅ Pull request šablona
- ✅ Pokyny pro přispívání
- ✅ Kodex chování
- ✅ Bezpečnostní politika

## Řízení přístupu

### Oprávnění týmů
- **Maintainerři**: Admin přístup
- **Core přispěvatelé**: Write přístup
- **Externí přispěvatelé**: Žádný přímý přístup (pouze PRs)

### Obejití ochrany větví
Povolit pouze pro:
- Nouzové hotfixy (s řádnou dokumentací)
- Automatizované release procesy (přes GitHub Apps/Actions)

## Monitorování

### Pravidelné revize
- Měsíčně: Revize otevřených issues a PRs
- Čtvrtletně: Revize bezpečnostních upozornění a závislostí
- Ročně: Revize a aktualizace všech politik a šablon

### Metriky ke sledování
- Čas PR merge
- Čas řešení issue
- Bezpečnostní zranitelnosti
- Pokrytí kódem
- Úspěšnost CI/CD

## Kontrolní seznam implementace

- [ ] Nakonfigurovat ochranu větví pro `main`
- [ ] Přidat CODEOWNERS soubor
- [ ] Povolit bezpečnostní funkce
- [ ] Nastavit Dependabot
- [ ] Nakonfigurovat CodeQL
- [ ] Přidat požadované kontroly stavu
- [ ] Nakonfigurovat oprávnění Actions
- [ ] Zkontrolovat a aktualizovat šablony
- [ ] Dokumentovat specifická nastavení repozitáře

---

**Poznámka**: Toto jsou doporučení. Upravte podle velikosti týmu, zralosti projektu a specifických požadavků.
