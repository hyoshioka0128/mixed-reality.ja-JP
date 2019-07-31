---
title: MR Learning SpeechSDK モジュール-音声認識と議事録
description: このコースでは、mixed reality アプリケーション内で Azure Speech SDK を実装する方法について説明します。
author: jessemcculloch
ms.author: jemccull
ms.date: 02/26/2019
ms.topic: article
keywords: Mixed Reality、Unity、チュートリアル、Hololens
ms.openlocfilehash: fc65dccfcbc181af0c0b321374c721797e120e5d
ms.sourcegitcommit: c7c7e3c836373b65e319609b4e8389dea6b081de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/24/2019
ms.locfileid: "68460337"
---
# <a name="speech-sdk-learning-module---rocket-launcher-control-using-speech-commands"></a>Speech SDK Learning モジュール-音声コマンドを使用したロケットランチャーコントロール

このレッスンでは、Azure Speech Service のインテント機能を使用して、音声の意図を理解します。 検出された音声のインテントを音声コマンドとして使用して、ロケットランチャーを制御します。 このレッスンでは、ロケットランチャーアセットをインポートします。検出されたインテント音声コマンドを事前に定義されたコントロールボタンにインターフェイスして、ロケットを制御します。 

## <a name="objectives"></a>目的

- 音声の目的を音声コマンドとして接続する方法について説明します。
- 音声合成の音声コマンドを、ロケットコントロール入力コマンドとして使用する方法について説明します。

## <a name="instructions"></a>手順
1. このチュートリアルでは、"BaseModule" 資産を使用して、ロケットランチャーと音声コマンドを統合します。 そのためには、プロジェクトにアセットをインポートする必要があります。 このリンクを使用して、"ロケットランチャー" 資産をダウンロードできます (リンクを添付します)。 

2. アセットをインポートするには、[> アセット]、[パッケージのインポート]、[カスタム > パッケージ >] の順に選択し、ダウンロードしたファイルに移動して、[インポート] をクリックします。

![module4chapter5step1](images/module4chapter5step1.PNG)

3. "ロケットランチャー" 資産をインポートした後、"ロケットランチャー" フォルダー内を移動します。 > Prefabs-> [ロケット Launcher_Complete] を選択し、既存のシーン階層にドラッグアンドドロップします。

![module4chapter5step2](images/module4chapter5step2.PNG)

4. ここで、前のレッスンで作業した "ロケットランチャー" を LUIS プロジェクトと統合する必要があります (lesson4 のリンク)。 そのためには、階層の "ロケット Launcher_Complete" prefab を展開し、"LaunchRoundButton"、"ResetRoundButton"、および "Placement ヒント" の各ボタンを見つけます。

![module4chapter5step3](images/module4chapter5step3.PNG)

5. "LaunchRoundButton" を選択し、[インスペクター] パネルで "対話型" に移動して、"OnClick ()" イベントの下にある "LunarModule" prefab をドラッグアンドドロップします。 次に、[関数] ドロップダウンを選択し、"LunarModule" を選択します。次に、"StartThruster ()" 関数に移動して選択します。

![module4chapter5step 3.0](images/module4chapter5step3.0.PNG)

![module4chapter5step3a](images/module4chapter5step3a.PNG)

6. "ResetRoundButton" を選択し、[インスペクター] パネルで "対話型" に移動して、"OnClick ()" イベントの下にある "LunarModule" prefab をドラッグアンドドロップします。 次に、[関数] ドロップダウンを選択し、"LunarModule" を選択します。次に、"resetModule ()" 関数に移動して選択します。

![module4chapter5step3b](images/module4chapter5step3b.PNG)

7. [配置ヒント] を選択し、[インスペクター] パネルで "対話型" に移動して、"OnClick ()" イベントの下にある "LunarModule" prefab をドラッグアンドドロップします。 次に、[関数] ドロップダウンを選択し、"LunarModule" を選択します。次に、"TogglePlacementHints" 関数に移動し、[ToggleGameOBjects ()] を選択します。

![module4chapter5step3c](images/module4chapter5step3c.PNG)

8.  次に、[Lunarcom_Completed prefab in Hierarchy] を選択し、[LaunchRoundButton]、[ResetRoundButton]、および [Placement ヒント] の各ボタンをそれぞれの場所にドラッグアンドドロップします。

![module4chapter5step4](images/module4chapter5step4.PNG)

9. Unity エディターの [play] ボタンをクリックし、[ロケット] ボタンをクリックして、[utter] ボタンをクリックします。「プッシュボタンを押す」、「配置ヒントを表示する」、「rest ボタンを押す」、またはロケットランチャーに対する起動、リセット、またはヒントの要求に関連するその他の語句を選択します。

![module4chapter5step5a](images/module4chapter5step5a.PNG)

![module4chapter5step5b](images/module4chapter5step5b.PNG)

![module4chapter5step5c](images/module4chapter5step5c.PNG)

## <a name="congratulations"></a>結論

このレッスンでは、AI による音声認識コマンドを音声コマンドに接続して、入力方式として使用する方法を学習しました。 これで、プログラムは、スピーカーの意図を音声コマンドとして使用して、ロケットランチャーの入力を提供できるようになりました。
