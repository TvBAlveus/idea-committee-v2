# Weekly Validation-Routine — FALLBACK ONLY (nicht standardmäßig scheduled)

**STATUS: Diese Routine ist NICHT mehr im Standard-Schedule.** Stages 4+5 laufen seit Mai 2026 inline im Cowork-Daily-Run (siehe Cowork-Task `idea-kill-committee-cowork`).

Diese Datei bleibt als **Methodik-Referenz** + **Fallback-Routine** falls Cowork-Daily mal mehrere Tage ausfällt und sich ein Backlog an `3-promoted`-Ideen bildet die noch kein `anti_brief`/`customer_brief` haben.

## Wann manuell triggern

- Cowork-Daily war ≥3 Tage off (Backlog an `3-promoted`-Ideen ohne Validation-Update)
- Größerer Re-Validation-Run nötig (z.B. nach Methodik-Update sollen alle bestehenden `interview-pending`-Ideen neu durch Stage 4+5)

## Inhalt: Stage 4 (Hardcore Pre-Mortem) + Stage 5 (Tiefe Customer-Validation)

Identisch zu den entsprechenden Sektionen im Cowork-Task. Siehe vollständige Definitionen unten.

---

# STAGE 4: HARDCORE PRE-MORTEM (+10-15 Suchen pro Idee)

**Logik:** Steel-Manning der Anti-These. Aktiv versuchen die Idee zu töten.

## 5 Failure-Szenarien (PFLICHT — alle 5 Kategorien)

Für jede `3-promoted`-Idee:

1. **Wettbewerb-Killer:** Inkumbent/Startup launcht direkten Konkurrent mit besserem Distribution. Funding-Rounds, Job-Posts, Roadmap-Hints suchen.
2. **Regulatorik-Killer:** Why-Now-Trigger verschiebt sich, fällt weg, oder kreiert plötzlich Compliance-Burden. EU-Trilog-Status, BMWi-Konsultationen.
3. **Vertriebs-Killer:** target_customer kauft nicht zum unterstellten Preis. Pricing-Tolerance-Recherche bei Verbänden, Procurement, Buyer-Surveys.
4. **Tech-Killer:** Foundation-Model-Sprung commoditisiert den Wedge. KI-Modell-Roadmap-Suchen, Open-Source-Alternativen.
5. **Operativer-Killer:** Bootstrap bricht durch versteckte Kosten (Compliance, Integration, Service, Hardware-CapEx).

## Pro Szenario

- **2-3 gezielte Tavily-Suchen** (10-15 total für Stage 4)
- Quellen-Hierarchie: Primär > Validated-Aggregat > Anbieter-Direkt > Sekundär-Press. Schwache Quellen (Reddit, LinkedIn-Posts) zählen NICHT.
- **Wahrscheinlichkeits-Schätzung in %** mit Quellen-Beleg
- **Mitigation-Pfad** falls relevant

## STOP-KRITERIUM Stage 4

- **EIN Szenario ≥ 30% UND nicht mitigierbar** → Idee killen. `stage: "4-killed"` mit Trigger.
- **Alle 5 < 30% ODER mitigierbar** → `stage: "4-complete"`.

## Devil's-Advocate-Pflicht-Fragen

Mit Quellen-Beleg:
1. **"Wenn so offensichtlich, warum gibt's sie noch nicht?"** Markt-Reife, Tech-Trigger, oder echter Killer.
2. **"Was würde ein VC sofort als Killer nennen?"** 3 Hypothesen mit Widerlegung.
3. **"Welcher Vorgänger ist gescheitert?"** Postmortem-Suchen ("[Vertikal] startup failed", "[Wettbewerber] shutdown").

## Output: `anti_brief`

```javascript
anti_brief: {
  date: "DD.MM.YYYY",
  failure_scenarios: [
    {category, scenario, probability_pct, evidence, source, mitigation},
    // 5 total
  ],
  devils_advocate: {
    why_not_yet: "...",
    vc_killer_hypotheses: [{hypothesis, refuted_by}, ...],
    failed_predecessors: [{name, postmortem_source}, ...]
  },
  result: "4-complete" | "4-killed",
  killer_trigger: null | "Szenario X mit Y%"
}
```

# STAGE 5: TIEFE CUSTOMER-VALIDATION (+8-12 Suchen)

**Nur für `stage: "4-complete"`.**

**Logik:** Existiert der angenommene Buyer? Kauft er zum unterstellten Preis? Würde er wechseln?

## 5 Pflicht-Recherche-Felder

1. **Pool-Größe verifiziert:** 2+ unabhängige Quellen mit ≥1 Primärquelle (Verband + Stat.Bundesamt + Konferenz-Liste). Konkrete Zahl statt Schätzung.
2. **3-5 namentliche Beispiel-Buyer:** LinkedIn-Profile mit Rolle + Firma + Größe; ≥1 mit veröffentlichter Buyer-Journey (Case-Study, Konferenz-Talk, Podcast); ≥1 mit Procurement-relevanter Funktion.
3. **Anbieter-Pricing der `current_alternative`:** Direkt von Pricing-Pages, 3-5 Wettbewerber mit Tier-Differenzierung, mit Datum-Stempel.
4. **Switching-Cost-Schätzung mit Quelle:** Konkret in Zeit/Geld/Risiko. Migration-Guides, Case-Studies, Branchen-Reports.
5. **Buyer-Journey-Map:** Champion + Decision-Maker + Procurement-Stage (Sales-Cycle Monate) + Pricing-Tolerance, mit Quellen-Beleg.

## STOP-KRITERIUM Stage 5

- **Pool < 50% unterstellter Größe** ODER **kein namentlicher Buyer findbar** ODER **Switching-Cost > 12 Mo Pricing-Aequivalent** → `stage: "5-killed"`.
- Alle 5 Felder mit ≥1 Primär/Validated-Aggregat UND keine Hard-Killer → `stage: "interview-pending"`.

## Output: `customer_brief`

```javascript
customer_brief: {
  date: "DD.MM.YYYY",
  pool_size: {value, sources, confidence},
  example_buyers: [{name, role, company, size, linkedin, journey_source}, ...],
  current_alternative_pricing: [{vendor, tier, price_eur, pricing_page_date}, ...],
  switching_cost: {time_months, cost_eur, risk, sources},
  buyer_journey: {champion_role, decision_maker_role, sales_cycle_months, pricing_tolerance_eur, sources},
  result: "interview-pending" | "5-killed",
  killer_trigger: null | "..."
}
```

# QUELLEN-HIERARCHIE (gilt für Stage 4 + 5)

1. **Primärquelle** = Gesetzestext, Bilanz, Verband-Studie mit Methodik, EU-Funding-Register, Pricing-Page direkt
2. **Validated-Aggregat** = Crunchbase + Pitchbook + ≥1 unabhängige Bestätigung
3. **Anbieter-Direkt** = Customer-Logo, Case-Study, Karriereseite
4. **Sekundär-Press** = Handelsblatt, FAZ, Sifted, TechCrunch
5. **Schwach** = LinkedIn-Posts, Reddit → zählt NICHT

**Findings in Stage 4+5 erfordern `high` Confidence.**

# PUSH MIT REBASE-SCHUTZ

```bash
git -c safe.directory=$(pwd) pull --rebase origin main
git -c user.email="tillbuttlar@gmail.com" -c user.name="Till" -c safe.directory=$(pwd) add index.html
git -c user.email="tillbuttlar@gmail.com" -c user.name="Till" -c safe.directory=$(pwd) commit -m "Validation-Fallback: $(date +%Y-%m-%d) - [X] S4-complete, [Y] S5-complete, [Z] interview-pending, [K] killed"
git push origin main
```
