# apigee-opdk-cwc-update — Apigee OPDK Customer-Writable Configuration Updates

> **An Ansible role that applies property overrides to Apigee Edge Private Cloud component `.properties` files under `/opt/apigee/customer/application/` — the supported interface for persisting configuration changes across upgrades.**

> [!NOTE]
> Engineering portfolio note — this project demonstrates Apigee OPDK configuration management discipline and idempotent property-file updates. See the [skills assessment →](SKILLS-ASSESSMENT.md) for the expertise applied.

The Customer-Writable Configuration (CWC) layer is Apigee's supported mechanism for persisting runtime configuration overrides. Properties placed in `/opt/apigee/customer/application/<component>.properties` survive upgrades and are merged on top of defaults. This role provides a structured, validated interface for applying those overrides via Ansible.

<!-- BEGIN Google Required Disclaimer -->

## Not Google Product Clause

This is not an officially supported Google product.
<!-- END Google Required Disclaimer -->

---

## What the role actually does

`tasks/main.yml` validates that `cwc_properties` is defined, then iterates over each entry using `loop_control` and delegates to `tasks/cwc-update.yml`.

`tasks/cwc-update.yml` validates that each item has `key`, `value`, and `file_name`, then uses `lineinfile` to:

1. **Create or update** the target `<component>.properties` file at `{{ apigee_home }}/customer/application/{{ cwc_property.file_name }}.properties`.
2. **Set ownership** to `{{ opdk_user_name }}:{{ opdk_group_name }}` (defaults: `apigee:apigee`).
3. **Set mode** `0644` — readable by all, writable by owner.
4. **Match by key** — `regexp: "^{{ cwc_property.key }}"` ensures only the matching key is updated (idempotent).
5. **Back up** the original file before modification (`backup: yes`).

---

## Role variables (selected)

| Variable | Default | Description |
|----------|---------|-------------|
| `cwc_properties` | *(required)* | List of dicts, each with `key`, `value`, `file_name` |
| `opdk_user_name` | `apigee` | OS user that owns Apigee files |
| `opdk_group_name` | `apigee` | OS group that owns Apigee files |
| `apigee_home` | `/opt/apigee` | Path to the Apigee installation home |

**`cwc_properties` entry format:**

```yaml
cwc_properties:
  - { key: 'conf_pg_hba_replication.connection', value: '{{ replication_string }}', file_name: 'postgresql' }
```

---

## Usage

```yaml
- hosts: pgmaster
  vars:
    replication_string: "host    replication     apigee        10.142.0.32/32            trust"
    cwc_properties:
      - { key: 'conf_pg_hba_replication.connection', value: '{{ replication_string }}', file_name: 'postgresql' }
  roles:
    - apigee-opdk-cwc-update
```

---

## Provenance

Authored and maintained by **Carlos Frias** during his tenure on Apigee Edge Private Cloud. One of the configuration-management roles in the `apigee-opdk-*` corpus — the same expertise is aggregated in the [`apigee-edge-opdk`](https://github.com/carlosfrias/apigee-edge-opdk) framework.

Contributions welcome — see [CONTRIBUTING.md](./CONTRIBUTING.md).

## License

See [LICENSE](./LICENSE).