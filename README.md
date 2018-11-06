# eduroam-playbook Ansible playbook
berikut adalah [Ansible playbook](http://docs.ansible.com/ansible/latest/playbooks.html) untuk set up [eduroam](https://eduroam.org/)
untuk info lebih lanjut mengenai silahkan kunjungi [eduroamID](https://eduroamid.info/)

## menggunakan ansible playbook
semua konfigurasi berada di [group_vars/all](group_vars/all). Kamu harus mengubah file tersebut sehingga sesuai dengan enviroment-mu.

##Roles
### [freeradius-eduroam](roles/freeradius-eduroam)
untuk konfigurasi eduroam SP dan IdP sebagian besar mengikuti dokumentasi dan GÉANT [eduroam service providers](https://wiki.geant.org/display/H2eduroam/freeradius-sp) and [eduroam identity providers](https://wiki.geant.org/display/H2eduroam/freeradius-idp).

### [eapol_test](roles/eapol_test)

untuk testing authentication menggunakan [eapol_test](http://deployingradius.com/scripts/eapol_test/) and [rad_eap_test](https://github.com/CESNET/rad_eap_test).

# untuk langkah langkah instalasi eduroam SP dan Idp ikuti langkah berikut
