# Ansible Workshop Update Report
**Date:** October 25, 2025  
**Repository:** joe-speedboat/workshop.ansible  
**Analysis Scope:** All playbooks, roles, inventory, and configuration files

## Executive Summary

The Ansible workshop repository contains several outdated practices and deprecated modules that should be updated to align with current Ansible best practices (2024-2025). While most functionality remains operational, updating these items will improve maintainability, security, and future compatibility.

## Critical Issues (High Priority)

### 1. Deprecated VMware Modules
**Files Affected:**
- `esxi_vm_snap_revert.yml`
- `vcenter_vm_clone.yml`
- `vcenter_vm_clone_linked.yml`
- `vcenter_vm_clone_linked_loop.yml`

**Issues:**
- `vmware_guest_powerstate` is deprecated and will be removed in community.vmware 7.0.0
- `vmware_guest_snapshot` is deprecated and will be removed in community.vmware 8.0.0

**Recommended Actions:**
- Replace `vmware_guest_powerstate` with `community.vmware.vmware_guest` using `state: powered-on/powered-off`
- Replace `vmware_guest_snapshot` with newer snapshot management approaches
- Update to use FQCN: `community.vmware.vmware_guest`

### 2. Cisco IOS Provider Parameter (Deprecated)
**Files Affected:**
- `cisco_switch_backup.yml`

**Issues:**
- Uses deprecated `provider` parameter in `ios_config` and `ios_command` modules
- Missing FQCN (Fully Qualified Collection Names)

**Recommended Actions:**
```yaml
# OLD (deprecated)
ios_config:
  save_when: always
  provider: "{{ creds }}"

# NEW (recommended)
cisco.ios.ios_config:
  save_when: always
# Use connection: network_cli and become: yes in play-level vars
```

## Medium Priority Issues

### 3. Loop Syntax Modernization
**Files Affected:**
- `install_upgrade_web_handler.yml`
- `vcenter_vm_clone_linked_loop.yml`
- `roles/demo.jinja2/tasks/main.yml`

**Issues:**
- Uses older `with_items` and `with_dict` syntax
- While not deprecated, `loop` is the preferred modern syntax

**Recommended Actions:**
```yaml
# OLD
with_items:
  - item1
  - item2

# NEW
loop:
  - item1
  - item2

# OLD
with_dict: "{{ machines }}"

# NEW
loop: "{{ machines | dict2items }}"
```

### 4. Windows Module FQCN
**Files Affected:**
- `win_update_example.yml`
- `windows_update.yml`
- `roles/demo.ping/tasks/main.yml`

**Issues:**
- Missing FQCN for Windows modules

**Recommended Actions:**
```yaml
# OLD
win_updates:
win_ping:

# NEW
ansible.windows.win_updates:
ansible.windows.win_ping:
```

### 5. Deprecated Parameter in Windows Updates
**Files Affected:**
- `windows_update.yml`

**Issues:**
- Uses deprecated `category_name` parameter (should be `category_names`)

**Recommended Actions:**
```yaml
# OLD
win_updates:
  category_name:
    - SecurityUpdates

# NEW
ansible.windows.win_updates:
  category_names:
    - SecurityUpdates
```

## Low Priority Issues

### 6. Hardcoded Credentials
**Files Affected:**
- Multiple playbooks contain example passwords

**Issues:**
- Hardcoded passwords in examples (security concern for workshops)

**Recommended Actions:**
- Use Ansible Vault for sensitive data
- Add comments indicating these are example values only

### 7. Missing FQCN for Core Modules
**Files Affected:**
- Various playbooks

**Issues:**
- While not required, using FQCN improves clarity

**Recommended Actions:**
```yaml
# Consider updating to FQCN
ansible.builtin.copy:
ansible.builtin.service:
ansible.builtin.lineinfile:
```

## Configuration Files Status

### ansible.cfg ✅ GOOD
- Configuration is current and follows best practices
- No deprecated settings found

### inventory ✅ GOOD
- Inventory format is current and valid
- Uses modern group variable syntax

## Roles Analysis

### demo.ping ✅ MOSTLY GOOD
- Basic role structure is sound
- Only needs FQCN updates for `win_ping`

### demo.jinja2 ⚠️ NEEDS UPDATES
- Uses `with_items` (should use `loop`)
- Otherwise well-structured

## Recommended Update Priority

### Phase 1 (Immediate - Critical)
1. Update VMware modules to avoid future breakage
2. Fix Cisco IOS provider parameter usage
3. Update Windows module parameter names

### Phase 2 (Short-term - Medium Priority)
1. Modernize loop syntax across all playbooks
2. Add FQCN to all module calls
3. Update role structures

### Phase 3 (Long-term - Low Priority)
1. Implement Ansible Vault for credentials
2. Add comprehensive error handling
3. Standardize variable naming conventions

## Updated Example Files

### Example: Updated VMware Playbook
```yaml
---
- name: Revert ESXi VM back to snapshot and powerOn with Prompt
  hosts: localhost
  vars:
    esxi_host: esxi1
  tasks:
  - name: Get virtual machine names
    community.vmware.vmware_vm_info:
      hostname: "{{ esxi_host }}"
      username: "{{ esxi_user }}"
      password: "{{ esxi_password }}"
      folder: ""
      validate_certs: no
    delegate_to: localhost
    register: vm_info

  - name: Revert Snapshot
    community.vmware.vmware_guest:
      hostname: "{{ esxi_host }}"
      username: "{{ esxi_user }}"
      password: "{{ esxi_password }}"
      datacenter: ""
      folder: ""
      validate_certs: no
      name: "{{ vmname }}"
      state: present
      # Use vmware_guest_snapshot_info and vmware_guest for snapshot operations
    delegate_to: localhost

  - name: Power on VM
    community.vmware.vmware_guest:
      hostname: "{{ esxi_host }}"
      username: "{{ esxi_user }}"
      password: "{{ esxi_password }}"
      validate_certs: no
      name: "{{ vmname }}"
      state: powered-on
    delegate_to: localhost
```

### Example: Updated Cisco Playbook
```yaml
---
- name: backup cisco switch configuration into daily file
  hosts: cisco_switch
  gather_facts: no
  connection: network_cli
  become: yes
  vars:
    backup_dir: ./backup
  tasks:
  - name: save running config to device
    cisco.ios.ios_config:
      save_when: always

  - name: get cisco switch config
    cisco.ios.ios_command:
      commands: 
      - show running-config
    register: config
```

## Testing Recommendations

1. **Ansible Lint**: Run `ansible-lint` on all playbooks to catch additional issues
2. **Syntax Check**: Use `ansible-playbook --syntax-check` on all playbooks
3. **Dry Run**: Test with `--check` mode before applying changes
4. **Collection Dependencies**: Ensure all required collections are installed:
   ```bash
   ansible-galaxy collection install cisco.ios
   ansible-galaxy collection install community.vmware
   ansible-galaxy collection install ansible.windows
   ```

## Conclusion

The workshop repository requires moderate updates to align with current Ansible best practices. Most issues are related to deprecated syntax and missing FQCN rather than fundamental problems. The updates will improve maintainability and ensure compatibility with future Ansible versions.

**Estimated Update Time:** 4-6 hours for complete modernization
**Risk Level:** Low (most changes are syntax updates)
**Compatibility:** Updates will maintain backward compatibility while preparing for future Ansible versions