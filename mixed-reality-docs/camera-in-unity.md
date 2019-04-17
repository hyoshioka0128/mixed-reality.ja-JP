---
title: Unity でのカメラ
description: Unity のメイン カメラ用 Windows Mixed Reality 開発を使用して、holographic のレンダリングを実行する方法
author: keveleigh
ms.author: kurtie
ms.date: 03/21/2018
ms.topic: article
keywords: holotoolkit、mixedrealitytoolkit、mixedrealitytoolkit unity、holographic レンダリング、holographic、没入型、フォーカス ポイント、深度バッファー、方向専用、位置指定、非透過、透過的な場合は、クリップ
ms.openlocfilehash: 8ea5a1f53351faab1b2863a0afac74e958b4b1a0
ms.sourcegitcommit: 384b0087899cd835a3a965f75c6f6c607c9edd1b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/12/2019
ms.locfileid: "59597781"
---
# <a name="camera-in-unity"></a><span data-ttu-id="28086-104">Unity でのカメラ</span><span class="sxs-lookup"><span data-stu-id="28086-104">Camera in Unity</span></span>

<span data-ttu-id="28086-105">Mixed reality ヘッドセットを着用する holographic 世界の中心になります。</span><span class="sxs-lookup"><span data-stu-id="28086-105">When you wear a mixed reality headset, it becomes the center of your holographic world.</span></span> <span data-ttu-id="28086-106">Unity[カメラ](http://docs.unity3d.com/Manual/class-Camera.html)コンポーネント ステレオスコ ピック レンダリングは自動的に処理し、は、ヘッドの移動と回転するときに従う、プロジェクトで「仮想現実はサポートされて」"Windows Mixed Reality"で選択されている (デバイスとしてその他の設定 セクションの Windows ストアのプレーヤー設定)。</span><span class="sxs-lookup"><span data-stu-id="28086-106">The Unity [Camera](http://docs.unity3d.com/Manual/class-Camera.html) component will automatically handle stereoscopic rendering and will follow your head movement and rotation when your project has "Virtual Reality Supported" selected with "Windows Mixed Reality" as the device (in the Other Settings section of the Windows Store Player Settings).</span></span> <span data-ttu-id="28086-107">これは、以前のバージョンの Unity で"Windows Holographic"として表示可能性があります。</span><span class="sxs-lookup"><span data-stu-id="28086-107">This may be listed as "Windows Holographic" in older versions of Unity.</span></span>

<span data-ttu-id="28086-108">ただし、表示品質を最適化して[ホログラム安定性](hologram-stability.md)カメラの設定を以下に説明を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="28086-108">However, to fully optimize visual quality and [hologram stability](hologram-stability.md), you should set the camera settings described below.</span></span>

>[!NOTE]
><span data-ttu-id="28086-109">これらの設定は、アプリの各シーンでカメラに適用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="28086-109">These settings need to be applied to the Camera in each scene of your app.</span></span>
>
><span data-ttu-id="28086-110">既定では、Unity では、新しいシーンを作成するときにカメラのコンポーネントが含まれていますが正しく適用されている以下の設定はありませんが、階層内のメイン カメラ GameObject が格納されます。</span><span class="sxs-lookup"><span data-stu-id="28086-110">By default, when you create a new scene in Unity, it will contain a Main Camera GameObject in the Hierarchy which includes the Camera component, but does not have the settings below properly applied.</span></span>

## <a name="holographic-vs-immersive-headsets"></a><span data-ttu-id="28086-111">イマーシブ ヘッドセットとホログラフィック</span><span class="sxs-lookup"><span data-stu-id="28086-111">Holographic vs. immersive headsets</span></span>

<span data-ttu-id="28086-112">Unity のカメラのコンポーネントの既定の設定では、現実の世界があるないために、スカイ ボックスのような背景を必要とする従来の 3D アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="28086-112">The default settings on the Unity Camera component are for traditional 3D applications which need a skybox-like background as they don't have a real world.</span></span>
* <span data-ttu-id="28086-113">実行されているときに、 **[イマーシブ ヘッドセット](immersive-headset-hardware-details.md)**、ユーザーに表示される、すべてのものをレンダリングして、したがって、スカイ ボックスを保持する必要あります可能性があります。</span><span class="sxs-lookup"><span data-stu-id="28086-113">When running on an **[immersive headset](immersive-headset-hardware-details.md)**, you are rendering everything the user sees, and so you'll likely want to keep the skybox.</span></span>
* <span data-ttu-id="28086-114">ただしで実行されているときに、 **holographic ヘッドセット**など[HoloLens](hololens-hardware-details.md)、現実の世界がレンダリングすべてカメラの背後に表示されます。</span><span class="sxs-lookup"><span data-stu-id="28086-114">However, when running on a **holographic headset** like [HoloLens](hololens-hardware-details.md), the real world should appear behind everything the camera renders.</span></span> <span data-ttu-id="28086-115">これを行うには、HoloLens、透明色としてレンダリングを黒) の「透過的カメラ背景を設定するスカイ ボックス テクスチャではなく。</span><span class="sxs-lookup"><span data-stu-id="28086-115">To do this, set the camera background to be transparent (in HoloLens, black renders as transparent) instead of a Skybox texture:</span></span>
    1. <span data-ttu-id="28086-116">階層ウィンドウで、メイン カメラを選択します。</span><span class="sxs-lookup"><span data-stu-id="28086-116">Select the Main Camera in the Hierarchy panel</span></span>
    2. <span data-ttu-id="28086-117">Inspector パネルで、カメラのコンポーネントを検索し、スカイ ボックスのフラグをクリア ドロップダウンを純色に変更します。</span><span class="sxs-lookup"><span data-stu-id="28086-117">In the Inspector panel, find the Camera component and change the Clear Flags dropdown from Skybox to Solid Color</span></span>
    3. <span data-ttu-id="28086-118">バック グラウンドのカラー ピッカーを選択し、(0, 0、0, 0) に、RGBA 値を変更します。</span><span class="sxs-lookup"><span data-stu-id="28086-118">Select the Background color picker and change the RGBA values to (0, 0, 0, 0)</span></span>

<span data-ttu-id="28086-119">実行時にチェックしてが、没入型か holographic にヘッドセットかどうかを判断するスクリプト コードを使用する[HolographicSettings.IsDisplayOpaque](https://docs.unity3d.com/ScriptReference/XR.WSA.HolographicSettings.IsDisplayOpaque.html)します。</span><span class="sxs-lookup"><span data-stu-id="28086-119">You can use script code to determine at runtime whether the headset is immersive or holographic by checking [HolographicSettings.IsDisplayOpaque](https://docs.unity3d.com/ScriptReference/XR.WSA.HolographicSettings.IsDisplayOpaque.html).</span></span>


## <a name="positioning-the-camera"></a><span data-ttu-id="28086-120">カメラの位置</span><span class="sxs-lookup"><span data-stu-id="28086-120">Positioning the Camera</span></span>

<span data-ttu-id="28086-121">(X: として、ユーザーの開始位置を想定している場合、アプリのレイアウトに簡単になります0、Y:0、Z:0).</span><span class="sxs-lookup"><span data-stu-id="28086-121">It will be easier to lay out your app if you imagine the starting position of the user as (X: 0, Y: 0, Z: 0).</span></span> <span data-ttu-id="28086-122">Main Camera は、ユーザーの頭の動きを追跡は、ために、Main Camera の開始位置を設定して、ユーザーの開始位置を設定できます。</span><span class="sxs-lookup"><span data-stu-id="28086-122">Since the Main Camera is tracking movement of the user's head, the starting position of the user can be set by setting the starting position of the Main Camera.</span></span>
1. <span data-ttu-id="28086-123">階層ウィンドウでメイン カメラを選択します。</span><span class="sxs-lookup"><span data-stu-id="28086-123">Select Main Camera in the Hierarchy panel</span></span>
2. <span data-ttu-id="28086-124">Inspector パネルで、変換コンポーネントを検索し、(x: から位置を変更します。0、Y:1、z:-10) には、(x:0、Y:0、Z:0)</span><span class="sxs-lookup"><span data-stu-id="28086-124">In the Inspector panel, find the Transform component and change the Position from (X: 0, Y: 1, Z: -10) to (X: 0, Y: 0, Z: 0)</span></span>

   <span data-ttu-id="28086-125">![インスペクター ウィンドウの Unity でのカメラ](images/maincamera-350px.png)</span><span class="sxs-lookup"><span data-stu-id="28086-125">![Camera in the Inspector pane in Unity](images/maincamera-350px.png)</span></span><br>
   <span data-ttu-id="28086-126">*インスペクター ウィンドウの Unity でのカメラ*</span><span class="sxs-lookup"><span data-stu-id="28086-126">*Camera in the Inspector pane in Unity*</span></span>

## <a name="clip-planes"></a><span data-ttu-id="28086-127">クリップ面</span><span class="sxs-lookup"><span data-stu-id="28086-127">Clip planes</span></span>

<span data-ttu-id="28086-128">コンテンツのレンダリングに近すぎるユーザーできます複合現実で使いやすい。</span><span class="sxs-lookup"><span data-stu-id="28086-128">Rendering content too close to the user can be uncomfortable in mixed reality.</span></span> <span data-ttu-id="28086-129">調整することができます、[近く、はるかにクリップ平面](hologram-stability.md#hologram-render-distances)カメラ コンポーネント。</span><span class="sxs-lookup"><span data-stu-id="28086-129">You can adjust the [near and far clip planes](hologram-stability.md#hologram-render-distances) on the Camera component.</span></span>
1. <span data-ttu-id="28086-130">階層ウィンドウで、メイン カメラを選択します。</span><span class="sxs-lookup"><span data-stu-id="28086-130">Select the Main Camera in the Hierarchy panel</span></span>
2. <span data-ttu-id="28086-131">Inspector パネルで、カメラ コンポーネント クリッピング平面を見つけて.85 0.3 からほぼテキスト ボックスに変更します。</span><span class="sxs-lookup"><span data-stu-id="28086-131">In the Inspector panel, find the Camera component Clipping Planes and change the Near textbox from 0.3 to .85.</span></span> <span data-ttu-id="28086-132">もっと近づきレンダリングされたコンテンツがユーザーの不安につながるとあたりしないで、[距離のガイドラインをレンダリング](hologram-stability.md#hologram-render-distances)。</span><span class="sxs-lookup"><span data-stu-id="28086-132">Content rendered even closer can lead to user discomfort and should be avoided per the [render distance guidelines](hologram-stability.md#hologram-render-distances).</span></span>

## <a name="multiple-cameras"></a><span data-ttu-id="28086-133">複数のカメラ</span><span class="sxs-lookup"><span data-stu-id="28086-133">Multiple Cameras</span></span>

<span data-ttu-id="28086-134">シーンでカメラの複数のコンポーネントがある場合は、Unity ステレオスコ ピックのレンダリングに使用するには、どのカメラを知っているし、どの GameObject をチェックしてヘッドの追跡が MainCamera タグ。</span><span class="sxs-lookup"><span data-stu-id="28086-134">When there are multiple Camera components in the scene, Unity knows which camera to use for stereoscopic rendering and head tracking by checking which GameObject has the MainCamera tag.</span></span>

## <a name="recentering-a-seated-experience"></a><span data-ttu-id="28086-135">取り付けられていないエクスペリエンス中</span><span class="sxs-lookup"><span data-stu-id="28086-135">Recentering a seated experience</span></span>

<span data-ttu-id="28086-136">構築する場合、[取り付けられているスケール エクスペリエンス](coordinate-systems.md)、呼び出すことによって、ユーザーの現在のヘッドの位置に戻しますの Unity の世界配信元ことができます、 **[XR します。InputTracking.Recenter](https://docs.unity3d.com/ScriptReference/XR.InputTracking.Recenter.html)** メソッド。</span><span class="sxs-lookup"><span data-stu-id="28086-136">If you're building a [seated-scale experience](coordinate-systems.md), you can recenter Unity's world origin at the user's current head position by calling the **[XR.InputTracking.Recenter](https://docs.unity3d.com/ScriptReference/XR.InputTracking.Recenter.html)** method.</span></span>

## <a name="reprojection-modes"></a><span data-ttu-id="28086-137">Reprojection モード</span><span class="sxs-lookup"><span data-stu-id="28086-137">Reprojection modes</span></span>

<span data-ttu-id="28086-138">HoloLens とイマーシブ ヘッドセットの両方には、各フレームの光子が出力されるときに、ユーザーの実際のヘッドの位置の misprediction の調整を表示するため、アプリが reproject されます。</span><span class="sxs-lookup"><span data-stu-id="28086-138">Both HoloLens and immersive headsets will reproject each frame your app renders to adjust for any misprediction of the user's actual head position when photons are emitted.</span></span>

<span data-ttu-id="28086-139">既定では。</span><span class="sxs-lookup"><span data-stu-id="28086-139">By default:</span></span>

* <span data-ttu-id="28086-140">**イマーシブ ヘッドセット**位置指定 reprojection、アプリは、特定のフレームの深度バッファーを提供する場合、ホログラムの位置と向きの両方で misprediction を調整することを実行します。</span><span class="sxs-lookup"><span data-stu-id="28086-140">**Immersive headsets** will perform positional reprojection, adjusting your holograms for misprediction in both position and orientation, if the app provides a depth buffer for a given frame.</span></span>  <span data-ttu-id="28086-141">深度バッファーが指定されていない場合、システムはキャッシュミス方向にのみ修正します。</span><span class="sxs-lookup"><span data-stu-id="28086-141">If a depth buffer is not provided, the system will only correct mispredictions in orientation.</span></span>
* <span data-ttu-id="28086-142">**Holographic ヘッドセット**かどうか、アプリが、深度バッファーを提供するかどうか、HoloLens の位置指定 reprojection は実行するようにします。</span><span class="sxs-lookup"><span data-stu-id="28086-142">**Holographic headsets** like HoloLens will perform positional reprojection whether the app provides its depth buffer or not.</span></span>  <span data-ttu-id="28086-143">レンダリングは、現実の世界で提供される、安定したバック グラウンドではスパースでは多くの場合、位置指定 reprojection は深度バッファー HoloLens のない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="28086-143">Positional reprojection is possible without depth buffers on HoloLens as rendering is often sparse with a stable background provided by the real world.</span></span>

<span data-ttu-id="28086-144">作成することがわかっている場合、[方向のみのエクスペリエンス](coordinate-systems-in-unity.md#building-an-orientation-only-or-seated-scale-experience)剛体本文-ロックされているコンテンツ (例: 360 度のビデオ コンテンツ) のみで印刷の向きを reprojection モードを明示的に設定できます[HolographicSettings.ReprojectionMode](https://docs.unity3d.com/ScriptReference/XR.WSA.HolographicSettings.ReprojectionMode.html)に[HolographicReprojectionMode.OrientationOnly](https://docs.unity3d.com/ScriptReference/XR.WSA.HolographicSettings.HolographicReprojectionMode.html)します。</span><span class="sxs-lookup"><span data-stu-id="28086-144">If you know that you are building an [orientation-only experience](coordinate-systems-in-unity.md#building-an-orientation-only-or-seated-scale-experience) with rigidly body-locked content (e.g. 360-degree video content), you can explicitly set the reprojection mode to be orientation only by setting [HolographicSettings.ReprojectionMode](https://docs.unity3d.com/ScriptReference/XR.WSA.HolographicSettings.ReprojectionMode.html) to [HolographicReprojectionMode.OrientationOnly](https://docs.unity3d.com/ScriptReference/XR.WSA.HolographicSettings.HolographicReprojectionMode.html).</span></span>

## <a name="sharing-your-depth-buffers-with-windows"></a><span data-ttu-id="28086-145">Windows と、深度バッファーの共有</span><span class="sxs-lookup"><span data-stu-id="28086-145">Sharing your depth buffers with Windows</span></span>

<span data-ttu-id="28086-146">共有を Windows の各フレームは、アプリ 2 つの要因の 1 つでホログラム安定性、ヘッドセットの種類に基づいて、アプリの深度バッファーのレンダリングしています。</span><span class="sxs-lookup"><span data-stu-id="28086-146">Sharing your app's depth buffer to Windows each frame will give your app one of two boosts in hologram stability, based on the type of headset you're rendering for:</span></span>
* <span data-ttu-id="28086-147">**イマーシブ ヘッドセット**深度バッファーが指定の位置と向きの両方で misprediction、ホログラムを調整する場合に、位置指定 reprojection を実行できます。</span><span class="sxs-lookup"><span data-stu-id="28086-147">**Immersive headsets** can perform positional reprojection when a depth buffer is provided, adjusting your holograms for misprediction in both position and orientation.</span></span>
* <span data-ttu-id="28086-148">**Holographic ヘッドセット**HoloLens が自動的に選択されているように、[フォーカス ポイント](focus-point-in-unity.md)深度バッファーが提供されている場合は、最もコンテンツと交差する面に沿ったホログラム安定性を最適化します。</span><span class="sxs-lookup"><span data-stu-id="28086-148">**Holographic headsets** like HoloLens will automatically select a [focus point](focus-point-in-unity.md) when a depth buffer is provided, optimizing hologram stability along the plane that intersects the most content.</span></span>

<span data-ttu-id="28086-149">Unity アプリは Windows に深度バッファーを提供するかどうかを設定するには。</span><span class="sxs-lookup"><span data-stu-id="28086-149">To set whether your Unity app will provide a depth buffer to Windows:</span></span>
1. <span data-ttu-id="28086-150">移動して**編集** > **プロジェクト設定** > **Player** > **ユニバーサル Windows プラットフォーム タブ**  >  **XR 設定**します。</span><span class="sxs-lookup"><span data-stu-id="28086-150">Go to **Edit** > **Project Settings** > **Player** > **Universal Windows Platform tab** > **XR Settings**.</span></span>
2. <span data-ttu-id="28086-151">展開、 **Windows Mixed Reality SDK**項目。</span><span class="sxs-lookup"><span data-stu-id="28086-151">Expand the **Windows Mixed Reality SDK** item.</span></span>
3. <span data-ttu-id="28086-152">オンまたはオフにして、**深度バッファーの共有を有効にする**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="28086-152">Check or uncheck the **Enable Depth Buffer Sharing** check box.</span></span>  <span data-ttu-id="28086-153">これは、アップグレードされた古いプロジェクトに対して既定でこの機能が Unity に追加された、オフになりますのでを作成した新しいプロジェクトで既定でチェックされます。</span><span class="sxs-lookup"><span data-stu-id="28086-153">This will be checked by default in new projects created since this feature was added to Unity and will be unchecked by default for older projects that were upgraded.</span></span>

<span data-ttu-id="28086-154">Windows に深度バッファーを提供することにより、Windows 正確にマップできます、深度バッファー内のピクセルごとの正規化された深度値メートル単位の距離にメイン カメラの Unity で設定したのとほぼ平面を使用している限り、表示品質が向上します。</span><span class="sxs-lookup"><span data-stu-id="28086-154">Providing a depth buffer to Windows can improve visual quality so long as Windows can accurately map the normalized per-pixel depth values in your depth buffer back to distances in meters, using the near and far planes you've set in Unity on the main camera.</span></span>  <span data-ttu-id="28086-155">レンダリングがハンドルの深さをパスした場合、一般的な方法で値おく必要がある一般的にここでは、半透明のレンダリングは、既存透けて表示されるときに、深度バッファーに書き込みを渡しますが色ピクセル混乱することが、reprojection 問題ありません。</span><span class="sxs-lookup"><span data-stu-id="28086-155">If your render passes handle depth values in typical ways, you should generally be fine here, though translucent render passes that write to the depth buffer while showing through to existing color pixels can confuse the reprojection.</span></span>  <span data-ttu-id="28086-156">レンダリング パスが不正確な深さの値を持つ、最終的な深さピクセルの多くにままがわかっている場合は共有することでオフ"を有効にする深度バッファー"表示品質を向上させる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="28086-156">If you know that your render passes will leave many of your final depth pixels with inaccurate depth values, you are likely to get better visual quality by unchecking "Enable Depth Buffer Sharing".</span></span>

## <a name="mixed-reality-toolkits-automatic-scenesetup"></a><span data-ttu-id="28086-157">Mixed Reality Toolkit の自動シーンのセットアップ</span><span class="sxs-lookup"><span data-stu-id="28086-157">Mixed Reality Toolkit's automatic scene setup</span></span>
<span data-ttu-id="28086-158">インポートするときに[MRTK が Unity パッケージをリリース](https://github.com/Microsoft/MixedRealityToolkit-Unity/releases)からプロジェクトを複製するか、 [GitHub リポジトリ](https://github.com/Microsoft/MixedRealityToolkit-Unity)Unity で新しいメニュー ' Mixed Reality Toolkit' を検索します。</span><span class="sxs-lookup"><span data-stu-id="28086-158">When you import [MRTK release Unity packages](https://github.com/Microsoft/MixedRealityToolkit-Unity/releases) or clone the project from the [GitHub repository](https://github.com/Microsoft/MixedRealityToolkit-Unity), you are going to find a new menu 'Mixed Reality Toolkit' in Unity.</span></span> <span data-ttu-id="28086-159">[構成] メニューで、' Mixed Reality シーン設定の適用 ' メニューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="28086-159">Under 'Configure' menu, you will see the menu 'Apply Mixed Reality Scene Settings'.</span></span> <span data-ttu-id="28086-160">既定のカメラを削除し、基本コンポーネントを追加します。 これをクリックすると [InputManager](https://github.com/Microsoft/MixedRealityToolkit-Unity/blob/htk_release/Assets/HoloToolkit/Input/Prefabs/InputManager.prefab)、 [MixedRealityCameraParent](https://github.com/Microsoft/MixedRealityToolkit-Unity/blob/htk_release/Assets/HoloToolkit/Input/Prefabs/MixedRealityCameraParent.prefab)、および[DefaultCursor](https://github.com/Microsoft/MixedRealityToolkit-Unity/blob/htk_release/Assets/HoloToolkit/Input/Prefabs/Cursor/DefaultCursor.prefab)します。</span><span class="sxs-lookup"><span data-stu-id="28086-160">When you click it, it removes the default camera and adds foundational components - [InputManager](https://github.com/Microsoft/MixedRealityToolkit-Unity/blob/htk_release/Assets/HoloToolkit/Input/Prefabs/InputManager.prefab), [MixedRealityCameraParent](https://github.com/Microsoft/MixedRealityToolkit-Unity/blob/htk_release/Assets/HoloToolkit/Input/Prefabs/MixedRealityCameraParent.prefab), and [DefaultCursor](https://github.com/Microsoft/MixedRealityToolkit-Unity/blob/htk_release/Assets/HoloToolkit/Input/Prefabs/Cursor/DefaultCursor.prefab).</span></span>

<span data-ttu-id="28086-161">![シーンのセットアップの MRTK メニュー](images/MRTK_Input_Menu.png)</span><span class="sxs-lookup"><span data-stu-id="28086-161">![MRTK Menu for scene setup](images/MRTK_Input_Menu.png)</span></span><br>
<span data-ttu-id="28086-162">*シーンのセットアップの MRTK メニュー*</span><span class="sxs-lookup"><span data-stu-id="28086-162">*MRTK Menu for scene setup*</span></span>

<span data-ttu-id="28086-163">![MRTK で自動シーンのセットアップ](images/MRTK_HowTo_Input1.png)</span><span class="sxs-lookup"><span data-stu-id="28086-163">![Automatic scene setup in MRTK](images/MRTK_HowTo_Input1.png)</span></span><br>
<span data-ttu-id="28086-164">*MRTK で自動シーンのセットアップ*</span><span class="sxs-lookup"><span data-stu-id="28086-164">*Automatic scene setup in MRTK*</span></span>

## <a name="mixedrealitycamera-prefab"></a><span data-ttu-id="28086-165">MixedRealityCamera プレハブ</span><span class="sxs-lookup"><span data-stu-id="28086-165">MixedRealityCamera prefab</span></span>
<span data-ttu-id="28086-166">これらのプロジェクト パネルから手動で追加できます。</span><span class="sxs-lookup"><span data-stu-id="28086-166">You can also manually add these from the project panel.</span></span> <span data-ttu-id="28086-167">プレハブとしてこれらのコンポーネントを見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="28086-167">You can find these components as prefabs.</span></span> <span data-ttu-id="28086-168">検索する場合に**MixedRealityCamera**、2 つの異なるカメラ プレハブを表示することができます。</span><span class="sxs-lookup"><span data-stu-id="28086-168">When you search **MixedRealityCamera**, you will be able to see two different camera prefabs.</span></span> <span data-ttu-id="28086-169">違いは、 **MixedRealityCamera**はカメラのみプレハブは、 **MixedRealityCameraParent** Teleportation、モーションなどイマーシブ ヘッドセットの追加のコンポーネントが含まれていますコント ローラーと、境界。</span><span class="sxs-lookup"><span data-stu-id="28086-169">The difference is, **MixedRealityCamera** is the camera only prefab whereas, **MixedRealityCameraParent** includes additional components for the immersive headsets such as Teleportation, Motion Controller and, Boundary.</span></span>

<span data-ttu-id="28086-170">![MRTK でカメラのプレハブ](images/MRTK_HowTo_Input2.png)</span><span class="sxs-lookup"><span data-stu-id="28086-170">![Camera prefabs in MRTK](images/MRTK_HowTo_Input2.png)</span></span><br>
<span data-ttu-id="28086-171">*MRTK でカメラのプレハブ*</span><span class="sxs-lookup"><span data-stu-id="28086-171">*Camera prefabs in MRTK*</span></span>

<span data-ttu-id="28086-172">**MixedRealtyCamera** HoloLens とイマーシブ ヘッドセットの両方をサポートします。</span><span class="sxs-lookup"><span data-stu-id="28086-172">**MixedRealtyCamera** supports both HoloLens and immersive headset.</span></span> <span data-ttu-id="28086-173">デバイスの種類を検出し、フラグをクリアし、スカイ ボックスなどのプロパティを最適化します。</span><span class="sxs-lookup"><span data-stu-id="28086-173">It detects the device type and optimizes the properties such as clear flags and Skybox.</span></span> <span data-ttu-id="28086-174">以下、アニメーション コント ローラー モデルでは、カスタムのカーソルなどをカスタマイズでき、Floor、便利なプロパティの一部を見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="28086-174">Below you can find some of the useful properties you can customize such as custom Cursor, Motion Controller models, and Floor.</span></span>

<span data-ttu-id="28086-175">![カーソルと床面、モーションのコント ローラーのプロパティ](images/MRTK_HowTo_Input3.png)</span><span class="sxs-lookup"><span data-stu-id="28086-175">![Properties for the Motion controller, Cursor and Floor](images/MRTK_HowTo_Input3.png)</span></span><br>
<span data-ttu-id="28086-176">*カーソルと床面、モーションのコント ローラーのプロパティ*</span><span class="sxs-lookup"><span data-stu-id="28086-176">*Properties for the Motion controller, Cursor and Floor*</span></span>

## <a name="see-also"></a><span data-ttu-id="28086-177">関連項目</span><span class="sxs-lookup"><span data-stu-id="28086-177">See also</span></span>
* [<span data-ttu-id="28086-178">ホログラム安定性</span><span class="sxs-lookup"><span data-stu-id="28086-178">Hologram stability</span></span>](hologram-stability.md)
* [<span data-ttu-id="28086-179">MixedRealityToolkit Main Camera.prefab</span><span class="sxs-lookup"><span data-stu-id="28086-179">MixedRealityToolkit Main Camera.prefab</span></span>](https://github.com/Microsoft/MixedRealityToolkit-Unity/tree/htk_release/Assets/HoloToolkit/Input/Prefabs)