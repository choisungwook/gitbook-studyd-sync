# Ansible playbook script



## Systemctl

{% hint style="info" %}
reference: [https://docs.ansible.com/ansible/latest/collections/ansible/builtin/systemd\_module.html](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/systemd\_module.html)
{% endhint %}

### Start service

```yaml
- name: Start kubelet
  systemd:
    name: kubelet
    state: started
```

### Stop Service

```yaml
- name: Stop kubelet
  systemd:
    name: kubelet
    state: stop
```

### Status service

```yaml
- name: Check status of systemd service
  systemd:
    name: kubelet
  register: service_status

- name: Print service status
  debug:
    var: service_status.status.ActiveState
```

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

## File

### Copy file

```yaml
- name: Copy yaml
  copy:
    src: example.yaml
    dest: /tmp/example.yaml
```

### Delete file

```yaml
- name: Delete directory
  file:
    path: /tmp/example
    state: absent
    force: yes
```

