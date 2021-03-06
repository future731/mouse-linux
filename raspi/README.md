差別やジェンダーへの配慮が必要であることは理解した上で，
技術的観点から記述時点での通称でドキュメントを記述することとする

# raspi zero wh2台
ホスト名は
サーバー: raspberrypi0, PCと接続
クライアント: raspberrypi1, マウスと接続


# ホスト側作業
```bash
git clone https://github.com/future731/mouse-linux
```
2枚のSDカードにOSをインストール
~/.ssh/configにssh_configの中身を登録
それぞれraspiwh0，raspiwh1として登録してある
/mediaにマウントされたやつの/boot/に
```bash
sudo touch /boot/ssh
sudo vi /rootfs/etc/wpa_supplicant/wpa_suplicant.conf # wifi設定
```
N=0,1として
```bash
sudo vi /etc/hosts
sudo vi /etc/hostname
```
してraspberrypiNとする
起動して，
```bash
ssh raspiwhN
passwd # 初期パスはraspberryなのでpasswdする
```

# サーバー，クライアント共通設定
```bash
git clone https://github.com/future731/mouse-linux
cd mouse-linux/raspi
mkdir build
cd build
cmake ..
make
sudo /home/pi/mouse-linux/raspi/setup_ssh.sh
sudo /home/pi/mouse-linux/raspi/setup_deps.sh
sudo /home/pi/mouse-linux/raspi/setup_serial.sh
```

# サーバー設定
raspberrypi0の方
サーバー側
```bash
sudo /home/pi/mouse-linux/raspi/server/setup_otg.sh
sudo /home/pi/mouse-linux/raspi/server/rc.local.server /etc/rc.local
```



# クライアント設定
```bash
# 99-mouse.rulesをマウスに合わせて編集
sudo cp /home/pi/mouse-linux/raspi/client/99-mouse.rules /etc/udev/rules.d
sudo udevadm control --reload
sudo /home/pi/mouse-linux/raspi/client/rc.local.client /etc/rc.local
```

# UART接続 (Wi-Fi不使用の場合)
サーバー側UART_TX <-> クライアント側UART_RX
サーバー側UART_RX <-> クライアント側UART_TX
のピンヘッダをメスメスコネクタで物理接続

# サーバー側明示的起動
sudo pkill server
sudo /home/pi/mouse-linux/raspi/build/server
# クライアント側明示的起動
sudo pkill client
sudo /home/pi/mouse-linux/raspi/build/client
