# Newad Conversion Tracking

## Overview (EN)
Conversion pixel template for Newad programmatic campaigns (web and cross-device/CTV mode).

Supports both web endpoint and CTV endpoint based on `crossDevice`.

## Use Cases (EN)
- Track conversion events for Newad campaigns in web flows.
- Support CTV/cross-device conversion flow with duplicate prevention.
- Standardize pixel dispatch via GTM template permissions.

## Installation (EN)
### Community Gallery
1. In GTM, open **Templates** -> **Tag Templates**.
2. Search for **Newad Conversion Tracking** in Community Template Gallery.
3. Add template and review permissions.

### Local Import
1. Open **Templates** -> **Tag Templates** -> **New**.
2. Import `template.tpl` from this repository root.
3. Save template and create a tag instance.

## Required Fields (EN)
| Field | Type | Required | Example | Impact |
|---|---|---|---|---|
| `organizationId` | text | Yes | `newad_brazil-ADV01` | Main advertiser/org identifier in endpoint URL. |
| `eventNumber` | text/number | Optional | `1` | Event code appended as `ms_event_num`. |
| `crossDevice` | checkbox | Optional | `true` | Switches to CTV endpoint and de-duplication flow. |


## Optional Fields (EN)
- `eventNumber` defaults to `1` if omitted in CTV mode.
- `crossDevice=false` keeps legacy web endpoint behavior.

## Consent/Privacy Notes (EN)
- This template does not set consent state.
- Use together with a Consent Mode template.
- Pixel dispatch is restricted to allowed domains in permissions.

## Triggering Recommendations (EN)
- Fire on conversion events only (e.g., purchase/signup confirmation).
- Avoid broad pageview triggers.
- In CTV mode, template storage avoids duplicate in-session requests.

## Permission Rationale (EN)
| Permission | Why required | Risk if removed |
|---|---|---|
| `get_referrer` | Reads referrer URL parameters (`msxt`, `msev`, `msck`). | Attribution parameters cannot be resolved. |
| `get_url` | Reads current page URL parameters. | Current URL-based attribution cannot be resolved. |
| `send_pixel` | Sends conversion pixel only to allowed domains. | No conversion request is sent. |
| `access_template_storage` | Prevents duplicate CTV firing in-session. | Duplicate CTV pixels may be sent. |


## Public Interface Contract (EN)
- Inputs: `organizationId`, `eventNumber`, `crossDevice`.
- Behavior:
  - `crossDevice=false`: sends web conversion request (`/m/open`).
  - `crossDevice=true`: sends CTV request (`/ctv-ga`) and blocks duplicate send by storage key.
- Operational output: `sendPixel` call to approved domains.

## Test Coverage (EN)
- Template includes 4 baseline scenarios:
  - URL build with `organizationId` + `eventNumber`
  - `crossDevice=true/false` behavior
  - `sendPixel` invocation on allowed domain flow
  - invalid/null fallback (`gtmOnFailure`)

## Troubleshooting (EN)
- `gtmOnFailure` in CTV mode: check `organizationId` is non-empty.
- No conversion seen: validate trigger scope and browser network filters.
- Wrong endpoint: verify `crossDevice` toggle in tag config.

## Support (EN)
- Email: `support@newad.com.br`
- SLA: critical <= 1 business day, standard <= 2 business days.

---

## Visao Geral (PT-BR)
Template de pixel de conversao para campanhas programmatic da Newad (modo web e cross-device/CTV).

Suporta endpoint web e endpoint CTV com base em `crossDevice`.

## Casos de Uso (PT-BR)
- Rastrear conversoes de campanhas Newad no fluxo web.
- Suportar fluxo CTV/cross-device com prevencao de duplicidade.
- Padronizar disparo de pixel com permissoes controladas no GTM.

## Instalacao (PT-BR)
### Gallery
1. No GTM, abra **Templates** -> **Tag Templates**.
2. Busque **Newad Conversion Tracking** na Community Template Gallery.
3. Adicione o template e revise as permissoes.

### Importacao local
1. Abra **Templates** -> **Tag Templates** -> **New**.
2. Importe `template.tpl` da raiz deste repositorio.
3. Salve e crie uma tag com o template.

## Campos (PT-BR)
| Field | Type | Required | Example | Impact |
|---|---|---|---|---|
| `organizationId` | text | Yes | `newad_brazil-ADV01` | Main advertiser/org identifier in endpoint URL. |
| `eventNumber` | text/number | Optional | `1` | Event code appended as `ms_event_num`. |
| `crossDevice` | checkbox | Optional | `true` | Switches to CTV endpoint and de-duplication flow. |


## Notas de Privacidade (PT-BR)
- Este template nao define estado de consentimento.
- Use em conjunto com template de Consent Mode.
- O pixel so pode ser enviado para dominios permitidos.

## Permissoes (PT-BR)
| Permission | Why required | Risk if removed |
|---|---|---|
| `get_referrer` | Reads referrer URL parameters (`msxt`, `msev`, `msck`). | Attribution parameters cannot be resolved. |
| `get_url` | Reads current page URL parameters. | Current URL-based attribution cannot be resolved. |
| `send_pixel` | Sends conversion pixel only to allowed domains. | No conversion request is sent. |
| `access_template_storage` | Prevents duplicate CTV firing in-session. | Duplicate CTV pixels may be sent. |


## Cobertura de Testes (PT-BR)
- O template inclui 4 cenarios base de validacao para endpoint, modo cross-device, envio de pixel e fallback de erro.

## Solucao de Problemas (PT-BR)
- Falha em CTV: valide `organizationId`.
- Sem conversao: valide trigger/evento e aba Network.
- Endpoint incorreto: revise `crossDevice`.

## Suporte (PT-BR)
- Email: `support@newad.com.br`
- SLA: critico <= 1 dia util, padrao <= 2 dias uteis.
