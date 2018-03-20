fortios_config

## 環境
python2,3混在環境
pyenv利用

## ナレッジ
* ansible_python_interpreterを指定していないとssh先で/usr/bin/pythonを利用しようとするので、python2系でpyFGがうまく動かないとエラーで落ちる。

* 存在しないvdomを指定してしまうと変にvdomが作成されるので注意。
---
## サンプル
```yaml
- hosts: all
  connection: local
  gather_facts: no
  vars:
    ansible_python_interpreter: pythonのパス
    now_date: "{{ lookup('pipe','date +%Y%m%d%H%M') }}"
  tasks:
   - name: test
     fortios_config:
       host: "192.168.x.x"
       username: 'xxxx'
       password: 'xxxx'
       backup: yes
       backup_path: '/var/backup/'
       backup_filename: "sample_{{ item }}_{{ now_date }}"
       vdom: "{{ item }}"
     with_items:
      - 'global'
      - 'vdom1'
      - 'vdom2'
      - 'vdom3'
```
