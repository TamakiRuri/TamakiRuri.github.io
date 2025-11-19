---
title: Arch Linuxを使ってみた
published: 2022-09-01
description: 'Arch LinuxをメインOSとして使っていた時のメモ'
image: './arch_front.jpg'
tags: [Arch Linux, Linux, System]
category: 'Linux'
draft: false 
---

:::note[更新]
2025年11月19日
:::

Arch LinuxをメインOSとして使うプロジェクトです。

いまは勉強用ノーパソでArch Linux（手動インストール、Wayland、GNOME）を使っていますが、メインPCでは既にWindowsに戻しました。

勉強用ノートPCでは問題なく改変をしており、ぶいちゃも（PCモードで）出先でやったりしています。

ここでは、このプロジェクトで発見した細かいことを記録します。

## インストール

### Wifi

Wifiの環境では *archinstall* を使っても予めに [*iwd*](https://wiki.archlinux.jp/index.php/Iwd) を使ってセットアップする必要があります。

詳しくはドキュメントに従ってください。

### archinstall

Arch Linuxをインストールするには手動インストールとarchinstall二つの方法があります。archinstallは便利のかわりに設定が手動インストールと違うため、フォーラムで問題報告するときに明記するべきとのことです。公式Wikiで手動インストールする場合、Wikiだけでなく、ほかのチュートリアルと参照しながらインストールすると問題がある時に解決しやすくなります。ここでは、archinstallでインストールする方法を紹介します。

まず、同封の *archinstall* ではなく、先に *pacman -Syy* 、そして *pacman -S archinstall* をしてください。この *archinstall* のコマンドは同じですが同封のより若干使いやすいです。

設定を調整する時にいくつかの注意事項があります。

#### ドライブ

自作PC以外を使っている場合は、ドライブのすべてをフォーマットしないで、ファームウェアのパテーションを残してください。（将来Windowsに戻る時や中古パソコンとして売る時のため）

#### グラボのドライバー

*「Nvidia」* オーペンソースドライバー（DKMS）と公式(Proprietary)ドライバーの区別はここで詳しく説明しません。Arch Wikiにご参照ください。

AMD に関しては mesa が問題なく使えます。

#### サウンドドライバー

Pulseaudio と pipewire 両方とも使えます。もしインストールしたあとに音が出ない場合、追加パケージが必要です。pipewire の場合の一例は *pipewire-alsa* です。OBSをご利用の場合、そのままpulseaudioインストールしても動かないので、pipewire-pulseをご利用ください。（pulseaudio-bluetoothは予めアンインストールしてください。）

#### ブートローダー

最近 Grub のアップデートでシステムが起動できなくなる可能性がありますので、更新したあとは必ず
```
grub-install ...
grub-mkconfig -o /boot/grub/grub.cfg
```
をしてください。

### インストールしたあと

日本語を表示、入力するにはフォントとインプットメソッドが必要です、具体的にはドキュメントをご参照ください。

AUR を楽に使うために [*yay*](https://github.com/Jguer/yay) のような AUR ヘルパーを使った方が良いでしょう。

### VRChatを動かす

Steam で Proton Experimental を使うと、動画の再生が不安定なため、[Proton GE](https://github.com/GloriousEggroll/proton-ge-custom) がおすすめします。（ProtonGEでもよく再生ができない場合があるが、全くできないよりはましです）

ほかに、予め *gamemode* というパケージをインストールしてVRChatのパラメータに *gamemoderun %command%* を追加したら（私の環境では） FPS が 10% くらい高くなります。

ALVR を使う場合、Chromium （Chromeや、その他Chromeベースのブラウザも可）を予めインストールする必要があります。そうしなければ、ALVRが起動時に落ちます。

> Protonレイヤーを利用するため、写真とログの場所が通常と違います

> 写真
```
~/.steam/steam/steamapps/compatdata/438100/pfx/drive_c/users/steamuser/Pictures/VRChat
```
> ログ
```
~/.steam/steam/steamapps/compatdata/438100/pfx/drive_c/users/steamuser/AppData/LocalLow/VRChat
```
ちなみにもしほかのゲームの写真がなかったり、探したいファイルがあったりしたら、上のdrive_cからはWindowsみたいなフォルダ構造になるので、そこで探せばいいと思います。

#### 内蔵グラフィックスのVRAMについて

Linuxでは、Intel iGPUではUnified Memory、AMD iGPUがgttを利用できます。これらの技術によってiGPUがメモリをそのままアクセスでき（上限が異なる）、メモリが十分である限りではiGPUのVRAM不足の心配がほぼないと思います。

### Unityでアバター改変

> テスト環境: Wayland / Gnome

Unityでオブジェクトの選択ができない場合では、 *gconf* (AUR)というパッケージをインストールすることをおすすめします。（ほとんどの場合では、インストールしなくても正しく動作すると思いますが...稀に発生する問題です）

Unity 2019では*gconf* (AUR)が必須です。Waylandでは  *gconf* (AUR)があっても、Unity 2019が起動しないことが確認されています。

> Unity 2022.3.22f1では、シェーダーが問題なく動作しますが、2019では以下通りシェーダーの動作にいくつかの問題があったためおすすめしません。

liltoon 2.3 - 2.12 カラー選択ツールを使うとUnityが固まります。 \
liltoon 3: シェーダーキーワードの数が超えているためアバターが表示されません(51/32)。 \
Sunao 1.6.1 テクスチャエラーになります。

## 最後に

Unity 2019のごろ、メインPCでは二回くらいArch Linuxをインストールしましたが、アバター改変ができず断念しました。いまはその後購入したノートPCにArch Linuxを入れて改変をしたりしています。

もしこのポストが参考になったら幸いです。