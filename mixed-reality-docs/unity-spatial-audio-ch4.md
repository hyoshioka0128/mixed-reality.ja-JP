---
title: 空間オーディオチュートリアル-4. 実行時の空間オーディオの有効化と無効化
description: ボタンを使用して、実行時に spatialization のオーディオを有効または無効にします。
author: kegodin
ms.author: kegodin
ms.date: 12/01/2019
ms.topic: article
keywords: mixed reality、unity、チュートリアル、hololens2、空間オーディオ
ms.openlocfilehash: 3fc9b56dc58d4c19f229d92f4562f04cbca0e7a6
ms.sourcegitcommit: 8bf7f315ba17726c61fb2fa5a079b1b7fb0dd73f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/17/2019
ms.locfileid: "75182869"
---
# <a name="enabling-and-disabling-spatialization-at-run-time"></a>実行時の spatialization の有効化と無効化

## <a name="objectives"></a>目標
この4番目の章では、次のことについて説明します。
* Game オブジェクトの spatialization を制御する新しいスクリプトを追加します
* ボタンの操作から spatialization コントロールスクリプトを駆動する

## <a name="add-spatialization-control-script"></a>Spatialization コントロールスクリプトの追加
**プロジェクト**ペイン内を右クリックし、[ C# **作成-> C#スクリプト**] を選択して新しいスクリプトを作成します。 スクリプトに "SpatializeOnOff" という名前を指定します。

![スクリプトの作成](images/spatial-audio/create-script.png)

**プロジェクト**ウィンドウでスクリプトをダブルクリックして、Visual Studio で開きます。 既定のスクリプトの内容を次の内容に置き換えます。

> [!NOTE]
> スクリプトのいくつかの行がコメントアウトされています。これらの行は、 [5 章](unity-spatial-audio-ch5.md)でコメント解除されます。

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Audio;

[RequireComponent(typeof(AudioSource))]
public class SpatializeOnOff : MonoBehaviour
{
    public GameObject ButtonTextObject;
    //public AudioMixerGroup RoomEffectGroup;
    //public AudioMixerGroup MasterGroup;

    private AudioSource m_SourceObject;
    private bool m_IsSpatialized;
    private TMPro.TextMeshPro m_TextMeshPro;

    public void Start()
    {
        m_SourceObject = gameObject.GetComponent<AudioSource>();
        m_TextMeshPro = ButtonTextObject.GetComponent<TMPro.TextMeshPro>();
        SetSpatialized();
    }

    public void SwapSpatialization()
    {
        if (m_IsSpatialized)
        {
            SetStereo();
        }
        else
        {
            SetSpatialized();
        }
    }

    private void SetSpatialized()
    {
        m_IsSpatialized = true;
        m_SourceObject.spatialBlend = 1;
        m_TextMeshPro.SetText("Set Stereo");
        //m_SourceObject.outputAudioMixerGroup = RoomEffectGroup;
    }

    private void SetStereo()
    {
        m_IsSpatialized = false;
        m_SourceObject.spatialBlend = 0;
        m_TextMeshPro.SetText("Set Spatialized");
        //m_SourceObject.outputAudioMixerGroup = MasterGroup;
    }

}
```

> [!NOTE]
> Spatialization を有効または無効にするために、スクリプトは**spatialBlend**プロパティを調整するだけで、 **spatialization**プロパティを有効のままにします。 このモードでは、Unity でも**ボリューム**曲線が適用されます。 そうしないと、ユーザーがソースから遠くに spatialization を無効にした場合に、音量が急激に増加していることが聞こえます。 <br> <br>
> Spatialization を完全に無効にする場合は、 **SourceObject**変数の**spatialization** boolean プロパティも調整するようにスクリプトを変更します。

## <a name="attach-your-script-and-drive-it-from-the-button"></a>スクリプトをアタッチして、ボタンからドライブを作成します
**クワッド**の **[インスペクター]** ウィンドウで、 **[コンポーネントの追加]** をクリックし、 **Spatialize On Off**スクリプトを追加します。

![スクリプトをクワッドに追加する](images/spatial-audio/add-script-to-quad.png)

**クワッド**の**Spatialize on Off**コンポーネントで、次のようにします。
1. **階層**内で**PressableButtonHoloLens2-> iconandtext-> TextMeshPro**サブオブジェクトを検索する
2. **TextMeshPro** Suboject を**Spatialize On Off**コンポーネントの**buttontextobject**フィールドにドラッグします。

これらの変更が完了すると、 **Quad**の**Spatialize**コンポーネントは次のようになります。

![Spatialize on off basic](images/spatial-audio/spatialize-on-off-basic.png)

ボタンが離されたときに**Spatialize On Off**スクリプトを呼び出すようにボタンを設定するには、 **PressableButtonHoloLens2**オブジェクトの **[インスペクター]** ペインを開き、**対話型**コンポーネントを見つけます。
1. **Events**サブセクションの**OnClick ()** 領域を検索します。
2. **階層**からターゲットオブジェクトスロットに**クワッド**をドラッグします。
3. アクション ボックスの一覧から  **SpatializeOnOff** を選択します。

これらの変更が完了すると、**対話型**コンポーネントは次のようになります。

![ボタンの操作の設定](images/spatial-audio/button-action-settings.png)

## <a name="next-steps"></a>次のステップ
HoloLens 2 または Unity エディターでアプリを試してみてください。 アプリで、ボタンをタッチして、ビデオの spatialization をアクティブ化および非アクティブ化できるようになりました。 Unity エディターでテストする場合は、スペースバーを押し、スクロールホイールを使用してスクロールし、ハンドシミュレーションをアクティブにします。 

[第5章](unity-spatial-audio-ch5.md)に進み、リバーブを使用してサウンドソースに認識された距離を追加します。

