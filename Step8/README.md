# Step8:DGX Sparkの2台運用
DGX Sparkを2台使用可能にするための環境構築手順をまとめます。


## 8.1:ユーザーとパスワードの設定
追加する2台目のDGX Sparkに、1台目と同じ「ユーザー名」「パスワード」のアカウントを設定してください。

## 8.2:ネットワーク構築
QSFPケーブルで接続した2台のDGX Sparkでネットワークを構築します。
まずipアドレスの固定をします。
node1とnode2で、それぞれ以下を実行してください。
```
# node1

```
```
# node2
sudo tee /etc/netplan/40-cx7.yaml > /dev/null <<EOF
network:
  version: 2
  ethernets:
    enp1s0f0np0:
      addresses:
        - 192.168.100.11/24
      dhcp4: no
EOF
sudo chmod 600 /etc/netplan/40-cx7.yaml
sudo netplan apply
ip addr show enp1s0f0np0   # inet 192.168.100.11/24 が付くことを確認
```

