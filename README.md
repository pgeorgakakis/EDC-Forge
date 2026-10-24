# EDC-Forge

**Generate a working [Eclipse Dataspace Components (EDC)](https://github.com/eclipse-edc/Connector) node from a short questionnaire.**

> **Status: early stage, no code yet.** This repository holds the plan, the scope and the discussion.
> Feedback is very welcome: please open an issue or join the discussion linked below.

## The problem

EDC is the open-source
basis of many European data spaces. Deploying an EDC node by hand is still slow and error-prone. You have to:

- set up separate control and data planes;
- connect an identity provider and a secrets store;
- write [ODRL (Open Digital Rights Language)](https://www.w3.org/TR/odrl-model/) usage policies by hand;
- configure CORS, TLS and endpoints consistently.

The [adopter documentation](https://eclipse-edc.github.io/documentation/for-adopters/) helps, but users still report that
"the setup of an EDC is complicated" ([eclipse-edc/Connector#4136](https://github.com/eclipse-edc/Connector/issues/4136)).
For SMEs and public administrations without in-house data space experts, this setup cost is often where adoption stops.

The [Minimum Viable Dataspace (MVD)](https://github.com/eclipse-dataspace-hub/MinimumViableDataspace) is an excellent
learning playground, but it is deliberately not a deployment tool. Its maintainers have said that automation like this
belongs in a separate repository
([MinimumViableDataspace discussion #598](https://github.com/eclipse-dataspace-hub/MinimumViableDataspace/discussions/598)).
This is that repository.

## What EDC-Forge will do

EDC-Forge adds the missing step **before deployment**:

1. **Guided requirements dialogue (web UI and CLI).** You answer questions about your connector's role, storage,
   identity provider, transfer types and network exposure.
2. **Generated artefacts.** EDC-Forge outputs:
   - environment and configuration files;
   - a [Docker Compose](https://docs.docker.com/compose/) script that uses [Keycloak](https://www.keycloak.org/) and
     [HashiCorp Vault](https://developer.hashicorp.com/vault), including a development-tier issuer for the
     [Decentralized Claims Protocol (DCP)](https://github.com/eclipse-dataspace-dcp/decentralized-claims-protocol).
   - ODRL policies generated from a small, documented set of patterns (open access, credential-gated,
     time-bounded), following the [ODRL profile for data spaces](https://w3c.github.io/odrl/profile-dataspaces/).
3. **Validation before first use:**
   - static checks on endpoints, secrets, CORS/TLS and control/data-plane consistency;
   - a smoke test in which two generated participants complete a catalog request, a contract negotiation and a
     transfer.

**Design principles**

- **No fork.** EDC-Forge targets stock upstream EDC images and outputs plain configuration that you own and can edit.
- **Standards-based.** EDC-Forge targets the
  [Dataspace Protocol 2025-1](https://eclipse-dataspace-protocol-base.github.io/DataspaceProtocol/2025-1-err2/) and the
  [Decentralized Claims Protocol 1.0](https://projects.eclipse.org/projects/technology.dataspace-dcp/releases/1.0.0),.
- **Tracks upstream.** EDC [releases often](https://github.com/eclipse-edc/Connector/releases), so a versioned
  profile/template layer is tested in CI against two or more pinned EDC releases. A new upstream release means updating
  a profile, not rewriting the generator.
- **Open.** Apache 2.0, developed in the open.

## How it relates to other projects

- **[Eclipse Dataspace Components (EDC)](https://github.com/eclipse-edc/Connector):** EDC-Forge configures EDC and does
  not replace or fork it. Documentation gaps we find will be reported upstream.
- **[Minimum Viable Dataspace (MVD)](https://github.com/eclipse-dataspace-hub/MinimumViableDataspace):** a learning
  scenario to copy. EDC-Forge generates a configuration that fits your own requirements.
- **[Connector Fabric Manager (CFM)](https://projects.eclipse.org/projects/technology.cfm):** an Eclipse project
  providing a multi-tenant management plane for operators running EDC. EDC-Forge is complementary: it works before
  deployment, is single-tenant and outputs plain configuration. We want to coordinate with CFM and contribute where the
  two overlap.
- **[Eclipse Dataspace Hub](https://projects.eclipse.org/projects/technology.dataspace-hub):** we plan to offer tested
  templates and worked examples.

## Roadmap

| Phase | Goal |
|-------|------|
| Months 1–2 | Deploy a two-participant EDC exchange by hand and document every manual step. This is our baseline and a go/no-go check. |
| By month 6 | Generator core: outputs a configuration set and a Compose stack that boots. Public demo at the EDC community call. |
| By month 10 | Web UI, CLI and baseline ODRL scaffolding. |
| By month 12 | Validation layer, the two-participant smoke test, CI-matrix and TCK-aligned results, documentation and an onboarding guide, plus the first Apache 2.0 release. |

## Who this is for

- SMEs and public administrations that want to run their own data space node;
- the integrators who set up nodes for them;
- anyone learning EDC who wants a reproducible, documented starting point.

## Get involved

- **Ideas, requirements or "this would never work for us because…":** open an issue in this repository.
- **Discussion:** [eclipse-dataspace-hub / MinimumViableDataspace Discussions](https://github.com/eclipse-dataspace-hub/MinimumViableDataspace/discussions)
- **Eclipse Dataspace Working Group mailing list:** [dataspace-wg](https://accounts.eclipse.org/mailing-list/dataspace-wg)

## Licence

Apache License 2.0. See [LICENSE](LICENSE).

© SolutiONN
