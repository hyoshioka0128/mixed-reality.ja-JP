---
title: 旧暦モジュール
description: LunarModule は、Microsoft の混合現実デザイン ラボからオープン ソースのサンプル アプリです。 このプロジェクトでは、Hololens の基本のジェスチャ両手の追跡を拡張する方法を学習でき、Xbox コント ローラーは、入力、サーフェスのマッピングと平面の検索を事後対応型であるオブジェクトを作成し、単純なメニュー システムを実装します。
author: radicalad
ms.author: adlinv
ms.date: 03/21/2018
ms.topic: article
keywords: Windows が実際には、サンプル アプリを設計、HoloLens の混在
ms.openlocfilehash: 38f70d78b5572930b874e221fa4a85572c07b342
ms.sourcegitcommit: 384b0087899cd835a3a965f75c6f6c607c9edd1b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/12/2019
ms.locfileid: "59602484"
---
# <a name="lunar-module"></a><span data-ttu-id="46f14-105">旧暦モジュール</span><span class="sxs-lookup"><span data-stu-id="46f14-105">Lunar Module</span></span>

>[!NOTE]
><span data-ttu-id="46f14-106">この資料で説明した調査用のサンプル、[混合現実デザイン Labs](https://github.com/Microsoft/MRDesignLabs_Unity)、について学習したことを共有の場所とに関する推奨事項は、現実アプリ開発を混在させます。</span><span class="sxs-lookup"><span data-stu-id="46f14-106">This article discusses an exploratory sample we’ve created in the [Mixed Reality Design Labs](https://github.com/Microsoft/MRDesignLabs_Unity), a place where we share our learnings about and suggestions for mixed reality app development.</span></span> <span data-ttu-id="46f14-107">新しい検出を行ったときは、デザインに関連する記事とコードも進化します。</span><span class="sxs-lookup"><span data-stu-id="46f14-107">Our design-related articles and code will evolve as we make new discoveries.</span></span>

<span data-ttu-id="46f14-108">[旧暦モジュール](https://github.com/Microsoft/MRDesignLabs_Unity_LunarModule)はオープン ソースのサンプル アプリの Microsoft の混合現実デザイン ラボからです。</span><span class="sxs-lookup"><span data-stu-id="46f14-108">[Lunar Module](https://github.com/Microsoft/MRDesignLabs_Unity_LunarModule) is an open-source sample app from Microsoft's Mixed Reality Design Labs.</span></span> <span data-ttu-id="46f14-109">このプロジェクトでは、Hololens の基本のジェスチャ両手の追跡を拡張する方法を学習でき、Xbox コント ローラーは、入力、サーフェスのマッピングと平面の検索を事後対応型であるオブジェクトを作成し、単純なメニュー システムを実装します。</span><span class="sxs-lookup"><span data-stu-id="46f14-109">With this project, you can learn how to extend Hololens' base gestures with two-handed tracking and Xbox controller input, create objects that are reactive to surface mapping and plane finding and implement simple menu systems.</span></span> <span data-ttu-id="46f14-110">すべてのプロジェクトのコンポーネントは、独自の複合現実アプリ エクスペリエンスで使用できます。</span><span class="sxs-lookup"><span data-stu-id="46f14-110">All of the project's components are available for use in your own mixed reality app experiences.</span></span>

## <a name="rethinking-classic-experiences-for-windows-mixed-reality"></a><span data-ttu-id="46f14-111">Windows Mixed Reality をクラシックのエクスペリエンスについての再考</span><span class="sxs-lookup"><span data-stu-id="46f14-111">Rethinking classic experiences for Windows Mixed Reality</span></span>

<span data-ttu-id="46f14-112">大気で上位に Apollo モジュールの小規模の出荷はジャグ地形の下系統的に調査します。</span><span class="sxs-lookup"><span data-stu-id="46f14-112">High up in the atmosphere, a small ship reminiscent of the Apollo module methodically surveys jagged terrain below.</span></span> <span data-ttu-id="46f14-113">これほど大胆なパイロットは spots 着陸に適した領域です。</span><span class="sxs-lookup"><span data-stu-id="46f14-113">Our fearless pilot spots a suitable landing area.</span></span> <span data-ttu-id="46f14-114">降下は困難ですが、さいわいにも、この体験は前に何度も.</span><span class="sxs-lookup"><span data-stu-id="46f14-114">The descent is arduous but thankfully, this journey has been made many times before...</span></span>

<span data-ttu-id="46f14-115">![Atari の 1979 太陰暦 Lander から元のインターフェイス](images/640px-atari-lunar-lander.png)</span><span class="sxs-lookup"><span data-stu-id="46f14-115">![Original interface from Atari’s 1979 Lunar Lander](images/640px-atari-lunar-lander.png)</span></span><br>
<span data-ttu-id="46f14-116">*Atari の 1979 太陰暦 Lander から元のインターフェイス*</span><span class="sxs-lookup"><span data-stu-id="46f14-116">*Original interface from Atari’s 1979 Lunar Lander*</span></span>

<span data-ttu-id="46f14-117">[太陰暦 Lander](https://en.wikipedia.org/wiki/Lunar_Lander_(1979_video_game))アーケード クラシックは、プレイヤーが太陰暦地形のフラットなスポットにザ ムーン lander の試験的導入しようとします。</span><span class="sxs-lookup"><span data-stu-id="46f14-117">[Lunar Lander](https://en.wikipedia.org/wiki/Lunar_Lander_(1979_video_game)) is an arcade classic where players attempt to pilot a moon lander onto a flat spot of lunar terrain.</span></span> <span data-ttu-id="46f14-118">接着 plummeting 空からこのベクトル船をその目で、アーケード ゲームで時間を費やして、1970 年代に生まれた人がほとんどの場合。</span><span class="sxs-lookup"><span data-stu-id="46f14-118">Anyone born in the 1970s has most likely spent hours in an arcade with their eyes glued to this vector ship plummeting from the sky.</span></span> <span data-ttu-id="46f14-119">プレイヤーが目的のランディング領域に向かって、出荷を移動したときに、地形を徐々 に詳細な内容をスケーリングします。</span><span class="sxs-lookup"><span data-stu-id="46f14-119">As a player navigates their ship toward a desired landing area the terrain scales to reveal progressively more detail.</span></span> <span data-ttu-id="46f14-120">成功した場合は、水平および垂直の速度の安全なしきい値の範囲内のランディングを意味します。</span><span class="sxs-lookup"><span data-stu-id="46f14-120">Success means landing within the safe threshold of horizontal and vertical speed.</span></span> <span data-ttu-id="46f14-121">ランディングと残りの fuel, ランディング領域のサイズに基づく乗数で費やされた時間のポイントが授与されます。</span><span class="sxs-lookup"><span data-stu-id="46f14-121">Points are awarded for time spent landing and remaining fuel, with a multiplier based on the size of the landing area.</span></span>

<span data-ttu-id="46f14-122">別に、ゲームプレイのゲームのアーケード時代 (年号) はコントロール パターンの定数の革新になります。</span><span class="sxs-lookup"><span data-stu-id="46f14-122">Aside from the gameplay, the arcade era of games brought constant innovation of control schemes.</span></span> <span data-ttu-id="46f14-123">最も簡単な 4 ウェイのジョイスティックとボタンの構成から (、アイコンで表示される[パックマン](https://en.wikipedia.org/wiki/Pac-Man))、90 年代と 00 秒 (ゴルフ シミュレーターとレール連発銃など) に非常に複雑な特定のスキームにします。</span><span class="sxs-lookup"><span data-stu-id="46f14-123">From the simplest 4-way joystick and button configurations (seen in the iconic [Pac-Man](https://en.wikipedia.org/wiki/Pac-Man)) to the highly specific and complicated schemes seen in the late 90s and 00s (like those in golf simulators and rail shooters).</span></span> <span data-ttu-id="46f14-124">2 つの理由、太陰暦 Lander マシンで使用される入力のスキームは特に興味深い: 魅力と immersion を援助します。</span><span class="sxs-lookup"><span data-stu-id="46f14-124">The input scheme used in the Lunar Lander machine is particularly intriguing for two reasons: curb appeal and immersion.</span></span>

<span data-ttu-id="46f14-125">![Atari の太陰暦 Lander のアーケード ゲーム コンソール](images/atariconsole.png)</span><span class="sxs-lookup"><span data-stu-id="46f14-125">![Atari’s Lunar Lander’s arcade console](images/atariconsole.png)</span></span><br>
<span data-ttu-id="46f14-126">*Atari の太陰暦 Lander アーケード ゲーム コンソール*</span><span class="sxs-lookup"><span data-stu-id="46f14-126">*Atari's Lunar Lander arcade console*</span></span>

<span data-ttu-id="46f14-127">理由でした Atari と他の多くのゲーム会社する入力を再考しますか。</span><span class="sxs-lookup"><span data-stu-id="46f14-127">Why did Atari and so many other game companies decide to rethink input?</span></span>

<span data-ttu-id="46f14-128">最新、flashiest マシンで、アーケード ゲームで子供をそそら自然されます。</span><span class="sxs-lookup"><span data-stu-id="46f14-128">A kid walking through an arcade will naturally be intrigued by the newest, flashiest machine.</span></span> <span data-ttu-id="46f14-129">ただし、太陰暦 Lander 抜きん出るきませんを整備士斬新な入力の機能します。</span><span class="sxs-lookup"><span data-stu-id="46f14-129">But Lunar Lander features a novel input mechanic that stood out from the crowd.</span></span>

<span data-ttu-id="46f14-130">太陰暦 Lander 左と右の出荷を回転させる 2 つのボタンを使用して、**スロットル レバー**宇宙船の生成のスロットルの量を制御します。</span><span class="sxs-lookup"><span data-stu-id="46f14-130">Lunar Lander uses two buttons for rotating the ship left and right and a **thrust lever** to control the amount of thrust the ship produces.</span></span> <span data-ttu-id="46f14-131">このレバーが使用するユーザー、一定レベルの見栄えが正規のジョイスティックを提供できません。</span><span class="sxs-lookup"><span data-stu-id="46f14-131">This lever gives users a certain level of finesse a regular joystick can’t provide.</span></span> <span data-ttu-id="46f14-132">またに最新の航空コックピットに共通のコンポーネントになります。</span><span class="sxs-lookup"><span data-stu-id="46f14-132">It is also happens to be a component common to modern aviation cockpits.</span></span> <span data-ttu-id="46f14-133">Atari 太陰暦 Lander を感じ旧暦モジュール実際されたパイロットことで、ユーザーが利用できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="46f14-133">Atari wanted Lunar Lander to immerse the user in the feeling that they were in fact piloting a lunar module.</span></span> <span data-ttu-id="46f14-134">この概念と呼ばれる**触る immersion**します。</span><span class="sxs-lookup"><span data-stu-id="46f14-134">This concept is known as **tactile immersion**.</span></span>

<span data-ttu-id="46f14-135">触る immersion は、繰り返し実行するアクションの実行からの感覚フィードバックのエクスペリエンスです。</span><span class="sxs-lookup"><span data-stu-id="46f14-135">Tactile immersion is the experience of sensory feedback from performing repetitious actions.</span></span> <span data-ttu-id="46f14-136">ここでは、繰り返し発生するアクション スロットル レバーと人間の目を参照してくださいし、言われるでしょうが紹介されている回転を調整するのでは、月面着陸船の動作をプレーヤーを接続することができます。</span><span class="sxs-lookup"><span data-stu-id="46f14-136">In this case, the repetitive action of adjusting the throttle lever and rotation which our eyes see and our ears hear, helps connect the player to the act of landing a ship on the moon’s surface.</span></span> <span data-ttu-id="46f14-137">この概念は「フロー」という心理的な概念に関係していることができます。</span><span class="sxs-lookup"><span data-stu-id="46f14-137">This concept can be tied to the psychological concept of "flow."</span></span> <span data-ttu-id="46f14-138">""ゾーンにいるユーザーが完全に課題と報酬の適切な組み合わせを持つタスクで吸収またはより簡単に言えば、</span><span class="sxs-lookup"><span data-stu-id="46f14-138">Where a user is fully absorbed in a task that has the right mixture of challenge and reward, or put more simply, they’re “in the zone.”</span></span>

<span data-ttu-id="46f14-139">おそらく、immersion 複合現実での最も顕著な型では、空間 immersion です。</span><span class="sxs-lookup"><span data-stu-id="46f14-139">Arguably, the most prominent type of immersion in mixed reality is spatial immersion.</span></span> <span data-ttu-id="46f14-140">複合現実の本質は、これらを利用して、自分たちが現実の世界にデジタル オブジェクトが存在します。</span><span class="sxs-lookup"><span data-stu-id="46f14-140">The whole point of mixed reality is to fool ourselves into believing these digital objects exist in the real world.</span></span> <span data-ttu-id="46f14-141">環境全体でエクスペリエンスが空間的に専念した状態、自分の周りホログラムを合成しています。</span><span class="sxs-lookup"><span data-stu-id="46f14-141">We’re synthesizing holograms in our surroundings, spatially immersed in entire environments and experiences.</span></span> <span data-ttu-id="46f14-142">これは、ことはできませんが採用しています immersion の他の種類、経験に Atari 太陰暦 Lander に触る immersion のと同様は限りません。</span><span class="sxs-lookup"><span data-stu-id="46f14-142">This doesn’t mean we can’t still employ other types of immersion in our experiences just as Atari did with tactile immersion in Lunar Lander.</span></span>

## <a name="designing-with-immersion"></a><span data-ttu-id="46f14-143">Immersion の設計</span><span class="sxs-lookup"><span data-stu-id="46f14-143">Designing with immersion</span></span>

<span data-ttu-id="46f14-144">クラシック Atari に、更新、帯域幅消費型続編に触る immersion を適用しますにはどう可能性がありますでしょうか。</span><span class="sxs-lookup"><span data-stu-id="46f14-144">How might we apply tactile immersion to an updated, volumetric sequel to the Atari classic?</span></span> <span data-ttu-id="46f14-145">入力のスキームを使って取り組むには、前に、3 次元空間のゲームのコンストラクトは、対処する必要があります。</span><span class="sxs-lookup"><span data-stu-id="46f14-145">Before tackling the input scheme, the game construct for 3-dimensional space needs to be addressed.</span></span>

<span data-ttu-id="46f14-146">![HoloLens のサーフェス マップの視覚化](images/surfacemapping.png)</span><span class="sxs-lookup"><span data-stu-id="46f14-146">![Visualizing surface mapping in HoloLens](images/surfacemapping.png)</span></span><br>
<span data-ttu-id="46f14-147">*HoloLens で空間のマッピングを視覚化します。*</span><span class="sxs-lookup"><span data-stu-id="46f14-147">*Visualizing spatial mapping in HoloLens*</span></span>

<span data-ttu-id="46f14-148">ユーザーの環境を活用することによって、旧暦モジュールのランディング無限地形オプションがあります効果的に。</span><span class="sxs-lookup"><span data-stu-id="46f14-148">By leveraging a user’s surroundings, we effectively have infinite terrain options for landing our lunar module.</span></span> <span data-ttu-id="46f14-149">ゲームを元のタイトルなどほとんどさせるには、ユーザーが可能性のある操作し、環境内でさまざまな問題がある場合のランディング パッドを配置する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="46f14-149">To make the game most like the original title, a user could potentially manipulate and place landing pads of varying difficulties in their environment.</span></span>

<span data-ttu-id="46f14-150">![飛行、](images/640px-lm-hero.jpg)</span><span class="sxs-lookup"><span data-stu-id="46f14-150">![Flying the Lunar Module](images/640px-lm-hero.jpg)</span></span><br>
<span data-ttu-id="46f14-151">*飛行、*</span><span class="sxs-lookup"><span data-stu-id="46f14-151">*Flying the Lunar Module*</span></span>

<span data-ttu-id="46f14-152">質問の多くは、入力の構成について説明します、船を制御およびを小規模のターゲットが存在するように求めます。</span><span class="sxs-lookup"><span data-stu-id="46f14-152">Requiring the user to learn the input scheme, control the ship, and have a small target to land on is a lot to ask.</span></span> <span data-ttu-id="46f14-153">チャレンジと報酬のバランスを適正化機能のゲーム エクスペリエンスの成功します。</span><span class="sxs-lookup"><span data-stu-id="46f14-153">A successful game experience features the right mix of challenge and reward.</span></span> <span data-ttu-id="46f14-154">ユーザーの難易度のレベルを選択することができるよう、最も簡単なモードでは、HoloLens でスキャン単に画面でユーザー定義領域で正常に配置する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="46f14-154">The user should be able to choose a level of difficulty, with the easiest mode simply requiring the user to successfully land in a user-defined area on a surface scanned by the HoloLens.</span></span> <span data-ttu-id="46f14-155">ユーザーは、ゲームのハングを取得、合ったの難しさをし crank ができます。</span><span class="sxs-lookup"><span data-stu-id="46f14-155">Once a user gets the hang of the game, they can then crank up the difficulty as they see fit.</span></span>

### <a name="adding-input-for-hand-gestures"></a><span data-ttu-id="46f14-156">手のジェスチャの入力を追加します。</span><span class="sxs-lookup"><span data-stu-id="46f14-156">Adding input for hand gestures</span></span>

<span data-ttu-id="46f14-157">HoloLens の基本入力が 2 つのジェスチャの[エア タップとブルーム](gestures.md)します。</span><span class="sxs-lookup"><span data-stu-id="46f14-157">HoloLens base input has only two gestures - [Air Tap and Bloom](gestures.md).</span></span> <span data-ttu-id="46f14-158">ユーザーは、コンテキストの微妙な差異や習得しやすいと汎用性の高いプラットフォームのインターフェイスを特定のジェスチャの洗濯物の一覧を覚える必要はありません。</span><span class="sxs-lookup"><span data-stu-id="46f14-158">Users don’t need to remember contextual nuances or a laundry list of specific gestures which makes the platform’s interface both versatile and easy to learn.</span></span> <span data-ttu-id="46f14-159">システムでは、これら 2 つのジェスチャが公開される可能性がのみ、HoloLens デバイスとしては、一度に 2 つの手を追跡できます。</span><span class="sxs-lookup"><span data-stu-id="46f14-159">While the system may only expose these two gestures, HoloLens as a device is capable of tracking two hands at once.</span></span> <span data-ttu-id="46f14-160">太陰暦 Lander に、コードは、[イマーシブ アプリ](app-model.md)を 2 つの手を活用し、適切に触る独自旧暦モジュール ナビゲーション手段を追加するジェスチャの基本セットを拡張する機能があることを意味します。</span><span class="sxs-lookup"><span data-stu-id="46f14-160">Our ode to Lunar Lander is an [immersive app](app-model.md) which means we have the ability to extend the base set of gestures to leverage two hands and add our own delightfully tactile means for lunar module navigation.</span></span>

<span data-ttu-id="46f14-161">元のコントロール パターンを見る**のスロットルと回転を解決する必要がありました**します。</span><span class="sxs-lookup"><span data-stu-id="46f14-161">Looking back at the original control scheme, **we needed to solve for thrust and rotation**.</span></span> <span data-ttu-id="46f14-162">注意事項は、新しいコンテキストでの回転は追加の軸を追加します (技術的には 2 つが、Y 軸はランディングの重要度の低い)。</span><span class="sxs-lookup"><span data-stu-id="46f14-162">The caveat is rotation in the new context adds an additional axis (technically two, but the Y axis is less important for landing).</span></span> <span data-ttu-id="46f14-163">2 つの個別の出荷の動作では、各手の形にマップする自然適しています。</span><span class="sxs-lookup"><span data-stu-id="46f14-163">The two distinct ship movements naturally lend themselves to be mapped to each hand:</span></span>

<span data-ttu-id="46f14-164">![タップし、3 つすべての軸で lander を回転するジェスチャをドラッグします](images/module-handdrag.gif)</span><span class="sxs-lookup"><span data-stu-id="46f14-164">![Tap and drag gesture to rotate lander on all three axes](images/module-handdrag.gif)</span></span><br>
<span data-ttu-id="46f14-165">*タップし、3 つすべての軸で lander を回転するジェスチャをドラッグします*</span><span class="sxs-lookup"><span data-stu-id="46f14-165">*Tap and drag gesture to rotate lander on all three axes*</span></span>

<span data-ttu-id="46f14-166">**スロットル**</span><span class="sxs-lookup"><span data-stu-id="46f14-166">**Thrust**</span></span>

<span data-ttu-id="46f14-167">元のアーケード マシン上のレバー、値の小数点以下桁数にマップが大きいほど、レバー移動された複数のスロットルが配送先に適用されました。</span><span class="sxs-lookup"><span data-stu-id="46f14-167">The lever on the original arcade machine mapped to a scale of values, the higher the lever was moved the more thrust was applied to the ship.</span></span> <span data-ttu-id="46f14-168">ここで指摘して重要なに微妙な違いは、ユーザーはコントロールからその手の形を取得し、目的の値を維持します。</span><span class="sxs-lookup"><span data-stu-id="46f14-168">An important nuance to point out here is the user can take their hand off of the control and maintain a desired value.</span></span> <span data-ttu-id="46f14-169">タップ アンド ドラッグ動作を同じ結果を実現するために効果的に使用できます。</span><span class="sxs-lookup"><span data-stu-id="46f14-169">We can effectively use tap-and-drag behavior to achieve the same result.</span></span> <span data-ttu-id="46f14-170">スロットル値は、0 から始まります。</span><span class="sxs-lookup"><span data-stu-id="46f14-170">The thrust value starts at zero.</span></span> <span data-ttu-id="46f14-171">ユーザーは、タップ、ドラッグ、値を大きくします。</span><span class="sxs-lookup"><span data-stu-id="46f14-171">The user taps and drags to increase the value.</span></span> <span data-ttu-id="46f14-172">その時点でそれを維持するために移動できるように可能性があります。</span><span class="sxs-lookup"><span data-stu-id="46f14-172">At that point they could let go to maintain it.</span></span> <span data-ttu-id="46f14-173">ドラッグ アンド タップ ジェスチャの値の変更を元の値から差分となります。</span><span class="sxs-lookup"><span data-stu-id="46f14-173">Any tap-and-drag gesture value change would be the delta from the original value.</span></span>

<span data-ttu-id="46f14-174">**回転**</span><span class="sxs-lookup"><span data-stu-id="46f14-174">**Rotation**</span></span>

<span data-ttu-id="46f14-175">これは、少し注意が必要です。</span><span class="sxs-lookup"><span data-stu-id="46f14-175">This is a little more tricky.</span></span> <span data-ttu-id="46f14-176">「回転」holographic ボタンの悲惨な経験をタップすることです。</span><span class="sxs-lookup"><span data-stu-id="46f14-176">Having holographic “rotate” buttons to tap makes for a terrible experience.</span></span> <span data-ttu-id="46f14-177">動作は、操作、lander を表すオブジェクトのまたは lander 自体から取得する必要があるために、これを活用すると、物理的な制御が用意されていません。</span><span class="sxs-lookup"><span data-stu-id="46f14-177">There isn’t a physical control to leverage, so the behavior must come from manipulation of an object representing the lander, or with the lander itself.</span></span> <span data-ttu-id="46f14-178">そうタップ アンド ドラッグこれにより、ユーザーを効果的に「プッシュし、プル」を使用して、メソッドと、その方向にすることに直面します。</span><span class="sxs-lookup"><span data-stu-id="46f14-178">We came up with a method using tap-and-drag which enables a user to effectively “push and pull” it in the direction they want it to face.</span></span> <span data-ttu-id="46f14-179">いつでも、ユーザーがタップし、保持、ジェスチャが開始された空間内の点の回転を基準となります。</span><span class="sxs-lookup"><span data-stu-id="46f14-179">Any time a user taps and holds, the point in space where the gesture was initiated becomes the origin for rotation.</span></span> <span data-ttu-id="46f14-180">配信元からドラッグすることでは、手の形の変換 (X, Y, Z) のデルタを変換し、lander の回転値のデルタに適用されます。</span><span class="sxs-lookup"><span data-stu-id="46f14-180">Dragging from the origin converts the delta of the hand's translation (X,Y,Z) and applies it to the delta of the lander's rotation values.</span></span> <span data-ttu-id="46f14-181">または、単により*船を適宜回転スペースに戻り、左 <> - 右、上 <> - 下]、[進む <> - をドラッグ*します。</span><span class="sxs-lookup"><span data-stu-id="46f14-181">Or more simply, *dragging left <-> right, up <-> down, forward <-> back in spaces rotates the ship accordingly*.</span></span>

<span data-ttu-id="46f14-182">HoloLens が 2 つの手を追跡するため、スロットルが左によって制御されるときに回転は、右側に割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="46f14-182">Since the HoloLens can track two hands, rotation can be assigned to the right hand while thrust is controlled by the left.</span></span> <span data-ttu-id="46f14-183">見栄えは、このゲームで成功の推進要因です。</span><span class="sxs-lookup"><span data-stu-id="46f14-183">Finesse is the driving factor for success in this game.</span></span> <span data-ttu-id="46f14-184">*感じ*絶対の最も高い優先順位は、これらの相互作用の。</span><span class="sxs-lookup"><span data-stu-id="46f14-184">The *feel* of these interactions is the absolute highest priority.</span></span> <span data-ttu-id="46f14-185">特にで触る immersion のコンテキスト。</span><span class="sxs-lookup"><span data-stu-id="46f14-185">Especially in context of tactile immersion.</span></span> <span data-ttu-id="46f14-186">反応が早すぎる宇宙船を進めるために、遅すぎるの 1 つは、ユーザーを「プッシュとプル」が必要ですが、船転びに長い時間が不必要に困難になります。</span><span class="sxs-lookup"><span data-stu-id="46f14-186">A ship that reacts too quickly would be unnecessarily difficult to steer, while one too slow would require the user to “push and pull” on the ship for an awkwardly long amount of time.</span></span>

### <a name="adding-input-for-game-controllers"></a><span data-ttu-id="46f14-187">ゲーム コント ローラーの入力を追加します。</span><span class="sxs-lookup"><span data-stu-id="46f14-187">Adding input for game controllers</span></span>

<span data-ttu-id="46f14-188">粒度が細かいコントロールの新しいメソッドを提供する、HoloLens の手のジェスチャがありますアナログ コントロールから取得した触るフィードバックを 'true' の特定の欠如。</span><span class="sxs-lookup"><span data-stu-id="46f14-188">While hand gestures on the HoloLens provide a novel method of fine-grain control, there is still a certain lack of 'true' tactile feedback that you get from analog controls.</span></span> <span data-ttu-id="46f14-189">粒度が細かい制御を保持するコントロール スティックを活用しながら、この意味突き詰めるを戻すには、Xbox ゲーム コント ローラーを接続できます。</span><span class="sxs-lookup"><span data-stu-id="46f14-189">Connecting an Xbox game controller allows to bring back this sense of physicality while leveraging the control sticks to retain fine-grain control.</span></span>

<span data-ttu-id="46f14-190">比較的単純なコントロール パターン、Xbox コント ローラーに適用する複数の方法はあります。</span><span class="sxs-lookup"><span data-stu-id="46f14-190">There are multiple ways to apply the relatively straight-forward control scheme to the Xbox controller.</span></span> <span data-ttu-id="46f14-191">可能であれば、として設定する元のアーケード ゲームの近くに維持するため**押し付け**ベストをトリガー ボタンにマップされます。</span><span class="sxs-lookup"><span data-stu-id="46f14-191">Since we're trying to stay as close to the original arcade set up as possible, **Thrust** maps best to the trigger button.</span></span> <span data-ttu-id="46f14-192">これらのボタンはアナログ コントロールを持つ複数の単純な*オンとオフを*状態、それらの配置の負荷の度合いに実際に応答します。</span><span class="sxs-lookup"><span data-stu-id="46f14-192">These buttons are analog controls, meaning they have more than simple *on and off* states, they actually respond to the degree of pressure put on them.</span></span> <span data-ttu-id="46f14-193">同様の構成をこれにより、**スロットル レバー**します。</span><span class="sxs-lookup"><span data-stu-id="46f14-193">This gives us a similar construct as the **thrust lever**.</span></span> <span data-ttu-id="46f14-194">元のゲーム、手のジェスチャとは異なり、ユーザーは、トリガーでの負荷が増大が停止したら、このコントロールは、宇宙船のスロットルを切り取ります。</span><span class="sxs-lookup"><span data-stu-id="46f14-194">Unlike the original game and the hand gesture, this control will cut the ship's thrust once a user stops putting pressure on the trigger.</span></span> <span data-ttu-id="46f14-195">それでも、ユーザー同程度に見栄えは元のアーケード ゲームと同様です。</span><span class="sxs-lookup"><span data-stu-id="46f14-195">It still gives the user the same degree of finesse as the original arcade game did.</span></span>

<span data-ttu-id="46f14-196">![左のサムスティックがヨー、ロールにマップされて、適切なスティックがピッチし、ロールにマップされます。](images/thumbsticksidebyside.gif)</span><span class="sxs-lookup"><span data-stu-id="46f14-196">![Left thumbstick is mapped to Yaw and Roll, Right thumbstick is mapped to Pitch and Roll](images/thumbsticksidebyside.gif)</span></span><br>
<span data-ttu-id="46f14-197">*左のサムスティックがヨーし、; をロールにマップされます。右スティックがピッチし、ロールにマップされます。*</span><span class="sxs-lookup"><span data-stu-id="46f14-197">*Left thumbstick is mapped to yaw and roll; right thumbstick is mapped to pitch and roll*</span></span>

<span data-ttu-id="46f14-198">デュアル サムスティック自然を起こす出荷回転を制御します。</span><span class="sxs-lookup"><span data-stu-id="46f14-198">The dual thumbsticks naturally lend themselves to controlling ship rotation.</span></span> <span data-ttu-id="46f14-199">残念ながら、上にある 3 つの軸回転させることが、出荷と 2 つのサムスティック両方をサポートする 2 つの軸。</span><span class="sxs-lookup"><span data-stu-id="46f14-199">Unfortunately, there are 3 axes on which the ship can rotate and two thumbsticks which both support two axes.</span></span> <span data-ttu-id="46f14-200">この不一致はいずれか 1 つのサムスティック コントロール 1 つの軸。または、サムスティックの軸のオーバー ラップがあります。</span><span class="sxs-lookup"><span data-stu-id="46f14-200">This mismatch means either one thumbstick controls one axis; or there is overlap of axes for the thumbsticks.</span></span> <span data-ttu-id="46f14-201">前者の方法が得られるため、本質的に blend 自分のローカルのサムスティック"破損を示す"感じて X と Y の値。</span><span class="sxs-lookup"><span data-stu-id="46f14-201">The former solution ended up feeling "broken" since thumbsticks inherently blend their local X and Y values.</span></span> <span data-ttu-id="46f14-202">後者のソリューションが必要なテストを検索する冗長な軸が最も自然に感じです。</span><span class="sxs-lookup"><span data-stu-id="46f14-202">The latter solution required some testing to find which redundant axes feel the most natural.</span></span> <span data-ttu-id="46f14-203">最終的なサンプルを使用して*ヨー*と*ロール*(Y と X 軸) の左のサムスティック、および*ピッチ*と*ロール*(Z および X 軸) 右側のサムスティックです。</span><span class="sxs-lookup"><span data-stu-id="46f14-203">The final sample uses *yaw* and *roll* (Y and X axes) for the left thumbstick, and *pitch* and *roll* (Z and X axes) for the right thumbstick.</span></span> <span data-ttu-id="46f14-204">これは、感じてとして最も自然な*ロール*で個別にペアリングするよう*ヨー*と*ピッチ*します。</span><span class="sxs-lookup"><span data-stu-id="46f14-204">This felt the most natural as *roll* seems to independently pair well with *yaw* and *pitch*.</span></span> <span data-ttu-id="46f14-205">なお、使用の両方のサムスティック*ロール*回転の値を 2 倍にも行われますが非常に楽しく do ループ lander があります。</span><span class="sxs-lookup"><span data-stu-id="46f14-205">As a side note, using both thumbsticks for *roll* also happens to double the rotation value; it's pretty fun to have the lander do loops.</span></span>

<span data-ttu-id="46f14-206">このサンプル アプリはどのように空間認識を示し、触る immersion から Windows Mixed Reality の拡張可能な入力の様相に協力してくれた、エクスペリエンスを大幅に変更することができます。</span><span class="sxs-lookup"><span data-stu-id="46f14-206">This sample app demonstrates how spatial recognition and tactile immersion can significantly change an experience thanks to Windows Mixed Reality's extensible input modalities.</span></span> <span data-ttu-id="46f14-207">太陰暦 Lander を 40 年の時代に近づいている場合があります、中に、概念で公開される小さな八角形の区間が永続的な内で動作することです。</span><span class="sxs-lookup"><span data-stu-id="46f14-207">While Lunar Lander may be nearing 40 years in age, the concepts exposed with that little octagon-with-legs will live on forever.</span></span> <span data-ttu-id="46f14-208">将来を見直して、するときは、過去で確認できないでしょうか。</span><span class="sxs-lookup"><span data-stu-id="46f14-208">When imagining the future, why not look at the past?</span></span>

## <a name="technical-details"></a><span data-ttu-id="46f14-209">技術的な詳細</span><span class="sxs-lookup"><span data-stu-id="46f14-209">Technical details</span></span>

<span data-ttu-id="46f14-210">見つかりますスクリプトとプレハブ旧暦モジュールのサンプル アプリで、 [Mixed Reality デザイン Labs GitHub](https://github.com/Microsoft/MRDesignLabs_Unity_LunarModule)します。</span><span class="sxs-lookup"><span data-stu-id="46f14-210">You can find scripts and prefabs for the Lunar Module sample app on the [Mixed Reality Design Labs GitHub](https://github.com/Microsoft/MRDesignLabs_Unity_LunarModule).</span></span>

## <a name="about-the-author"></a><span data-ttu-id="46f14-211">執筆者紹介</span><span class="sxs-lookup"><span data-stu-id="46f14-211">About the author</span></span>

<table style="border-collapse:collapse" padding-left="0px">
<tr>
<td style="border-style: none" width="60"><img alt="Picture of Addison Linville" width="60" height="60" src="images/addisonlinville-tile-60px.jpg"></td>
<td style="border-style: none"><span data-ttu-id="46f14-212"><b>Addison リンビル</b></span><span class="sxs-lookup"><span data-stu-id="46f14-212"><b>Addison Linville</b></span></span><br><span data-ttu-id="46f14-213">UX デザイナー @Microsoft</span><span class="sxs-lookup"><span data-stu-id="46f14-213">UX Designer @Microsoft</span></span></td>
</tr>
</table>

## <a name="see-also"></a><span data-ttu-id="46f14-214">関連項目</span><span class="sxs-lookup"><span data-stu-id="46f14-214">See also</span></span>
* [<span data-ttu-id="46f14-215">アニメーション コント ローラー</span><span class="sxs-lookup"><span data-stu-id="46f14-215">Motion controllers</span></span>](motion-controllers.md)
* [<span data-ttu-id="46f14-216">ジェスチャ</span><span class="sxs-lookup"><span data-stu-id="46f14-216">Gestures</span></span>](gestures.md)
* [<span data-ttu-id="46f14-217">複合現実アプリの種類</span><span class="sxs-lookup"><span data-stu-id="46f14-217">Types of mixed reality apps</span></span>](types-of-mixed-reality-apps.md)