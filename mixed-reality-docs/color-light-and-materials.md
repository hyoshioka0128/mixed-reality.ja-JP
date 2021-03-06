---
title: 色、光、マテリアル
description: Mixed Reality のコンテンツの設計では、エクスペリエンスで使用される各ビジュアル資産の色、ライト、マテリアルを慎重に検討する必要があります。
author: mavitazk
ms.author: pinkb
ms.date: 03/21/2018
ms.topic: article
keywords: Windows Mixed Reality、デザイン、カラー、ライト、素材
ms.openlocfilehash: b29fe8da92e67d15592f9d4503bc278d4fe9fd95
ms.sourcegitcommit: 05fa75193059a2dac4b580a9eef7b6c4bb64d8d7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2019
ms.locfileid: "74835088"
---
# <a name="color-light-and-materials"></a>色、光、マテリアル

Mixed Reality のコンテンツの設計では、エクスペリエンスで使用される各ビジュアル資産の色、ライト、マテリアルを慎重に検討する必要があります。 これらの決定は、明るい色や素材を使用したイマーシブ環境の雰囲気の設定、機能の目的 (たとえば、よりよくある行動をユーザーに警告するための照明色の使用など) の両方に適しています。 これらの各決定は、エクスペリエンスのターゲットデバイスの機会と制約に照らし合わせて検討する必要があります。

次に示すのは、イマーシブと holographic ヘッドセットの両方でのアセットのレンダリングに固有のガイドラインです。 これらの多くは、他の技術領域に密接に関連しており、関連する項目の一覧に[ついて](color,-light-and-materials.md#see-also)は、この記事の最後にある「関連項目」セクションを参照してください。

## <a name="rendering-on-immersive-vs-holographic-devices"></a>イマーシブおよび holographic デバイスでのレンダリング

イマーシブヘッドセットでレンダリングされるコンテンツは、holographic ヘッドセットでレンダリングされるコンテンツと比較すると、視覚的に異なります。 イマーシブヘッドセットでは、通常、2D 画面の場合と同じようにコンテンツがレンダリングされますが、HoloLens のような holographic ヘッドセットは、カラーシーケンシャルで、「」を参照して、ホログラムをレンダリングします。

Holographic ヘッドセットで holographic エクスペリエンスをテストするには、常に時間がかかります。 コンテンツの外観は、holographic デバイス専用に構築されている場合でも、セカンダリモニター、スナップショット、および spectator ビューで表示されるものとは異なります。 デバイスのエクスペリエンスについて説明し、ホログラムの照明をテストして、コンテンツがどのようにレンダリングされるかについて、すべての辺 (および上および下) から観察することを忘れないでください。 デバイスのさまざまな明るさの設定でテストしてください。すべてのユーザーが想定されている既定値と、さまざまな照明条件のセットを共有することはほとんどありません。

## <a name="fundamentals-of-rendering-on-holographic-devices"></a>Holographic デバイスでのレンダリングの基礎
* Holographic のデバイスには追加の**ディスプレイがあり**ます。実際の世界から光に光を追加することによって、ホログラムが作成されます。白は明るいと表示され、黒は透明に見えます。
* **色の影響はユーザーの環境によって異なり**ます。ユーザーの部屋にはさまざまな照明条件があります。 わかりやすくするために、適切なコントラストレベルでコンテンツを作成します。
* **動的な照明を避ける**– holographic エクスペリエンスで一様に点灯するホログラムは最も効率的です。 高度な機能を使用すると、モバイルデバイスの機能を超える可能性があります。 動的ライティングが必要な場合は、 [Mixed Reality Toolkit の標準シェーダー](https://github.com/microsoft/MixedRealityToolkit-Unity/blob/mrtk_release/Documentation/README_MRTKStandardShader.md)を使用することをお勧めします。 

## <a name="designing-with-color"></a>色を使用したデザイン

加法表示の性質により、holographic の表示によって特定の色が異なる場合があります。 照明環境ではいくつかの色が表示されますが、他の色は少ないインパクトとして表示されます。 クール色は背景に recede 傾向があり、ウォームカラーは前景に移動します。 エクスペリエンスの色を調べる際には、次の要因を考慮してください。
* 色**域**-HoloLens は、概念的には Adobe RGB に似ています。 その結果、色によっては、デバイス内のさまざまな品質と表現が表示されることがあります。
* **ガンマ**-レンダリングされるイメージの明るさとコントラストは、イマーシブデバイスと holographic デバイスで異なります。 多くの場合、これらのデバイスの違いは、色や影の暗い領域を作成したり、明るくしたりするように見えます。
* **色の分離**-"色を分割した" または "カラー fringing" とも呼ばれ、ユーザーが目のオブジェクトを追跡するときに、最も一般的に色の分離が発生します。
* **色の統一性**-通常、ホログラムは、背景に関係なくカラーの統一性を維持するために十分な明るい色で表示されます。 大きな領域が blotchy になる可能性があります。 明るい、純色の大きな領域は避けてください。
* **明るい色の表示**-白は非常に明るいため、控えめに使用する必要があります。 ほとんどの場合、R 235 G 235 B 235 に関する白い値を検討してください。 大きな明るい領域では、ユーザー不快感が発生する可能性があります。

**ダークカラーのレンダリング**

加法表示の性質により、暗い色は透明に見えます。 純色の黒のオブジェクトは、実際の世界とは異なるものとして表示されます。 下記の「アルファチャネル」を参照してください。 "Black" のように見えるようにするには、16、16、16のような非常に濃い灰色の RGB 値を試してみてください。

![標準とワイド色域](images/640px-widegamut.png)<br>
*標準またはワイド色域*

## <a name="technical-considerations"></a>技術的な考慮事項
* **別名**-ホログラム、ギザギザ、"階段のよく検討" のように、ホログラムのジオメトリのエッジが実際の世界を満たしていることを認識します。 詳細が高いテクスチャを使用すると、この効果を aggravate ことができます。 テクスチャをマップし、フィルター処理を有効にする必要があります。 ホログラムの端をフェードさせるか、オブジェクトの周囲に黒いエッジ境界線を作成するテクスチャを追加することを検討してください。 可能であれば、thin geometry は避けてください。
* **アルファチャネル**-ホログラムをレンダリングしていないすべてのパーツに対して完全に透明にするには、アルファチャネルをクリアする必要があります。 アルファを未定義のままにすると、デバイスまたは Spectator ビューで画像やビデオを撮影するときに、視覚的な成果物が得られます。
* **テクスチャの補正**-ライトは holographic ディスプレイで追加されるため、視覚効果が意図せずになることが多いため、明るい色の広い領域を避けることをお勧めします。

## <a name="storytelling-with-light-and-color"></a>薄いと color を使用したストーリーテリング

色と色を使用すると、ユーザーの環境でのホログラムの表示がより自然になり、ユーザーのためのガイダンスやヘルプが提供されます。 Holographic エクスペリエンスについては、照明と色を調べる際に、次の要因を考慮してください。

:::row:::
    :::column:::
* **Vignetting** -マテリアルを暗くするための ' vignette ' 効果は、ビューのフィールドの中央にユーザーの注意を集中するのに役立ちます。 この効果により、ユーザーの見つめベクターから、ある程度の半径でホログラムのマテリアルが暗くなります。 これは、ユーザーのビューが斜投影または glancing 角度からホログラムを持つ場合にも有効であることに注意してください。<br>
* **強調**描画: 色、明るさ、および光源を比較して、オブジェクトまたは相互作用のポイントに注目します。 ストーリーテリングの照明メソッドの詳細については、「[ピクセル Cinematography-コンピューターグラフィックスの照明アプローチ](http://media.siggraph.org/education/cgsource/Archive/ConfereceCourses/S96/course30.pdf)」を参照してください。<br>
        <br>
        *Image: 色を使用してストーリーテリング要素の強調を表示します。ここでは、[フラグメント](https://www.microsoft.com/p/fragments/9nblggh5ggm8)からのシーンに示します。*
    :::column-end:::
        :::column:::
        ![色を使用して、ストーリーテリング要素の強調を表示します。ここでは、フラグメントからのシーンに示します。](images/640px-fragments.jpg)<br>
    :::column-end:::
:::row-end:::


<br>

---

## <a name="see-also"></a>関連項目
* [色の分離](hologram-stability.md#color-separation)
* [ホログラム](hologram.md)
* [Microsoft デザイン言語-色](https://www.microsoft.com/design/color)
* [ユニバーサル Windows プラットフォーム-色](https://docs.microsoft.com/windows/uwp/style/color)
