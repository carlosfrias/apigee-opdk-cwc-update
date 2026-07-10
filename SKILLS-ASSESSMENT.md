# Skills Assessment — apigee-opdk-cwc-update

> **Skill domain:** Apigee OPDK configuration management — idempotent property-file updates via the Customer-Writable Configuration (CWC) layer. Part of the broader Apigee platform-operations portfolio; see the [`bap_coe` portfolio hub →](https://github.com/carlosfrias/apigee-hybrid-workspace/blob/master/SKILLS-ASSESSMENT.md) for the full corpus.

---

## Why this role is notable

- **The supported configuration interface.** CWC (Customer-Writable Configuration) is Apigee's official mechanism for persisting runtime configuration overrides — properties placed in `/opt/apigee/customer/application/<component>.properties` survive upgrades and are merged on top of defaults. This role provides a structured, validated Ansible interface for applying those overrides.
- **Idempotent by design.** Uses `lineinfile` with `regexp` matching on the key, so re-running the role updates the value without creating duplicates. Each property is keyed, not positional.
- **Backed up before modification.** `backup: yes` on every `lineinfile` ensures the original file is preserved before any change — no silent mutations.

---

## Expertise demonstrated

> Ansible is the medium. The engineering evidence lives in the [project README →](README.md). What follows is the skills assessment for the business reader.

- **Apigee CWC configuration discipline** — the CWC layer is the supported interface for runtime configuration overrides. This role makes CWC updates structured (validated `key`/`value`/`file_name` entries) rather than ad-hoc `sed` commands.
- **Idempotent property management** — `lineinfile` with `regexp` key matching ensures re-running the role is safe. No duplicates, no ordering issues.
- **Backup-before-modify discipline** — every change creates a backup. If something goes wrong, the original file is recoverable.
- **Interface-based role design** — the role accepts a list of `cwc_properties` dicts, not free-form file edits. Each entry has a `key`, `value`, and `file_name` — the interface validates the structure before applying.

---

## How this shows the expertise

The expertise is not "editing properties files" — it is **designing a structured, validated interface for the CWC layer** so that configuration changes are idempotent, backed up, and type-safe. The `cwc_properties` list-of-dicts interface is the abstraction that turns "edit a properties file" into "declare what the configuration should be." That is configuration management discipline, not file editing.

---

## Related expertise

| Skill | Repository | Assessment |
|-------|-----------|-----------|
| Apigee Hybrid / K8s automation (collection) | [`apigee-hybrid-workspace`](https://github.com/carlosfrias/apigee-hybrid-workspace) | [SKILLS-ASSESSMENT.md →](https://github.com/carlosfrias/apigee-hybrid-workspace/blob/master/SKILLS-ASSESSMENT.md) ✅ portfolio hub |
| Centralized defaults + runtime topology | [`apigee-opdk-setup-default-settings`](https://github.com/carlosfrias/apigee-opdk-setup-default-settings) | [SKILLS-ASSESSMENT.md →](https://github.com/carlosfrias/apigee-opdk-setup-default-settings/blob/master/SKILLS-ASSESSMENT.md) ✅ |

---

## Provenance

Authored and maintained by **Carlos Frias** during his tenure on Apigee Edge Private Cloud. This skills assessment is the companion to the engineering [README →](README.md).

## License

See [LICENSE](./LICENSE).