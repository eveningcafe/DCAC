# Cai ansible

Cài đặt gói phụ thuộc (Nếu dùng node ansible tách khỏi node poller)

Đối với Debian/Ubuntu, cập nhật the package index:

sudo apt update

Cài đặt các gói Python build dependencies:

## For CentOS/RHEL

sudo dnf install python3-devel python3-pip libffi-devel gcc openssl-devel python3-libselinux

## For Debian/Ubuntu

sudo apt install python3-dev python3-pip libffi-dev gcc libssl-dev

Cài đặt Ansible:
```
# If you're running on Ubuntu<=14.04/Debian <= 9 - Python 2.x or Python 3.5, this command may save your day
pip3 install --upgrade 'pip<21' 'setuptools<51'
# Ansible >= 2.7.9
pip3 install 'ansible<3.0'
```
# Tối ưu Ansible
Cấu hình Ansible như sau để đạt hiệu quả sử dụng tốt nhất.

```
[defaults]
timeout = 50
host_key_checking = false
pipelining=True
forks=100
gathering = smart
fact_caching = jsonfile
# Ansible should be run as root
fact_caching_connection = /etc/ansible/facts.d
retry_files_enabled = False
fact_caching_timeout = 0

[ssh_connection]
ssh_args = -o ControlMaster=auto -o ControlPersist=900s
pipelining = True
```

# Tips sử dụng

inventory
```
[all:vars]
ansible_user=sf
ansible_become_method=su
#ansible_ssh_private_key_file=/root/.ssh/id_rsa

[all]
demo_10.1.18.195 ansible_ssh_host=10.1.18.195 ansible_user=sf ansible_password="123" ansible_become_pass="123"
behi_172.29.7.204 ansible_ssh_host=172.29.7.204 ansible_user=sf ansible_password="123" ansible_become_pass="123" ansible_ssh_common_args='-o ProxyCommand="ssh -W %h:%p -q sf@10.1.18.195"'
```

ssh-copyid to 195 you can excute to 204
