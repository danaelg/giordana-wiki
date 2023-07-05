---
title: Playbooks Ansible
description: 
published: true
date: 2023-07-05T07:39:26.743Z
tags: ansible, work-in-progress, ansible-playbook
editor: markdown
dateCreated: 2023-07-03T17:34:59.507Z
---

# Introduction
Les playbooks sont des fichiers texte YAML qui servent de plan pour exécuter des tâches sur une liste d'hôte définie par les fichiers d'[inventaire](/ansible/inventory). Comme pour des script *bash* ou *powershell* classique, quasiment toutes les actions peuvent être décrite dans des playbooks. La différence réside dans l'approche descriptive qui s'oppose à une approche plus itérative que l'on retrouve dans les scripts.

# Syntaxe
Voici un exemple de playbooks
```kroki
excalidraw
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://excalidraw.com",
  "elements": [
    {
      "id": "n55O3tBfoIKXiZeZKm233",
      "type": "text",
      "x": 641,
      "y": 201,
      "width": 609.2666625976562,
      "height": 528,
      "angle": 0,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "fillStyle": "hachure",
      "strokeWidth": 1,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "seed": 686948453,
      "version": 111,
      "versionNonce": 1767409541,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1688542356318,
      "link": null,
      "locked": false,
      "text": "---\n- name: Update web servers\n  hosts: webservers\n  tasks:\n  - name: Ensure apache is at the latest version\n    ansible.builtin.yum:\n      name: httpd\n      state: latest\n  - name: Write the apache config file\n    ansible.builtin.template:\n      src: /srv/httpd.j2\n      dest: /etc/httpd.conf\n\n- name: Update db servers\n  hosts: databases\n  remote_user: root\n  tasks:\n  - name: Ensure postgresql is at the latest version\n    ansible.builtin.yum:\n      name: postgresql\n      state: latest\n",
      "fontSize": 20,
      "fontFamily": 3,
      "textAlign": "left",
      "verticalAlign": "top",
      "baseline": 523,
      "containerId": null,
      "originalText": "---\n- name: Update web servers\n  hosts: webservers\n  tasks:\n  - name: Ensure apache is at the latest version\n    ansible.builtin.yum:\n      name: httpd\n      state: latest\n  - name: Write the apache config file\n    ansible.builtin.template:\n      src: /srv/httpd.j2\n      dest: /etc/httpd.conf\n\n- name: Update db servers\n  hosts: databases\n  remote_user: root\n  tasks:\n  - name: Ensure postgresql is at the latest version\n    ansible.builtin.yum:\n      name: postgresql\n      state: latest\n",
      "lineHeight": 1.2,
      "isFrameName": false
    }
  ],
  "appState": {
    "gridSize": null,
    "viewBackgroundColor": "#ffffff"
  },
  "files": {}
}
```

# Commandes
## Exécuter un playbook
```bash
ansible-playbook [-i INVENTORY] PLAYBOOK
```

# Références
- [Using Ansible playbooks - Ansible Documentation](https://docs.ansible.com/ansible/latest/playbook_guide/index.html)
{.links-list}