# Red Hat Advanced Cluster Security (RHACS) Operator Ansible Automation

This role installs the Red Hat Advanced Cluster Security (RHACS) operator, creates Central, generates and creates the init-bundle.yaml, and creates the Secured Cluster.  

Clone this repo and run the rhacs-role.yaml play to execute the role.  Ensure you have installed Ansible with the control node requirements outlined in https://docs.ansible.com/ansible-core/2.13/installation_guide/intro_installation.html#control-node-requirements

ex. ansible-playbook rhacs-operator/rhacs-role.yaml -vv

This role includes the following playbooks in the tasks directory.  Make sure to specify which playbooks to run in the defaults/main.yml:

1. rhacs-operator.yml to install the RHACS operator and create the rhacs-operator and stackrox namespaces.
2. central.yml to create Central which includes 3 deployments (central, scanner, and scanner-db).
3. secured_cluster.yml which starts by ensuring the Central deployments are up and then installs the roxctl tool, authenticates it to central with the ROX_API_TOKEN, generates the init-bundle, creates the resulting sensor-tls, collector-tls, and admission-control-tls secrets which are needed for a cluster to join Central as a Secured Cluster.  Finally, the Secured Cluster object is created which starts the deployments for sensor, collector, and admission controller.


## Watch out for these potential issues

1) Ensure you have the correct version of Python.  If you need to update Python3 to a newer version, follow the steps in https://phoenixnap.com/kb/upgrade-python

2) If the playbook ends in an error No module named "Kubernetes", install the module with:

   python3 -m pip install kubernetes

   If it's missing a different module such as openshift, install the openshift module the same way.

3) Update /etc/ansible/ansible.cfg with:

   ansible_python_interpreter=/usr/bin/python3
