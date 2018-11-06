# eduroam-playbook Ansible playbook
berikut adalah [Ansible playbook](http://docs.ansible.com/ansible/latest/playbooks.html) untuk set up [eduroam](https://eduroam.org/) info lebih lanjut mengenai silahkan kunjungi [eduroamID](https://eduroamid.info/)

## menggunakan ansible playbook
semua konfigurasi berada di [group_vars/all](group_vars/all). Kamu harus mengubah file tersebut sehingga sesuai dengan enviroment-mu.

## Roles
### [freeradius-eduroam](roles/freeradius-eduroam)
untuk konfigurasi eduroam SP dan IdP sebagian besar mengikuti dokumentasi dan GÉANT [eduroam service providers](https://wiki.geant.org/display/H2eduroam/freeradius-sp) and [eduroam identity providers](https://wiki.geant.org/display/H2eduroam/freeradius-idp).

### [eapol_test](roles/eapol_test)

untuk testing authentication menggunakan [eapol_test](http://deployingradius.com/scripts/eapol_test/) and [rad_eap_test](https://github.com/CESNET/rad_eap_test).

# Langkah-langkah instalasi eduroam SP dan IdP.

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

# untuk konfigurasi dibawah ini akan anda dapatkan dari NRO Indonesia(ITB & UII).
eduroam_flr_servers:
  - hostname: flr1.uii.ac.id
    ip: 103.220.113.18
    port: 1812
    secret: MySharedSecret

# realm anda untuk eduroam(biasanya domain institusi anda, misal:uii.ac.id atau itb.ac.id)
radius_realm: university.ac.id

# user dan password local user di institusi anda
radius_local_users:
  - username: eduroamuser
    password: passworduser
```
install IRS - Run Ansible
```bash
ansible-playbook -i inventories/development site.yml
```
testing local user
```bash
rad_eap_test -H localhost -P 1812 -S testing123 -m WPA-EAP -s eduroam  -e TTLS -2 PAP -u eduroamuser@university.ac.id -p passworduser
```
testing remote user
```bash
rad_eap_test -H localhost -P 1812 -S testing123 -m WPA-EAP -s eduroam  -e PEAP -2 MSCHAPV2 -u demouser@uii.ac.id -p rahasia
```
