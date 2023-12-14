# Eclipse
```console
# İşlemleri herhangi bir sunucumuzda yapalım muhim değil.
# Önce düşük bellekli sunucular için 12Gb Bellek için swap alanı oluşturalım. 8 Gb la denedim ama hatalar aldım.
sudo swapoff -a
sudo fallocate -l 12288 /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

# bzip kütüpanelerini yükleyelim, eksik olursa dosayları açamazınız
sudo apt-get install bzip2

## Contrat Deploy:


# 1'i seçelim:
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source "$HOME/.cargo/env"

# solana cli kurulumu
sh -c "$(curl -sSfL https://release.solana.com/stable/install)"
PATH="/root/.local/share/solana/install/active_release/bin:$PATH"
solana config set --url https://staging-rpc.dev.eclipsenetwork.xyz

# key oluşacak, şifre belirleyip mnemonicleri ve cüzdan adresini kaydediyoruz
solana-keygen new

# cüzdana token alıyoruz
solana airdrop 10

# bu komut çalışması için 8 ram lazım - github swap space repom ile çözersiniz.
solana-test-validator
# komut çalışınca ctrl c ile durdurabilirsiniz.

# nodejs kurulumu
sudo apt-get install -y ca-certificates curl gnupg
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg

NODE_MAJOR=20
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list
sudo apt-get update
sudo apt-get install nodejs -y

# kontrat deploy 
git clone https://github.com/solana-labs/example-helloworld
cd example-helloworld
npm install

# cc hatası alırsanız aşağıdaki komutu uygulayın;
sudo apt install build-essential

# burda biraz bekletecek rusttan dolayı
npm run build:program-rust
# program id not edin
solana program deploy dist/program/helloworld.so
npm run start
# success çıktısı verecek en sonda.

```

## Bridge işlemi:

```console
sudo apt update -y && sudo apt upgrade -y
sudo apt install git

git clone https://github.com/Eclipse-Laboratories-Inc/testnet-deposit.git
cd testnet-deposit

yarn install
yarn add ethers


# Yukarıdaki son iki komutta hata alanlar aşağıdaki kodlarla devam etsinler;
sudo apt remove cmdtest 
sudo apt remove yarn 
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add - 
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list 
sudo apt-get update 
sudo apt-get install yarn -y


# Altta ki komutu düzenleyelim (tırnakları kaldırın). Sadece <solanaAdresi> ve <MetamaskPrivateKeyi> kısımları değiştiriyorsunuz.  
node deposit.js <solanaAdresi> 0x7C9e161ebe55000a3220F972058Fb83273653a6e 500000 100 <MetamaskPrivateKeyi> https://rpc.sepolia.org
# yukarıda oluşturduğumuz solana adresini yeni bir phantom cuzdana kelimeleri import ederke oluşturun. Cüzdandan Solana adresini kopyalayıp <solanaAdresi> yerine yapıştırn.
```

> başarılıysa success ve tx çıktısı verecek

> https://explorer.dev.eclipsenetwork.xyz/?cluster=testnet burdan solana cüzdanı kontrol edip eth var mı yok mu bakıyoruz. varsa ok. Yoksa sepoliafaucet.com dan Metamamask adresinize ETH gönderin. Aksi halde INSUFFICIENT_FUNDS hatası alırsınız.

> Formu doldurun: https://docs.google.com/forms/d/e/1FAIpQLSfJQCFBKHpiy2HVw9lTjCj7k0BqNKnP6G1cd0YdKhaPLWD-AA/viewform
