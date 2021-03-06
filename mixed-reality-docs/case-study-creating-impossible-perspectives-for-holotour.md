---
title: ケーススタディ-HoloTour の不可能なパースペクティブの作成
description: Microsoft HoloLens を unforgettable にするための HoloTour の経験が必要でした。 従来の tourist の停止に加えて、"不可能なパースペクティブ" を計画しています。
author: dannyaskew
ms.author: daaske
ms.date: 03/21/2018
ms.topic: article
keywords: HoloTour、HoloLens、Windows Mixed Reality
ms.openlocfilehash: f3ca07dfab1e4477039481c268e418aac9034bc5
ms.sourcegitcommit: d6ac8f1f545fe20cf1e36b83c0e7998b82fd02f8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/14/2020
ms.locfileid: "81278170"
---
# <a name="case-study---creating-impossible-perspectives-for-holotour"></a>ケーススタディ-HoloTour の不可能なパースペクティブの作成

Microsoft HoloLens を unforgettable にするための HoloTour の経験が必要でした。 従来の tourist の停止に加えて、私たちは、どのツアーでも経験できない "不可能なパースペクティブ" を計画しました。これは、HoloLens のテクノロジを通じて、リビングルームに直接持ち込むことができます。 これらのエクスペリエンスのためにコンテンツを作成するには、標準的なキャプチャプロセスとは異なる手法が必要です。

## <a name="the-content-challenge"></a>コンテンツチャレンジ

HoloTour 経験には特定のシーンがあります。たとえば、現代のローマでのホットエアバルーンや、古代ローマの Colosseum での gladiatorial 戦いなどがあります。これにより、他の場所に表示されない一意のビューが提供されます。 この瞬間は、楽しみな気持ちと amaze によって、単なる教育を経験するだけでなく、HoloTour を通じて旅行を行うことを目的としています。 これらは、覚えておく必要がある瞬間で、他のユーザーに知らせたいと思います。 カメラのマシンを空にすることはできなかったので、タイムトラベルをまだ行っていないので、これらの "不可能なパースペクティブ" は、コンテンツを作成するための特別なアプローチと呼ばれていました。

## <a name="behind-the-scenes"></a>しくみ

これらの一意の瞬間とパースペクティブを作成するには、撮影と編集だけではありません。 非常に多くの時間がかかっており、さまざまなスキルを持つ方が、Hollywood マジックも少しでした。

### <a name="viewing-rome-from-a-hot-air-balloon"></a>ホットエアバルーンからのローマの表示

初期の計画日から、HoloTour で航空写真ビューを行うことにしました。 空のローマを見ると、ほとんどの人が見られることがなく、人気のあるランドマークが空間的に配置されることがわかります。 既存のカメラとマイクのマシン群を使用してこれをキャプチャしようとすると、非常に困難になりましたが、幸運なことはありませんでした。

まず、HoloTour でアクセスするすべての場所に移動することを説明することが重要です。 私たちの目標は、"私たちは本当に私たちのような感覚を持っています" ということでした。また、実際にはどこでも動きが出てきています。これは、お客様の環境の動きも伝達するために必要な仮想ターゲットです。 たとえば、旅行の Pantheon にアクセスすると、通常と congregating の間に公があることがわかります。 バックグラウンドの動きは、ステージングされた静的な環境ではなく、場所を実際に使用しているように感じます。

気球の航空写真ビューを作成するために、Microsoft の他のチームと協力して、ローマの航空写真パノラマ画像にアクセスできるようにしました。 これらの画像の品質は非常に優れており、ビューは美しいものでしたが、変更せずにバックグラウンドで使用した場合、lifeless はツアーの他の部分と比較して、動きが邪魔にならないと感じていました。 


ローマに対してフリーエアバルーンバスケットを ![します。](images/hotairballoon1-300px.png)<br>
*ホットエアバルーンバスケット、ローマでフローティング*

航空写真の場所が他の変換先と同じ品質基準を満たしていることを確認するために、静的な写真を生きた動きに変換することにしました。 最初のステップでは、イメージと複合モーションを編集しました。 私たちは、これを支援する視覚効果のアーティストを契約しています。 編集が完了し、雲のません、鳥の鳥、地平線を飛ぶことのない飛行機やヘリコプターを表示するようになりました。 グラウンドでは、自動車の通りに車が作られました。 HoloTour のローマのツアーにいたとしても、このような動きを明示的に認識していることはほとんどありません。 これは本当にすばらしいです。 微妙な動きは目を引くことを意図したものではありませんが、これらの小さなタッチを使用しないと、シーン内の静的なイメージであることがすぐにわかります。

2番目に、シーンを表示する視点ポイントを提供しました。 実際には、自然な空気で浮動小数点演算が行われているように見えても、気球の3D モデルを作成してその中に配置しました。 これにより、バルーンを見て、より優れた視点を得ることができます。 航空画像を快適に体験できるように、これは自然で楽しい方法であることがわかりました。

ホットエアバルーンエクスペリエンスでは、オーディオチームに固有の課題が示されていました。これにより、飛行機はマイクを使ってローマを足するのを防ぐことができました。 さいわい、実稼働後に使用できるようになった都市全体から、大量のアンビエントオーディオキャプチャを利用できました。 オーディオ発信器は、どこからでも、そこから離れた場所に配置されました。 次に、ホットエアバルーンに含まれている他のユーザーの視点から聞こえたかのように、オーディオは遠く離れた状態のサウンドにフィルターされ、シーンに対して信頼性の高い指向性 soundscape が提供されます。

### <a name="time-traveling-to-ancient-rome"></a>古代ローマへの移動時間

ローマにおけるモニュメントや建物の残りの部分は、建築の後、2000年でも素晴らしいと思いますが、私たちは、時間の前に戻ってきたように、これらの構造を実際のローマで見てきたようなものを示しています。

当然ながら、Colosseum の作成時にはビデオ映像や静止画像が表示されないため、独自に作成する必要がありました。 このような構造については、多くの調査を行う必要がありました。仮想再作成を可能にするために必要な情報を得るために、作成されたマテリアルを理解し、アーキテクチャ図を確認し、履歴の説明を読みます。 

Colosseum の最新の日の廃墟を ![します。これは、古代ローマで見たように、のフロアフロアを示すオーバーレイを備えています。](images/rome-colosseum-overlay-500px.png)<br>
*古代ローマで見たように、Colosseum の最新の日の廃墟には、その分野を示すオーバーレイがあります。*

まず、教育オーバーレイを使用して従来のツアーを強化したいと考えていました。 HoloTour では、現在の Colosseum の廃墟にアクセスするときに、複雑な地下のステージング領域を含め、使用中にどのように表示されるかを示すために、領域フロアが変換されます。 通常のツアーでは、この情報が説明されている場合がありますが、想像してみてください。しかし、HoloTour では実際に見ることができます。

このようなオーバーレイについては、アーティストがキャプチャ映像の視点に一致し、手動でオーバーレイイメージを作成しています。 ビデオをイメージに置き換えると、両方が適切にアラインされるように、パースペクティブが一致している必要があります。

### <a name="staging-the-gladiator-fight"></a>Gladiator 戦いのステージング

オーバーレイを使用すると、履歴についてユーザーを教えることができますが、最も興奮していたのは時間が戻ってきたということです。 オーバーレイは特定の視点からの静止画像にすぎませんが、時間の移動には Colosseum 全体をモデル化する必要があります。前述のように、シーンで動作させる必要がありました。 これを実現するには、かなりの労力がかかっています。

この作業は、チームにとっては大きすぎて実行できませんでした。私たちの芸術チームは、通常、Hollywood 映画の視覚効果を発揮する外部の影響会社である Whiskytree を使用しました。 Whiskytree は、Colosseum を heyday に再作成するのに役立ちました。これにより、その構造について説明し、その中でも、皇帝の箱から gladiator 戦いのビューを作成できるようになりました。 声援 crowds とは、画像だけでなく実際の場所であるようにするために必要な微妙な動きを追加します。

Colosseum のフロアから見たように、再作成されたを ![します。 HoloTour で見ると、バナーは簡単に見えて、動きが見やすくなります。](images/recreated-colosseum-holotour-500px.png)<br>
*Colosseum のフロアから見たように、再作成された。HoloTour で見ると、バナーは簡単に見えて、動きが見やすくなります。*

Gladiator 戦いでのローマ culminates のツアー。 Whiskytree はビデオとしてレンダリングされた [gladiators] と [3D] の各シミュレーションを提供しましたが、それを追加する必要がありました。 プロセスのこの部分は、育成 game studio のプロジェクトと比べて、Hollywood ビデオ制作のようなものです。 私たちのチームのメンバーは、大まかな戦いのシーケンスをマップし、choreographer で改良しています。 私たちは、モックの戦いと購入した防御力をステージするためにアクターを採用しています。 最後に、シーン全体を緑色の画面に客席します。

gladiators を ![、の手順を取得します。](images/green-screen-gladiators-holotour-500px.jpg)<br>
*私たちの gladiatiors、手順の取得*

このシーンを使用すると、皇帝の箱が表示されます。これは、すべての映像がその観点から必要であるということを意味します。 私たちは、gladiators が客席にいる場所から、戦いの流れを適切に合成することはできませんでした。そのため、カメラのオペレータを非常に高度なシザーリフトにし、撮影の戦いシーケンスを見ていきます。

撮影を適切に取得するには、シザーリフトからします。 ![](images/scissor-lift-holotour-500px.jpg)<br>
*適切なパースペクティブの取得: シザーリフトからの撮影*

実稼働後、gladiators は領域フロアに合成されていますが、パースペクティブは正しいのですが、1つの問題が残っています。複合プロセスの一環として、緑色の画面の gladiators の影が削除されました。 シャドウを使用しない場合、gladiators は空気で浮動のように見えました。 幸運にも、Whiskytree はこのような問題を解決するのに非常に優れており、少しの技術 wizardry を使って、シーンに影を追加していました。 このツアーでは、結果が表示されます。

## <a name="about-the-authors"></a>作成者について

<table style="border:0">
<tr>
<td style="border:0" width="60px"> <img alt="David Haley" width="60" height="60" src="images/haley.png" /></td>
<td style="border:0" width="408"> <b>David Haley</b>は、HoloTour での作業によって可能であると考えられるカメラのマシンとビデオの再生について詳しく学習したシニア開発者です。</td>

<td style="border:0" width="60px"> <img alt="Jason Syltebo" width="60" height="60" src="images/syltebo.png" /></td>
<td style="border:0" width="408"> <b>Jason Syltebo</b>は、時間が戻った場合でも、アクセスするすべての宛先の soundscape を体験できるようにするオーディオデザイナーです。</td>
</tr>
<tr>
<td style="border:0" width="60px"> <img alt="Danny Askew" width="60" height="60" src="images/askew.png" /></td>
<td style="border:0" width="408"> <b>Danny Askew</b>は、ローマの旅をできるだけ完璧にしたビデオアーティストです。</td>

<td style="border:0" width="60px"></td>
<td style="border:0" width="408"></td>
</tr>
</table>


## <a name="see-also"></a>参照
* [ビデオ: Microsoft HoloLens: HoloTour](https://www.youtube.com/watch?v=pLd9WPlaMpY)
