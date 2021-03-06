---
title: Unity での手と視点の追跡
description: Unity、ハンドジェスチャ、およびモーションコントローラーでのアクションを実行するには、2つの主要な方法があります。
author: thetuvix
ms.author: alexturn
ms.date: 03/21/2018
ms.topic: article
keywords: ジェスチャ、モーションコントローラー、unity、宝石、入力
ms.openlocfilehash: fc56436cbe71f958b91fec56c5f0f7d93926b2ac
ms.sourcegitcommit: 317653cd8500563c514464f0337c1f230a6f3653
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/28/2019
ms.locfileid: "75503890"
---
# <a name="articulated-hand-and-eye-tracking-in-unity"></a>Unity での手と視点の追跡

HoloLens 2 では、左手や視線の追跡など、いくつかの新機能と魅力的な機能が導入されました。

Unity の新機能を利用する最も簡単な方法は、MRTK v2 を使用することです。 また、使用を開始する際に役立ついくつかのシーンもあります。

* [MRTK v2 での手による作業の開始](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/Input/HandTracking.html)
* [MRTK v2 での視線追跡の概要](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/EyeTracking/EyeTracking_Main.html)

## <a name="building-blocks-supporting-hands-eyes-and-others-in-mrtk-v2"></a>MRTK v2 での手や目などをサポートするビルディングブロック

MRTK v2 には、開発を高速化するために役立つ一連の UI コントロールと構成要素が用意されています。

|  [![ボタン](images/MRTK_Button_Main.png)](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_Button.html) [ボタン](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_Button.html) | [![境界ボックス](images/MRTK_BoundingBox_Main.png)](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_BoundingBox.html)の[境界ボックス](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_BoundingBox.html) | [![操作ハンドラー](images/MRTK_Manipulation_Main.png)](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_ManipulationHandler.html)の[操作ハンドラー](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_ManipulationHandler.html) |
|:--- | :--- | :--- |
| HoloLens2's を含むさまざまな入力メソッドをサポートするボタンコントロール | 3D 空間内のオブジェクトを操作するための標準 UI | 1人または2人の手でオブジェクトを操作するためのスクリプト |
|  [![スレート](images/MRTK_Slate_Main.png)](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_Slate.html) [スレート](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_Slate.html) | [![システムキーボード](images/MRTK_SystemKeyboard_Main.png)](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_SystemKeyboard.html)[システムキーボード](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_SystemKeyboard.html) | [![対話型](images/InteractableExamples.png)](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_Interactable.html) [対話型](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_Interactable.html) |
| 入力によるスクロールをサポートする2D スタイルプレーン | Unity でシステムキーボードを使用するサンプルスクリプト  | 視覚的な状態とテーマのサポートを使用してオブジェクトを対話型するためのスクリプト |
|  [![ソルバー](images/MRTK_Solver_Main.png)](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_Solver.html) [ソルバー](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_Solver.html) | [![オブジェクトコレクション](images/MRTK_ObjectCollection_Main.png)](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_ManipulationHandler.html)[オブジェクトコレクション](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_ManipulationHandler.html) | [![ツール](images/MRTK_Tooltip_Main.png)](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_Tooltip.html)ヒントの[ツールヒント](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_Tooltip.html) |
| タグに沿って、本文ロック、定数ビューサイズ、表面吸着などのさまざまなオブジェクトの配置動作 | 3次元図形内のオブジェクトの配列をレイアウトするためのスクリプト | フレキシブルアンカー/ピボットシステムを使用した注釈 UI。モーションコントローラーとオブジェクトのラベル付けに使用できます。 |
|  [![アプリ](images/MRTK_AppBar_Main.png)](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_AppBar.html)バー[アプリバー](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_AppBar.html) | [![ポインター](images/MRTK_Pointer_Main.png)](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/Input/Pointers.html) [ポインター](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/Input/Pointers.html) | [![指先ビジュアライゼーション](images/MRTK_FingertipVisualization_Main.png)](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_FingertipVisualization.html)[指先の視覚化](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_FingertipVisualization.html) |
| 境界ボックスの手動アクティブ化の UI | さまざまな種類のポインターについて | 指先での Visual affordance による直接的な相互作用の信頼性の向上 |
|  [![目の追跡: ターゲット選択](images/mrtk_et_targetselect.png)](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/EyeTracking/EyeTracking_TargetSelection.html)の[視点の追跡: ターゲットの選択](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/EyeTracking/EyeTracking_TargetSelection.html) | [![目の追跡: ナビゲーション](images/mrtk_et_navigation.png)](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/EyeTracking/EyeTracking_Navigation.html)の[追跡: ナビゲーション](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/EyeTracking/EyeTracking_Navigation.html) | [![目の追跡: ヒートマップ](images/mrtk_et_heatmaps.png)](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/EyeTracking/EyeTracking_Visualization.html)の[視線追跡: ヒートマップ](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/EyeTracking/EyeTracking_Visualization.html) |
| 視点、音声、手入力を組み合わせてシーン全体のホログラムをすばやく簡単に選択する | 表示内容に基づいて、テキストを自動的にスクロールするか、フォーカスされているコンテンツにズームする方法について説明します。| アプリに表示されているユーザーのログ記録、読み込み、視覚化の例 |

## <a name="example-scenes"></a>シーンの例

[このシーンの例](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_HandInteractionExamples.html)では、MRTK のさまざまな種類の相互作用と UI コントロールについて説明します。

その他の例については、「 **Assets/MixedRealityToolkit/デモ**フォルダー」にある[Mixed Reality Toolkit GitHub](https://github.com/Microsoft/MixedRealityToolkit-Unity)を参照してください。

[シーンの ![例](images/MRTK_Examples.png)](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_HandInteractionExamples.html)

## <a name="see-also"></a>「

* [視線ベースの操作] (eye-gaze-interaction.md)
* [HoloLens 2 上の視線追跡] (eye-tracking.md)
* [視線入力とコミット](gaze-and-commit.md)
* [手 - 直接操作](direct-manipulation.md)
* [手 - ジェスチャ](gaze-and-commit.md#composite-gestures)
* [手 - ポイントとコミット](point-and-commit.md)
* [本能的な操作](interaction-fundamentals.md)
* [モーション コントローラー](motion-controllers.md)
* [UnityEngine. XR](https://docs.unity3d.com/ScriptReference/XR.WSA.Input.InteractionManager.html)
* [UnityEngine. XR 追跡](https://docs.unity3d.com/ScriptReference/XR.InputTracking.html)
