# eduroam-playbook Ansible playbook
berikut adalah [Ansible playbook](http://docs.ansible.com/ansible/latest/playbooks.html) untuk set up [eduroam](https://eduroam.org/) info lebih lanjut mengenai silahkan kunjungi [eduroamID](https://eduroamid.info/)

## menggunakan ansible playbook
semua konfigurasi berada di [group_vars/all](group_vars/all). Kamu harus mengubah file tersebut sehingga sesuai dengan enviroment-mu.

##Roles
### [freeradius-eduroam](roles/freeradius-eduroam)
untuk konfigurasi eduroam SP dan IdP sebagian besar mengikuti dokumentasi dan GÉANT [eduroam service providers](https://wiki.geant.org/display/H2eduroam/freeradius-sp) and [eduroam identity providers](https://wiki.geant.org/display/H2eduroam/freeradius-idp).

### [eapol_test](roles/eapol_test)

untuk testing authentication menggunakan [eapol_test](http://deployingradius.com/scripts/eapol_test/) and [rad_eap_test](https://github.com/CESNET/rad_eap_test).

# untuk langkah langkah instalasi eduroam SP dan Idp ikuti langkah berikut

add repository
```bash
add-apt-repository universe
apt update
```
install git dan ansible
```bash
apt -y install git ansible
```
clone ansible scripts
```bash
git clone https://github.com/aguswawan/eduroam-playbook
```
IRS setting (file config at group_vars/all)
```bash
cd eduroam-playbook
nano group_vars/all
```
change ansible group variable
```bash
---
# The upstream eduroam federation-level RADIUS servers
# These are for sample, not working; get yours from your NRO
# to get secret, please contact the responsible of NRO Indonesia (ITB or UII)
eduroam_flr_servers:
  - hostname: flr.uii.ac.id
    ip: 103.220.113.18
    port: 1812
    secret: MySharedSecret

# Your realm for eduroam (usually your primary DNS name)
radius_realm: uii.ac.id

# Details of test account(s) to create within your realm
radius_local_users:
  - username: eduroamuser
    password: bsiuii
```
install IRS - Run Ansible
```bash
ansible-playbook -i inventories/development site.yml
```
testing local and remote account
```bash
rad_eap_test -H localhost -P 1812 -S testing123 -m WPA-EAP -s eduroam  -e TTLS -2 PAP -u eduroamuser@uii.ac.id -p bsiuii
```
