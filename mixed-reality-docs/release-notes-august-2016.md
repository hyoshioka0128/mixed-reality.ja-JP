---
title: リリースノート-2016 年8月
description: Windows 10 記念日リリースの HoloLens リリースノート (秋 2016)
author: mattzmsft
ms.author: mazeller
ms.date: 03/21/2018
ms.topic: article
keywords: HoloLens、リリースノート、os、プラットフォーム、機能、商用スイート
ms.openlocfilehash: dcac64524cd8d1b1f2b0a496c4dcd2ad2fc7b690
ms.sourcegitcommit: 6bc6757b9b273a63f260f1716c944603dfa51151
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2019
ms.locfileid: "73438096"
---
# <a name="release-notes---august-2016"></a>リリースノート-2016 年8月

HoloLens チームは、作業の優先順位を決定するために、Windows Insider プログラムの開発者からのフィードバックをリッスンしています。 フィードバックハブ、[開発者フォーラム](https://forums.hololens.com)、Twitter から、 [@HoloLens](https://twitter.com/hololens)を通じて[フィードバック](give-us-feedback.md)を引き続きお寄せください。 Windows 10 が記念日の更新を採用しているため、HoloLens チームは holographic エクスペリエンスにさらなる改善を提供しています。 この更新プログラムでは、Microsoft HoloLens 商用 Suite で利用できる、ビジネスによって要求された機能の主な修正、改善、導入に焦点を絞っています。

**最新リリース:** Windows Holographic 8 月2016更新プログラム (**10.0.14393.0**、Windows 10 記念日リリース)

>[!VIDEO https://www.youtube.com/embed/tNd0e2CiAkE]

[現在のリリースに更新](updating-hololens.md)するには、*設定*アプリを開き、[ *update & Security*] にアクセスして、[*更新プログラムの確認*] ボタンを選択します。

## <a name="new-features"></a>新機能

**プロセスデバッグにアタッチ**HoloLens では、プロセス間のデバッグがサポートされるようになりました。 Visual Studio 2015 Update 3 を使用して、HoloLens で実行中のアプリに接続し、[デバッグを開始](using-visual-studio.md#debugging-an-installed-or-running-app)することができます。 これは、Visual Studio プロジェクトから配置する必要がない場合に機能します。

**HoloLens エミュレーターを更新しました**また、更新されたバージョンの HoloLens エミュレーターもリリースしました。

**ゲームパッドのサポート**HoloLens で Bluetooth ゲームパッドを使用できるようになりました。 新しくリリースされた Xbox ワイヤレスコントローラーは、Bluetooth 機能を搭載しており、お気に入りのゲーム用ゲームやアプリの再生に使用できます。 Xbox ワイヤレスコントローラーを HoloLens に接続する前に、[コントローラーの更新プログラム](https://support.xbox.com/xbox-one/accessories/update-controller-for-stereo-headset-adapter)を適用する必要があります。 Xbox ワイヤレスコントローラーは、[XInput](https://msdn.microsoft.com/library/windows/desktop/hh405053(v=vs.85).aspx) と [Windows.Gaming.Input](https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx) API によってサポートされています。 Bluetooth コントローラーの追加モデルには、Windows の[ゲーム入力](https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx)API を使用してアクセスできます。

## <a name="improvements-and-fixes"></a>機能強化と修正

Microsoft は、Windows 10 の記念日更新プログラムとの同期を行っています。そのため、HoloLens 固有の修正プログラムに加えて、プラットフォームの信頼性とパフォーマンスを向上させるために、Windows update のすべての利点も受けています。 フィードバックは非常に重要であり、リリースの修正のために優先順位が付けられています。

次のエクスペリエンスが改善されました。
* ログインエクスペリエンス。
* workplace join。
* デバイスの電源状態遷移の電力効率。
* Mixed Reality キャプチャによる安定性。
* Bluetooth 接続の信頼性
* マルチアプリシナリオにおけるホログラムの永続性。

次の問題が修正されました。
* Visual Studio プロファイラーとグラフィックスデバッガーは接続に失敗します。
* 写真 & ドキュメントは、デバイスポータルのエクスプローラーに表示されません。
* アプリバーは、調整モードでカーソルが上に置かれたときにフラッシュできます。
* 調整モードでは、視線が上向きのドットカーソルが4方向のカーソルに変わります。
* "Cortana play music" さんは Groove を起動しません。
* 以前の更新の後、「ホーム」と言うと、ピンパネルが正しく表示されません。

## <a name="introducing-microsoft-hololens-commercial-suite"></a>Microsoft HoloLens 商用スイートの概要

Microsoft HoloLens 商用 Suite は、エンタープライズ展開の準備ができています。 初期のビジネスパートナーから、高度に要求された[商用機能](commercial-features.md)がいくつか追加されました。

Microsoft HoloLens 商用スイートを購入するには、ローカル Microsoft アカウントマネージャーにお問い合わせください。

### <a name="key-commercial-features"></a>主要商用機能 

* **キオスクモード。** HoloLens キオスクモードでは、実行するアプリを制限してデモやショーケースを有効にすることができます。<br>
  ![キオスクモードでは、HoloLens は選択したアプリに直接起動します。](images/201608-kioskmode-400px.png)
* **HoloLens 用のモバイルデバイス管理 (MDM)。** IT 部門は、Microsoft Intune などのソリューションを使用して、複数の HoloLens デバイスを同時に管理できます。 設定の管理、インストールするアプリの選択、組織のニーズに合わせたセキュリティ構成の設定を行うことができます。<br>
  HoloLens のモバイルデバイス管理 ![、複数のデバイスにわたるエンタープライズレベルのデバイス管理を提供します。](images/201608-enterprisemanagement-400px.png)
* **ビジネス向け Windows Update。** デバイスに対するオペレーティングシステムの更新と、長期的なサービスブランチのサポートを制御します。
* **データのセキュリティ。** HoloLens で BitLocker データ暗号化が有効になっているため、他の Windows デバイスと同じレベルのセキュリティ保護が提供されます。
* **職場へのアクセス。** 組織内のすべてのユーザーが、HoloLens の仮想プライベートネットワークを介して企業ネットワークにリモート接続できます。 HoloLens は、資格情報を必要とする Wi-fi ネットワークにアクセスすることもできます。
* **ビジネス向け Microsoft Store。** また、IT 部門は、特定の HoloLens 使用量について会社のアプリのみを含むエンタープライズプライベートストアをセットアップすることもできます。 エンタープライズユーザーの選択したグループにエンタープライズソフトウェアを安全に配布します。

### <a name="development-edition-vs-commercial-suite"></a>開発エディションと商用スイート

<table>
<tr>
<th>機能</th><th>開発エディション</th><th>商用スイート</th>
</tr><tr>
<td>デバイスの暗号化 (Bitlocker)</td><td></td><td style="text-align: center;">✔️</td>
</tr><tr>
<td>仮想プライベート ネットワーク (VPN)</td><td></td><td style="text-align: center;">✔️</td>
</tr><tr>
<td><a href="using-the-windows-device-portal.md#kiosk-mode">キオスクモード</a></td><td></td><td style="text-align: center;">✔️</td>
</tr><tr>
<th colspan="3" style="text-align: left;"> 管理と展開</th>
</tr><tr>
<td>モバイル デバイス管理 (MDM)</td><td style="text-align: center;"></td><td style="text-align: center;">✔️</td>
</tr><tr>
<td>登録解除をブロックする機能</td><td></td><td style="text-align: center;">✔️</td>
</tr><tr>
<td>証明書ベースの会社の Wi-fi アクセス</td><td></td><td style="text-align: center;">✔️</td>
</tr><tr>
<td>Microsoft Store (コンシューマー)</td><td style="text-align: center;">者</td><td style="text-align: center;">MDM を使用したフィルター処理</td>
</tr><tr>
<td><a href="https://technet.microsoft.com/itpro/windows/manage/working-with-line-of-business-apps">ビジネスストアポータル</a></td><td></td><td style="text-align: center;">✔️</td>
</tr><tr>
<th colspan="3" style="text-align: left;"> セキュリティと ID</th>
</tr><tr>
<td>Azure Active Directory を使用したログイン (AAD)</td><td style="text-align: center;">✔️</td><td style="text-align: center;">✔️</td>
</tr><tr>
<td>Microsoft アカウント (MSA) を使用してログインする</td><td style="text-align: center;">✔️</td><td style="text-align: center;">✔️</td>
</tr><tr>
<td>PIN のロック解除を含む次世代の資格情報</td><td style="text-align: center;">✔️</td><td style="text-align: center;">✔️</td>
</tr><tr>
<td><a href="https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/secure-boot-overview">セキュア ブート</a></td><td style="text-align: center;">✔️</td><td style="text-align: center;">✔️</td>
</tr><tr>
<th colspan="3" style="text-align: left;"> サービスとサポート</th>
</tr><tr>
<td>自動的にシステムを更新する</td><td style="text-align: center;">✔️</td><td style="text-align: center;">✔️</td>
</tr><tr>
<td><a href="https://technet.microsoft.com/itpro/windows/plan/windows-update-for-business">ビジネス向け Windows Update</a></td><td></td><td style="text-align: center;">✔️</td>
</tr><tr>
<td>長期的なサービスブランチ</td><td></td><td style="text-align: center;">✔️</td>
</tr>
</table>

## <a name="prior-release-notes"></a>以前のリリースノート
* [リリース ノート - 2016 年 5 月](release-notes-may-2016.md)
* [リリース ノート - 2016 年 3 月](release-notes-march-2016.md)

## <a name="see-also"></a>関連項目
* [HoloLens の既知の問題](hololens-known-issues.md)
* [業務用機能](commercial-features.md)
* [ツールのインストール](install-the-tools.md)
* [HoloLens のエミュレーターを使用する](using-the-hololens-emulator.md)
