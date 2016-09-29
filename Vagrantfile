Vagrant.configure('2') do |config|
  config.vm.box = 'obnox/fedora24-64-lxc'
  config.vm.hostname = 'ansible-role-gitconfig-fedora-24'

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.sudo = true
  end

  config.vm.provision 'shell' do |s|
    s.keep_color = true
    s.inline = <<SCRIPT
grep -q Trash /home/vagrant/.gitignore || exit 1
grep -q .bak /home/vagrant/.gitignore || exit 1
grep -q /home/vagrant/.gitignore /home/vagrant/.gitconfig || exit 1
grep -q "email = " /home/vagrant/.gitconfig || exit 1
grep -q "name = " /home/vagrant/.gitconfig || exit 1
grep -q "default = simple" /home/vagrant/.gitconfig || exit 1
grep -q "prune = true" /home/vagrant/.gitconfig || exit 1

echo "127.0.0.1" > /etc/ansible/hosts
echo "localhost" > /etc/ansible/inventory

cd /vagrant/
ansible-playbook playbook.yml --connection local 2>&1 | tee /tmp/ansible.log

# Remove colors from log file
sed -i -r "s/\\x1B\\[([0-9]{1,2}(;[0-9]{1,2})?)?[m|K]//g" /tmp/ansible.log

# Test for errors
test -n "$(grep -L 'ERROR' /tmp/ansible.log)" \
    && { echo "Errors test: pass"; } \
    || { echo "Errors test: fail" && exit 1; }

# Test for warnings
test -n "$(grep -L 'WARNING' /tmp/ansible.log)" \
    && { echo "Warnings test: pass"; } \
    || { echo "Warnings test: fail" && exit 1; }

# Test for idempotence
grep -q "changed=0.*failed=0" /tmp/ansible.log \
    && { echo "Idempotence test: pass"; } \
    || { echo "Idempotence test: fail" && exit 1; }
SCRIPT
  end
end
