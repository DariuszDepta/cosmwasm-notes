# Maintainers guide

## Proposed table of contents for a maintainers guide

### Introduction and purpose

- What is this guide, and who is it for
- What is a “maintainer” (roles & responsibilities)
- How this guide relates to contributor / developer docs
- How to propose changes to this guide

### Project structure and ecosystem

- Overview of the organization’s repositories, modules, and dependencies
- How new repos or submodules should be organized (naming, layout, conventions)
- Cross-repo dependencies and versioning policies
- Shared libraries, common tooling, templates

### Governance and decision-making

- Governance model / charter
- Decision-making processes (voting, consensus, major vs minor decisions)
- Role definitions (core maintainers, module maintainers, release lead, etc.)
- Onboarding and offboarding maintainers
- Conflict resolution and escalation paths

### Contribution workflow

- Issue triage / categorization / labeling
- Pull request standards & reviews
- Code review checklist / expectations
- Merging policy (who can merge, required approvals, status checks)
- Branching strategy (main, dev, feature branches, long-lived branches)
- Release process & versioning (semver, tagging, changelogs)
- Backporting, patch releases, hotfix strategy

### Quality and testing

- Continuous integration / automated testing setup
- Test coverage guidelines & metrics
- Integration testing / end-to-end testing across modules
- Performance testing / benchmarks
- Security audits, fuzzing, static analysis
- Code health, refactoring policy

### Documentation and onboarding

- Documentation standards (style, location, tooling)
- API / reference documentation maintenance
- Tutorials, examples, “getting started” guides
- Onboarding new contributors & maintainers
- Internal vs user-facing documentation
- Keeping docs in sync with code

### Communication and community

- Channels of communication (chat, mailing list, forum, issue tracker)
- Maintaining transparency (roadmaps, meeting notes, issue tracker)
- Maintaining a positive culture & community norms
- Managing expectations (response time guidelines, backlog, “help wanted” tags)
- Code of Conduct and handling misconduct

### Release and deployment

- Release cadence and schedule
- Release checklist (builds, artifacts, signing, packaging)
- Publishing (GitHub releases, package registries)
- Post-release checks, monitoring & metrics
- Rollback strategy / hotfix process

### Maintenance and lifecycle

- Dependency updates / upgrades (third-party, ecosystem libraries)
- Deprecation policy (how to phase out APIs, modules)
- Technical debt management
- Security patches and vulnerability response
- End-of-life of modules

### Governance of external interfaces

- Public APIs / ABI / compatibility guarantees
- Backwards compatibility guarantees & versioning
- Intermodule contracts, shared interfaces
- Compatibility across chains / runtimes (if relevant)

### Security and trust

- Security practices for maintainers (credentials, keys, secrets)
- Handling security disclosures / vulnerability reports
- Signing / verifying release artifacts
- Audit / review process, bug bounty (if applicable)

### Operational and infrastructure setup

- CI/CD infrastructure (which providers, how to grant access)
- Build environments, resource quotas, caching
- Monitoring, alerting, logging for critical services
- Backup, recovery, failover for infrastructure

### Access control and permissions

- Organization / repository permissions model
- Granting and revoking access (repos, CI, infrastructure)
- Role-based access and least privilege
- Security audits of access

### Onboarding and offboarding of maintainers

- Criteria for becoming a maintainer
- Mentorship and probation period
- Documentation and training for new maintainers
- Offboarding process (revoking access, transferring responsibilities)

### Tooling and automation

- Recommended tools (linting, bots, automation)
- Bot / automation policy (what is acceptable, what needs human oversight)
- Scripts and templates (issue templates, PR templates)
- Dashboarding and metrics for project health

### Metrics and project health monitoring

- Key metrics (issue backlog, PR latency, release frequency, code coverage)
- Dashboards and periodic reviews
- Community growth / engagement metrics
- Technical debt / code health metrics

### Troubleshooting and FAQ for maintainers

- Common challenges and recommended practices
- How to handle overloaded maintainers, burn-out, resource constraints
- FAQ: “Can I merge this?”, “Who approves?”, etc.
- Escalation guide / when to involve others

### Appendices and templates

- Template for “New Repository Setup” checklist
- Issue / PR templates
- Release checklist template
- Maintainer onboarding checklist
- Sample code review checklist
- Glossary of terms

## Some kind of detailed outline

1. Introduction and purpose
 
   - Goal of this guide:
     - Provide maintainers with a clear reference for workflows, standards, and governance.

   - Who is a maintainer?
     - Core maintainers vs module maintainers
     - Expected responsibilities and commitments

   - Scope of this guide:
     - Covers repo management, code quality, releases, security, community interaction.

   - Updating the guide:
     - Process for proposing changes or improvements.

2. Project structure and ecosystem

   - Organization overview:
     - CosmWasm repositories (e.g., wasm, contracts, vm, bindings)
     - Dependency map between modules

   - Repo conventions:
     - Naming, directory layout, module boundaries
     - Template repos for new contracts / modules

   - Cross-repo dependencies:
     - Version alignment
     - Upgrade procedures for breaking changes

   - Shared tooling:
     - CI/CD pipelines, linters, code generators

3. Governance and decision-making

   - Decision model:
     - Consensus vs voting
     - Approval thresholds

   - Roles and responsibilities:
     - Core maintainers, module owners, release leads
     - 
   - Onboarding/offboarding maintainers:
     - Criteria for promotion, probation period

   - Conflict resolution:
     - Escalation paths, mediators, dispute resolution

4. Contribution workflow

   - Issue triage:
     - Labels (bug, enhancement, help wanted)
     - Prioritization and backlog management

   - Pull request standards:
     - Required CI checks, review count, branch naming

   - Code review checklist:
     - Correctness, security, style, performance, documentation

   - Merging policy:
     - Who can merge, fast-tracking critical fixes
     - 
   - Branching strategy:
     - main = stable, dev = ongoing development

   - Release process & versioning:
     - Semver conventions, changelog updates
     - Backporting and hotfix strategy

5. Quality and testing

   - Automated testing:
     - Unit tests, integration tests, fuzzing

   - Cross-chain integration tests
   
   - Performance benchmarks:
     - Gas consumption, execution speed
    
   - Security audits:
     - Internal vs third-party, frequency, reporting vulnerabilities

   - Code health:
     - Refactoring policy, deprecated features

6. Documentation and onboarding

   - Documentation standards:
     - Markdown style, inline docstrings, examples

   - User vs internal docs:
     - Public API docs, maintainers guides

   - Onboarding new contributors:
     - Getting started, first PR checklist

   - Maintainer training:
     - Responsibilities, CI/CD tools, repo access

7. Communication and community

   - Channels: Discord, GitHub Discussions, mailing lists
   - Transparency: Meeting notes, roadmap updates
   - Community norms: Code of Conduct, responding to issues
   - Managing expectations: SLA for issue responses, backlog visibility

8. Release and deployment

   - Release cadence: Monthly / milestone-based
   - Release checklist: Build, test, tag, package, sign
   - Artifact publishing: GitHub releases, npm/Cargo/Go packages
   - Post-release monitoring: Usage stats, errors, regressions
   - Rollback / hotfix strategy

9. Maintenance and lifecycle

   - Dependency updates: Automated tools, periodic review
   - Deprecation policy: Warning periods, migration guides
   - Technical debt tracking: Documenting, prioritizing
   - Security patches: Response plan for vulnerabilities
   - EOL of modules: Archiving, notifying users
 
10. Governance of external interfaces

    - Public APIs / ABI stability
    - Backwards compatibility guarantees
    - Inter-module contracts
    - Cross-chain or multi-runtime compatibility

11. Security and trust

    - Maintainer security practices: Keys, tokens, CI secrets
    - Handling vulnerability reports
    - Signing / verifying release artifacts
    - Audit & review schedule
 
12. Operational and infrastructure setup
 
    - CI/CD infrastructure: GitHub Actions, Docker images
    - Build environments: Reproducibility, caching
    - Monitoring & alerting: Build failures, test coverage drops
    - Backup & recovery: Repo backups, release artifact storage
 
13. Access control and permissions
 
    - Repo and org permissions: Teams, roles
    - Granting / revoking access: Onboarding/offboarding
    - Least privilege enforcement
    - Periodic access review
 
14. Onboarding and offboarding maintainers
 
    - Criteria & mentorship
    - Training & documentation
    - Transfer of responsibilities
    - Revoking access & offboarding checklist
 
15. Tooling and automation
 
    - Recommended tools: Linters, code formatters, bots
    - Automation rules: PR auto-assign, stale issue bot
    - Scripts & templates: PR templates, release scripts
    - Metrics dashboards: CI/CD, code coverage, issues
 
16. Metrics and project health
 
    - Key metrics: PR latency, release frequency, bug backlog
    - Technical debt metrics
    - Community engagement: Contributors, discussions
    - Periodic reviews & reporting
 
17. Troubleshooting and FAQ
 
    - Common problems: Merge conflicts, failing CI, dependency issues
    - Maintainer overload / burn-out
    - FAQ examples: "Who approves?", "Can I merge this?"
    - Escalation procedures
 
18. Appendices and templates
 
    - New repository setup checklist
    - Issue / PR templates
    - Release checklist template
    - Maintainer onboarding checklist
    - Code review checklist
    - Glossary: CosmWasm-specific terms
