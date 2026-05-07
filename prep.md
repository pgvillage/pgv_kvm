# Preps

## Prepare ansible host

- Our example KVM setup has only one disk, which is both used for OS, data and WAL.
  In production, you should mke sure to have a separate DATA mounted as '/data/postgres/data' 
  and separate WAL disk mounted as '/data/postgres/wal'.
  You can change chese mountpaths in environments/cluster1/group_vars/all/pgvillage.yaml

- Make sure you can connect to all other hosts with a user with sudo privilleges
  **Note** If all users have the same username and password (e.. ldap auth), you can do without ssh keys.
  If not, alsoe setup passwordless authentication with ssh keys
- Make sure that all nodes (ansible, db hosts and backup server) can resolve eachother using either DNS or /etc/hosts
- Prepare the ansible host
  ```bash
  curl https://raw.githubusercontent.com/pgvillage/pgv_kvm/main/scripts/bootstrap_rocky-9.sh | bash
  ```
- setup inventory (~/git/pgv_kvm/environments/cluster1/hosts)
  - set proper hosts and proper ip adresses
