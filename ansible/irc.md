## 環境
ansible 2.4

## ナレッジ
* 複数のホスト分実行させるとircサーバに接続できずに終わるのでserialで絞っている(local_actionとかにせず各サーバでやってしまえばその辺は気にしないかも?)
* チャンネル全体だけでなく、特定の人物に通知したい場合はnick_toを利用。

## サンプル
### ホスト毎にパスの存在をチェックして、存在するなら緑でTrue表示、存在しないなら赤でFalse表示
```text
16:16 *system join #channel (system@ansible)
16:16 (system) host1: stat_status is True
16:16 *system part (system)
16:16 *system join #channel (system@ansible)
16:16 (system) host2: stat_status is False
16:16 *system part (system)
```
### プレイブック
```yaml
- hosts: all
  tasks:
   - name: task1
     stat:
       path: 'パス'
     register: result
     ignore_errors: true

- hosts: all
  gather_facts: no
  serial: 1
  tasks:
   - name: test
     local_action:
       module: irc
       server: 192.168.x.x
       channel: '#channel'
       passwd: 'irc_pass'
       msg: "{{ ansible_hostname }}: stat_status is {{ result.stat.exists }}"
       color: "{{ item.color }}"
       nick: system
#       nick_to: yun
     when:  item.when
     with_items:
      - { color: green, when: "{{result.stat.exists == true | bool}}" }
      - { color: red, when: "{{result.stat.exists == false | bool}}" }
```
