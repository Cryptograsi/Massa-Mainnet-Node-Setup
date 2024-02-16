# Massa-Mainnet-Node-Setup

Massa geçtiğimiz ay mainnete geçti. Nod kurabilmeniz için MAS token edinmeniz gerekiyor. Şu an hala public satışı devam etmekte. Nod için en az 1 adet roll almalısınız. 1 roll 100 MAS tokene karşılık geliyor / Massa moved to mainnet last month. In order to establish a node, you need to obtain MAS tokens. It is currently still on public sale. You must buy at least 1 roll for the node. 1 roll corresponds to 100 MAS tokens.

## Sunucumuzu Güncelleyelim / Update Our Server
```
sudo apt-get update && sudo apt-get upgrade
```

# Gerekli Portları Açalım / Open the Required Ports
```
sudo apt install ufw -y
sudo ufw allow 22
sudo ufw allow ssh
sudo ufw allow 31244/tcp
sudo ufw allow 31245/tcp 
sudo ufw enable
```

## Screen Açalım / Open Screen
```
screen -S massanode
```

## Rust Kuralım / Install Rust
Not: Kodları tek tek girelim, tek seferde hepsini kopyalayıp yapıştırmayalım! / Let's enter the codes one by one, not copy and paste them all at once!
```
sudo apt install pkg-config curl git build-essential libssl-dev libclang-dev cmake
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
rustc --version
rustup toolchain install 1.74.1
rustup default 1.74.1
```

### Versiyon Kontrolü Yapalım / Check Rust Version
Not: Çıktımız 1.74.1 olmalı! / Our output should be 1.74.1!
```
rustc --version
```

## Massa Dosyalarını Sunucumuza İndirelim / Download Massa Files to Our Server
```
git clone https://github.com/massalabs/massa.git
```

### Config Dosyasının İçine Girip IP Adresimizi Ekleyelim / Go into the Config File and Add Our IP Address
Not: Burada routable_ip yazan kısma .kendi sunucu IP'nizi yazın! / Write your own server IP in the routable_ip section!
```
nano massa/massa-node/config/config.toml
```

![Ekran görüntüsü 2024-02-16 192507](https://github.com/Cryptograsi/Massa-Mainnet-Node-Setup/assets/101165594/17d55660-c7d2-4394-ac10-e2ab082e9d00)

IP adresimizi yazdıktan sonra CTRL X'e bastıktan sonra Y tuşuna basıp ENTER yapıyoruz / After writing our IP address, press CTRL X, then press Y and ENTER

## Artık Nodumuzu Çalıştırabiliriz / Now We Can Run Our Node

Massa-node dizinine gidelim: / Let's go to the massa-node directory:
```
cd massa/massa-node/
```

Kodu girerek nodumuzu başlatalım. Kodu çalıştırmadan önce <password> yazan kısmı silip kendi belirlediğiniz bir şifreyi yazınız ( < ve > işaretleri de silinecek) / Start our node by entering the code. Before running the code, delete the part that says <password> and type a password of your own choosing (< and > signs will also be deleted)
```
RUST_BACKTRACE=full cargo run --release -- -p <PASSWORD> |& tee logs.txt
```

CTRL A+P basıp yeni bir screene geçelim (gerektiğinde screenler arasında geçiş yapmak için yine CTRL A+P kullanacağız) / Let's press CTRL A+P and move to a new screen (we will use CTRL A+P again to switch between screens when necessary)

Massa-client dizinine gidelim: / Let's go to the massa-client directory:
```
cd massa/massa-client/
```
```
cargo run --release -- -p <password>
```
Burada <password> yazan yeri silip yerine az önce belirlediğiniz şifreyi yazınız ( < ve > işaretleri de silinecek) / Here, delete the area where <password> is written and replace it with the password you just determined (< and > signs will also be deleted)
Bu kod çalıştıktan sonra karşımıza client ekranı çıkacak, bu ekranda roll alıp-satma, cüzdanlar arası transfer, yeni cüzdan oluşturma vb birçok işlem yapabiliyoruz. Üstelik bu işlemlerin kodları bu ekranda hazır bir şekilde bizi bekliyor olacak. Yapmamız gereken tek şey kodları kendimize uyarlamak / After this code runs, we will see the client screen. On this screen, we can perform many transactions such as buying and selling rolls, transferring between wallets, creating a new wallet, etc. Moreover, the codes for these transactions will be ready and waiting for us on this screen. All we have to do is adapt the codes to ourselves



Eğer eski bir cüzdanı kullanacaksanız wallet_add_secret_keys SecretKey1 kodunu yazıp SecretKey1 yazan kısma cüzdan keyinizi yazmanız ve entera basmanız gerekir. Yeni bir cüzdan oluşturmak içinse wallet_generate_secret_key komutu yeterli olacaktır / If you are going to use an old wallet, you need to write the wallet_add_secret_keys SecretKey1 code, write your wallet key in the section that says SecretKey1 and press enter. To create a new wallet, the wallet_generate_secret_key command will be sufficient.

Cüzdan oluşturduktan veya eski cüzdanı import ettikten sonra aşağıdaki kodla bakiyemizi görebiliriz /  After creating a wallet or importing the old wallet, we can see our balance with the code below
```
wallet_info
```
![Ekran görüntüsü 2024-02-16 194744](https://github.com/Cryptograsi/Massa-Mainnet-Node-Setup/assets/101165594/8ea81d52-7fe6-43ab-b48c-32c85305dc5a)

Staking işlemi yapabilmek için roll satın almanız gerekiyor, bunun için aşağıdaki kodu kullanalım / To perform staking, you need to purchase rolls, let's use the code below for this
```
buy_rolls cüzdan-adresiniz roll-adedi 0
```
Not: Burada cüzdan-adresi yazan yere kendi cüzdan adresinizi, roll-adedi yazan yere ise kaç roll alacaksanız o miktarı yazmanız gerekiyor. Sondaki 0 rakamı gas ücretine karşılık gelir / Here, you need to write your own wallet address where it says cüzdan-adresi, and how many rolls you will buy where it says roll-adedi. The trailing 0 corresponds to the gas fee

![Ekran görüntüsü 2024-02-16 194654](https://github.com/Cryptograsi/Massa-Mainnet-Node-Setup/assets/101165594/977d664b-453b-42b6-9178-ce711d3ad207)

Roll satın aldıktan sonra staking yapabiliriz. Bunun için aşağıdaki kodu kullanacağız / We can stake after purchasing rolls. We will use the following code for this
```
node_start_staking cüzdan-adresi
```
Not: cüzdan-adresi yazan yere yine kendi cüzdan adresimizi yazıyoruz / We write our own wallet address where it says cüzdan-adresi
![Ekran görüntüsü 2024-02-16 194744](https://github.com/Cryptograsi/Massa-Mainnet-Node-Setup/assets/101165594/afa566d0-e3db-4cbd-a9e4-4a1040ab2fcd)

Hepsi bu kadar. Nodumuz çalıştıkça stake gelirleriniz artacak ve bunları başka bir adrese veya borsaya gönderebileceksiniz. Listeleme olunca staking gelirlerini satıp düzenli bir gelir elde edebilirsiniz. Bol kazançlar / That is all. As your node works, your staking income will increase and you will be able to send them to another address or exchange. Once listed, you can sell the staking income and earn a regular income. Good luck

