install:
	echo "Removing existing anixel collections..."
	@rm -rf ./collections/ansible_collections/anixel/libvirt
	ansible-galaxy collection install -r requirements.yml --collections-path ./collections


run:
	ansible-playbook anixel.libvirt.run

test-iso:
	ansible-playbook anixel.libvirt.test_iso

forward-to-haproxy:
	ansible-playbook anixel.libvirt.forward_to_haproxy

forward-to-haproxy-open:
	ansible-playbook anixel.libvirt.forward_to_haproxy_open

forward-to-haproxy-open_destroy:
	ansible-playbook anixel.libvirt.forward_to_haproxy_open --tags destroy

# Runs role libvirt_emtpy_host (undefine all domains + delete their volumes; remove all libvirt networks).
destroy:
	ansible-playbook anixel.libvirt.run --tags destroy

.PHONY: install run test-iso forward-to-haproxy forward-to-haproxy-open forward-to-haproxy-open_destroy destroy

