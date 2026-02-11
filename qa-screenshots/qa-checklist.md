# QA Report: Juan Francisco Esteves

**Date:** 2026-02-11
**URL:** https://cofoundy.github.io/portfolio-juan-esteves/
**Status:** FAIL

## Source Data (Google Sheet)
- Name: Juan Francisco Esteves
- Email: juanfraesteves@gmail.com
- Social networks requested: LinkedIn, Instagram, ORCID, CTI Vitae (CONCYTEC)
- Style: Minimalista y limpio
- Colors: #062446 y #F4F6F8

## Data Validation
- [x] Name matches source (Sheet: "Juan Francisco Esteves", Page: "Juan Francisco Esteves")
- [x] Email matches source (Sheet: juanfraesteves@gmail.com, Page: mailto:juanfraesteves@gmail.com)
- [x] Job title plausible (config: "Research Biologist | PhD Candidate in Entomology" -- consistent with CV data)
- [x] Companies listed are plausible (Czech Academy of Sciences, James Cook University, Natural History Museum of Lima, NTNU University Museum)
- [x] Education institutions are plausible (Jihoceskа univerzita v Ceskych Budejovicich, Universidad Nacional Mayor de San Marcos)
- [x] Dates appear consistent with academic timeline
- [x] Publications have real DOI links (doi.org URLs for Nature, Scientific Reports, PLOS ONE, Wilson Journal)
- [ ] No hallucinated data detected -- CAVEAT: no research-notes.md or CV file was available to cross-reference against. Data appears credible but cannot be fully verified.

## Clean Deploy
- [x] No "Powered by" / "Made with" / "Built with" visible text
- [x] No "View source" / "View on GitHub" / "Fork this" template links
- [x] No "Lorem ipsum" / "Your name here" / "[placeholder]" text
- [x] No template watermarks visible to users
- [x] No "undefined" or "null" visible in content
- [x] No Astro logo or Vercel badge visible

## Technical
- [x] Page loads (HTTP 200)
- [x] CSS loads (HTTP 200) -- /portfolio-juan-esteves/_astro/index.z47Fn8Iq.css
- [x] Profile image loads (HTTP 200) -- /portfolio-juan-esteves/profile.jpg
- [x] Favicon loads (HTTP 200) -- /portfolio-juan-esteves/favicon.svg (initials "JE", navy #062446)
- [x] Astro config has both site + base correctly set
- [ ] Console errors -- UNABLE TO CHECK (Chrome MCP unavailable)

## Issues Found

### ISSUE 1 [FAIL - BROKEN LINKS]: Footer renders 3 empty social icons
**Severity:** High
**Description:** Footer.astro renders LinkedIn, Twitter, and GitHub icons unconditionally (no conditional check like Hero.astro does). Since config.ts has `linkedin: ""`, `twitter: ""`, `github: ""`, the rendered HTML is `<a href target="_blank" aria-label="LinkedIn">` -- three visible, clickable icons that link to nothing (empty href = navigates to current page).
**File:** /Users/styreep/cofoundy/projects/pollada/clients/2026-02-09_juan-francisco-esteves/src/components/Footer.astro (lines 47-120)
**Fix:** Add conditional rendering: `{siteConfig.social?.linkedin && ( <a href={siteConfig.social.linkedin} ...> )}` for LinkedIn, Twitter, and GitHub. Or remove these links from config entirely.

### ISSUE 2 [FAIL - MISSING SOCIAL LINKS]: Client's requested social networks not included
**Severity:** Medium
**Description:** The Google Sheet form shows the client requested: "LinkedIn, Instagram, ORCID, CTI Vitae (CONCYTEC)". However, config.ts has all social links as empty strings. The client's LinkedIn, Instagram, ORCID, and CTI Vitae URLs were not populated. This means none of the client's social presence is accessible from the portfolio.
**Fix:** Research and add the client's actual LinkedIn URL, Instagram URL, ORCID URL, and CONCYTEC CTI Vitae URL to config.ts. The config schema may need to be extended to support ORCID and CTI Vitae links.

### ISSUE 3 [WARNING - MISMATCHED CONTEXT]: Programming symbols background pattern for a biologist
**Severity:** Low (cosmetic)
**Description:** The Hero section uses a background SVG pattern called "programming-symbols" that renders code syntax characters (`</>`, `{}`, `=>`, `[]`, `()`, `::`, `==`, `++`, `;`). Juan Francisco Esteves is a Research Biologist / PhD Candidate in Entomology, not a software developer. These programming symbols are contextually inappropriate for his profession.
**File:** /Users/styreep/cofoundy/projects/pollada/clients/2026-02-09_juan-francisco-esteves/src/components/Hero.astro (lines 31-119)
**Fix:** Either remove the pattern, replace with biology-relevant symbols, or use a neutral geometric pattern.

### ISSUE 4 [WARNING - LANGUAGE INCONSISTENCY]: Spanish UI labels with English content
**Severity:** Low
**Description:** The page has `<html lang="es">` and all UI section headings are in Spanish ("Sobre Mi", "Proyectos", "Experiencia", "Educacion"), but all content (aboutMe, experience bullets, education achievements, project descriptions) is written in English. The client is based in Czech Republic with phone number +420 and an international academic profile. The form does not specify language preference, but the content being entirely in English suggests the UI should match.
**Fix:** Either change UI labels to English ("About Me", "Projects", "Experience", "Education") and set `lang="en"`, or translate the content to Spanish. English UI is recommended given the international academic audience.

### ISSUE 5 [WARNING - MISSING ACCENT]: "Educacion" missing tilde
**Severity:** Low (cosmetic)
**Description:** The section heading reads "Educacion" instead of "Educacion" (should be "Educacion" with accent: "Educacion"). Actually, the correct Spanish is "Educacion". Let me be precise: it should be "Educacion" with accent mark over the 'o' = "Educacion". The HTML shows the literal text "Educacion" without the accent.
**Fix:** If keeping Spanish, change to "Educacion" with proper accent. This is hardcoded in the Header.astro and repeated in Footer.astro and the Education section heading.

### ISSUE 6 [INFO - INCOMPLETE METADATA]: client-meta.md has unresolved placeholders
**Severity:** Low (internal only, not client-facing)
**Description:** The client-meta.md still has `<<VERIFICAR VOUCHER>>`, `<<VERIFICAR - NO INVENTAR>>`, `<<COPIAR NOTAS DEL FORMULARIO AQUI>>` placeholders. All source checkboxes are unchecked. This is an internal documentation gap but does not affect the deployed site.

### ISSUE 7 [WARNING - NO PROFILE PHOTO IN HERO]: Profile photo not displayed
**Severity:** Medium
**Description:** The profile.jpg exists and loads correctly (HTTP 200), but the Hero component does not include an `<img>` tag to display it. The client uploaded a professional photo. The hero section only shows text (greeting, name, title) with no photo. The profile photo exists in public/ but is never referenced in the HTML. For a minimal-mono template this may be by design, but the notes state "Foto profesional" as a feature.
**Fix:** Consider adding the profile photo to the hero section or about section.

## Summary

| Category | Result |
|----------|--------|
| Data accuracy | PASS (with caveat: no CV to cross-verify) |
| Clean deploy | PASS |
| Technical health | PASS |
| Social links | FAIL (3 broken empty-href icons in footer) |
| Content completeness | FAIL (client's requested socials not populated) |
| Visual appropriateness | WARNING (programming symbols for biologist) |
| Language consistency | WARNING (Spanish UI / English content mix) |

## Evidence
- Chrome MCP was unavailable during QA; no screenshots could be captured.
- All checks performed via curl HTTP requests and source file inspection.
