install:
	@rm -rf ./collections/ansible_collections/anixel/openshift_install ./collections/ansible_collections/anixel/libvirt
	ansible-galaxy collection install -r requirements.yml --collections-path ./collections

run:
	ansible-playbook anixel.openshift_install.run --tags setup

destroy:
	ansible-playbook anixel.openshift_install.run --tags destroy

.PHONY: install run destroy
