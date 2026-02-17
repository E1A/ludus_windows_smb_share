# Ansible Role: Windows SMB Share ([Ludus](https://ludus.cloud))

An Ansible Role that creates a Windows SMB share and configures both share permissions and NTFS ACLs for read and write groups.

## Requirements

- `ansible.windows` collection (included with ludus)

## Role variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

Required:

    ludus_windows_smb_share_name: ""
    ludus_windows_smb_share_path: ""
    ludus_windows_smb_share_read_group: ""
    ludus_windows_smb_share_write_group: ""

Optional:

    ludus_windows_smb_share_description: ""
    ludus_windows_smb_share_list: true
    ludus_windows_smb_share_full_users: 'BUILTIN\Administrators'
    ludus_windows_smb_share_create_path: true
    ludus_windows_smb_share_set_ntfs_acl: true
    ludus_windows_smb_share_read_acl_rights: "ReadAndExecute,Synchronize"
    ludus_windows_smb_share_write_acl_rights: "Modify,Synchronize"

## Dependencies

None.

## Example Playbook

```yaml
- hosts: smb_hosts
  roles:
    - e1a.ludus_windows_smb_share
  vars:
    ludus_windows_smb_share_name: corpfiles
    ludus_windows_smb_share_path: 'C:\Shares\corpfiles'
    ludus_windows_smb_share_read_group: 'E1A\Domain Users'
    ludus_windows_smb_share_write_group: 'E1A\Domain Admins'
```

## Example Ludus Range Config

```yaml
  - vm_name: "SRV01"
    hostname: "srv01"
    template: win2022-server-x64-template
    vlan: 10
    ip_last_octet: 2
    ram_gb: 8
    cpus: 2
    windows:
      sysprep: true
    domain:
      fqdn: e1a.local
      role: member
    roles:
      - e1a.ludus_windows_smb_share
    role_vars:
      ludus_windows_smb_share_name: Gabber
      ludus_windows_smb_share_path: 'C:\Shares\gabber'
      ludus_windows_smb_share_read_group: 'NT AUTHORITY\Authenticated Users'
      ludus_windows_smb_share_write_group: 'BUILTIN\Remote Desktop Users'

      # Optional
      ludus_windows_smb_share_description: 'Hakkenn'
      ludus_windows_smb_share_list: true
      ludus_windows_smb_share_full_users: 'BUILTIN\Administrators'
      ludus_windows_smb_share_create_path: true
      ludus_windows_smb_share_set_ntfs_acl: true
      ludus_windows_smb_share_read_acl_rights: 'ReadAndExecute,Synchronize'
      ludus_windows_smb_share_write_acl_rights: 'Modify,Synchronize'
```

## Ludus setup

```bash
ludus ansible roles add e1a.ludus_windows_smb_share
ludus range deploy -t user-defined-roles --only-roles e1a.ludus_windows_smb_share --limit <vm_name>
```

## License

MIT

## Author Information

This role was created by [E1A](https://x.com/7hreathunter), for [Ludus](https://ludus.cloud/).