install:
	echo "Removing existing anixel collections..."
	@rm -rf ./collections/ansible_collections/anixel/libvirt_examples
	ansible-galaxy collection install -r requirements.yml --collections-path ./collections


run:
	ansible-playbook anixel.libvirt_examples.run

# Runs role libvirt_emtpy_host (undefine all domains + delete their volumes; remove all libvirt networks).
destroy:
	ansible-playbook anixel.libvirt_examples.run --tags destroy

.PHONY: install run destroy
