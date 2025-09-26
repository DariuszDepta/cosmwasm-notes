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
