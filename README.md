# Tutorial testnet SUI Petani Node

<p align="center">
  <img height="75" width="75" src="https://d1fdloi71mui9q.cloudfront.net/fh2P8CPLQDSDUd0LS2E8_45Mg5E2qiHd8K7yN">
</p>

<p style="font-size:14px" align="center">
<a href="https://linktr.ee/petaninode" target="_blank">Petani Node Web</a>
</p>

## Menginstall linux Depedensi
```
sudo apt-get update \
&& sudo apt-get install -y --no-install-recommends \
tzdata \
ca-certificates \
build-essential \
libssl-dev \
libclang-dev \
pkg-config \
openssl \
protobuf-compiler \
cmake
```
## Install Rust
```
sudo curl https://sh.rustup.rs -sSf | sh -s -- -y
source $HOME/.cargo/env
rustc --version
```
## Clone SUI Repositori
```
cd $HOME
git clone https://github.com/MystenLabs/sui.git
cd sui
git remote add upstream https://github.com/MystenLabs/sui
git fetch upstream
git checkout -B testnet --track upstream/testnet
```
## Membuat Direktori SUI db & Genesis
```
mkdir $HOME/.sui
```
## Mendownload File Genesis
```
wget -O $HOME/.sui/genesis.blob  https://github.com/MystenLabs/sui-genesis/raw/main/testnet/genesis.blob
```
## Membuat salinan fullnode.yaml & Memperbarui Path ke file db dan genesis di dalamnya.
```
cp $HOME/sui/crates/sui-config/data/fullnode-template.yaml $HOME/.sui/fullnode.yaml
sed -i.bak "s|db-path:.*|db-path: \"$HOME\/.sui\/db\"| ; s|genesis-file-location:.*|genesis-file-location: \"$HOME\/.sui\/genesis.blob\"| ; s|127.0.0.1|0.0.0.0|" $HOME/.sui/fullnode.yaml
```
## Menambahkan peers pada fullnode.yaml
```
sudo tee -a $HOME/.sui/fullnode.yaml  >/dev/null <<EOF

p2p-config:
  seed-peers:
   - address: "/ip4/65.109.32.171/udp/8084"
   - address: "/ip4/65.108.44.149/udp/8084"
   - address: "/ip4/95.214.54.28/udp/8080"
   - address: "/ip4/136.243.40.38/udp/8080"
   - address: "/ip4/84.46.255.11/udp/8084"
   - address: "/ip4/135.181.6.243/udp/8088"
   - address: "/ip4/89.163.132.44/udp/8080
   - address: "/ip4/68.183.120.86/udp/8080"
EOF
```
## Membuat SUI Binaries
```
cargo build --release --bin sui-node
mv ~/sui/target/release/sui-node /usr/local/bin/
sui-node -V
```
## Membuat Service file untuk SUI Node.
```
echo "[Unit]
Description=Sui Node
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=/usr/local/bin/sui-node --config-path $HOME/.sui/fullnode.yaml
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target" > $HOME/suid.service

mv $HOME/suid.service /etc/systemd/system/

sudo tee <<EOF >/dev/null /etc/systemd/journald.conf
Storage=persistent
EOF
```
## Menjalankan SUI Full Node
```
sudo systemctl restart systemd-journald
sudo systemctl daemon-reload
sudo systemctl enable suid
sudo systemctl restart suid
journalctl -u suid -f
```
DONEEEEEEE😁
Mau membuat Address?Ikuti langkah di bawah.
# Membuat address

##Buat SUI Ulang Terlebih dahulu
1.
```
cd $HOME/sui
```
2.
```
git fetch upstream
git checkout -B testnet --track upstream/testnet
cargo build  -p sui --release
```
3.
```
mv ~/sui/target/release/sui /usr/local/bin/
```
## Buat Wallet
1.
```
sui client active-address
```
2. KETIK Y LALU ENTER
3. TEKAN ENTER
4. KETIK NOMOR 0 LALU ENTER
5. ⚠️SIMPAN INFORMASI YANG MUNCUL⚠️
## Meminta Faucet
1.Pergi Ke Discord SUI pilih channel Testnet Faucet
2.Ketikkan
```
!faucet XXXXXXXXXXXXXXXXXXX
```
XXXXXX diganti dengan address kalian.
Contoh:
```
!faucet 0x2c6f2bf2426192e698d4f7fd3e0863e84a16001d
```

DONNEEE🙏
## Check Node kalian jalan atau tidak
Kunjungi Halaman
```
https://www.scale3labs.com/check/sui
```
Paste IP kalian lalu pilih Testnet lalu check







