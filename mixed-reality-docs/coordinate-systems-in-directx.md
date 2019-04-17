---
title: DirectX の座標系
description: Windows Mixed Reality 空間ロケーター、参照フレーム、空間的なアンカーは、座標系を使用する方法、SpatialStage を使用する方法、追跡の損失を処理する方法、保存し、表現のアンカーを読み込む方法、および画像安定化する方法について説明します。
author: thetuvix
ms.author: alexturn
ms.date: 02/24/2019
ms.topic: article
keywords: 空間ロケーター、空間参照フレーム、空間座標系、空間ステージでは、実際には、混合コード、イメージの安定化、空間アンカー、空間アンカー ストア、追跡の損失、チュートリアルをサンプルします。
ms.openlocfilehash: c8cdb39cbf4634edb4ed0a595381fc70f1388ce4
ms.sourcegitcommit: f7fc9afdf4632dd9e59bd5493e974e4fec412fc4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2019
ms.locfileid: "59605151"
---
# <a name="coordinate-systems-in-directx"></a><span data-ttu-id="805cb-104">DirectX の座標系</span><span class="sxs-lookup"><span data-stu-id="805cb-104">Coordinate systems in DirectX</span></span>

<span data-ttu-id="805cb-105">[座標系](coordinate-systems.md)空間については、Windows Mixed Reality Api によって提供されるは、基礎を形成します。</span><span class="sxs-lookup"><span data-stu-id="805cb-105">[Coordinate systems](coordinate-systems.md) form the basis for spatial understanding offered by Windows Mixed Reality APIs.</span></span>

<span data-ttu-id="805cb-106">今日の取り付け VR または、単一ルーム VR デバイスは、追跡対象のスペースを表す 1 つのプライマリ座標システムを確立します。</span><span class="sxs-lookup"><span data-stu-id="805cb-106">Today's seated VR or single-room VR devices establish one primary coordinate system to represent their tracked space.</span></span> <span data-ttu-id="805cb-107">Windows Mixed Reality デバイスなど、HoloLens が定義されていない大規模な環境で使用するために設計されたデバイスを検出して、ユーザーとしてその周囲のものについて学習について説明しますの周り。</span><span class="sxs-lookup"><span data-stu-id="805cb-107">Windows Mixed Reality devices such as HoloLens are designed to be used throughout large undefined environments, with the device discovering and learning about its surroundings as the user walks around.</span></span> <span data-ttu-id="805cb-108">これにより、継続的に向上に合わせて、デバイス、ユーザーのルームが互いに、アプリの有効期間の関係を変更する座標システムでの結果に関する知識。</span><span class="sxs-lookup"><span data-stu-id="805cb-108">This allows the device to adapt to continually-improving knowledge about the user's rooms, but results in coordinate systems that will change their relationship to one another through the lifetime of the app.</span></span> <span data-ttu-id="805cb-109">Windows Mixed Reality は、幅広いデバイス、イマーシブ ヘッドセットが取り付けられていないから世界に接続された参照フレームまでをサポートします。</span><span class="sxs-lookup"><span data-stu-id="805cb-109">Windows Mixed Reality supports a wide spectrum of devices, ranging from seated immersive headsets through world-attached reference frames.</span></span>

>[!NOTE]
><span data-ttu-id="805cb-110">この記事のコード スニペットは現在の使用を示すC++/CX ではなく c++ 17 に準拠していませんC++/WinRT で使用するため、 [ C++ holographic プロジェクト テンプレート](creating-a-holographic-directx-project.md)します。</span><span class="sxs-lookup"><span data-stu-id="805cb-110">The code snippets in this article currently demonstrate use of C++/CX rather than C++17-compliant C++/WinRT as used in the [C++ holographic project template](creating-a-holographic-directx-project.md).</span></span>  <span data-ttu-id="805cb-111">概念は、同等のC++/WinRT のプロジェクトがコードに変換する必要があります。</span><span class="sxs-lookup"><span data-stu-id="805cb-111">The concepts are equivalent for a C++/WinRT project, though you will need to translate the code.</span></span>

## <a name="spatial-coordinate-systems-in-windows"></a><span data-ttu-id="805cb-112">Windows では、空間座標系</span><span class="sxs-lookup"><span data-stu-id="805cb-112">Spatial coordinate systems in Windows</span></span>

<span data-ttu-id="805cb-113">Windows では、実際の座標系を判断するために使用する主要なタイプは、 <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialcoordinatesystem" target="_blank">SpatialCoordinateSystem</a>します。</span><span class="sxs-lookup"><span data-stu-id="805cb-113">The core type used to reason about real-world coordinate systems in Windows is the <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialcoordinatesystem" target="_blank">SpatialCoordinateSystem</a>.</span></span> <span data-ttu-id="805cb-114">この型のインスタンスは、任意の座標系を表し、それぞれの詳細を理解することがなく 2 つの座標システム間で変換に使用できる変換行列を取得するメソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="805cb-114">An instance of this type represents an arbitrary coordinate system and provides a method to get a transformation matrix that you can use to transform between two coordinate systems without understanding the details of each.</span></span>

<span data-ttu-id="805cb-115">ポイント、線などで、またはユーザーの環境でボリュームとして表される空間の情報を返すメソッドが返されるこれらの座標に最も役立つ座標系を決定できるように SpatialCoordinateSystem パラメーターを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="805cb-115">Methods that return spatial information, represented as points, rays, or volumes in the user's surroundings, will accept a SpatialCoordinateSystem parameter to let you decide the coordinate system in which it's most useful for those coordinates to be returned.</span></span> <span data-ttu-id="805cb-116">これらの座標の単位がメートル単位で常になります。</span><span class="sxs-lookup"><span data-stu-id="805cb-116">The units for these coordinates will always be in meters.</span></span>

<span data-ttu-id="805cb-117">SpatialCoordinateSystem には、デバイスの位置を表すものも含め、他の座標システムと動的な関係があります。</span><span class="sxs-lookup"><span data-stu-id="805cb-117">A SpatialCoordinateSystem has a dynamic relationship with other coordinate systems, including those that represent the device's position.</span></span> <span data-ttu-id="805cb-118">任意の時点で、デバイスは一部の座標システムなどを検索することにあります。</span><span class="sxs-lookup"><span data-stu-id="805cb-118">At any point in time, the device may be able to locate some coordinate systems and not others.</span></span> <span data-ttu-id="805cb-119">ほとんどの座標システムでは、アプリを配置できません期間を処理できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="805cb-119">For most coordinate systems, your app must be ready to handle periods of time during which they cannot be located.</span></span>

<span data-ttu-id="805cb-120">アプリケーションを作成しないでください SpatialCoordinateSystems 直接 - ではなく認識 Api を使用して使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="805cb-120">Your application should not create SpatialCoordinateSystems directly - rather they should be consumed via the Perception APIs.</span></span> <span data-ttu-id="805cb-121">認識 Api で座標システムの 3 つのプライマリ ソースがあるで説明されている概念へのマップの各、[座標系](coordinate-systems.md)ページ。</span><span class="sxs-lookup"><span data-stu-id="805cb-121">There are three primary sources of coordinate systems in the Perception APIs, each of which map to a concept described on the [Coordinate systems](coordinate-systems.md) page:</span></span>
* <span data-ttu-id="805cb-122">静止した基準枠を取得するには、作成、 <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialstationaryframeofreference" target="_blank">SpatialStationaryFrameOfReference</a>現在から 1 つを取得または<a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialstageframeofreference" target="_blank">SpatialStageFrameOfReference</a>します。</span><span class="sxs-lookup"><span data-stu-id="805cb-122">To get a stationary frame of reference, create a <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialstationaryframeofreference" target="_blank">SpatialStationaryFrameOfReference</a> or obtain one from the current <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialstageframeofreference" target="_blank">SpatialStageFrameOfReference</a>.</span></span>
* <span data-ttu-id="805cb-123">空間のアンカーを取得するには、作成、 <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialanchor" target="_blank">SpatialAnchor</a>します。</span><span class="sxs-lookup"><span data-stu-id="805cb-123">To get a spatial anchor, create a <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialanchor" target="_blank">SpatialAnchor</a>.</span></span>
* <span data-ttu-id="805cb-124">添付のフレームの参照を取得するには、作成、 <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatiallocatorattachedframeofreference" target="_blank">SpatialLocatorAttachedFrameOfReference</a>します。</span><span class="sxs-lookup"><span data-stu-id="805cb-124">To get an attached frame of reference, create a <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatiallocatorattachedframeofreference" target="_blank">SpatialLocatorAttachedFrameOfReference</a>.</span></span>

<span data-ttu-id="805cb-125">これらのオブジェクトによって返される座標システムのすべてが右利きで、+ 右に、+ x、y と + z 内を後方に向かってします。</span><span class="sxs-lookup"><span data-stu-id="805cb-125">All of the coordinate systems returned by these objects are right-handed, with +y up, +x to the right and +z backwards.</span></span> <span data-ttu-id="805cb-126">方向正の x 方向の左側または右側のいずれかの本の指をポイントし、それらを正の y 方向に curling の正の z 軸点に注意することができます。</span><span class="sxs-lookup"><span data-stu-id="805cb-126">You can remember which direction the positive z-axis points by pointing the fingers of either your left or right hand in the positive x direction and curling them into the positive y direction.</span></span> <span data-ttu-id="805cb-127">親指が指している方向は、自身に向かうかまたは離れる方向のいずれかとなり、その座標系の z 軸の正の向きが指す方向となります。</span><span class="sxs-lookup"><span data-stu-id="805cb-127">The direction your thumb points, either toward or away from you, is the direction that the positive z-axis points for that coordinate system.</span></span> <span data-ttu-id="805cb-128">次の図は、これらの 2 つの座標系を示しています。</span><span class="sxs-lookup"><span data-stu-id="805cb-128">The following illustration shows these two coordinate systems.</span></span>

<span data-ttu-id="805cb-129">![左辺と右辺座標系](images/left-hand-right-hand.gif)</span><span class="sxs-lookup"><span data-stu-id="805cb-129">![Left-hand and right-hand coordinate systems](images/left-hand-right-hand.gif)</span></span><br>
<span data-ttu-id="805cb-130">*左辺と右辺座標系*</span><span class="sxs-lookup"><span data-stu-id="805cb-130">*Left-hand and right-hand coordinate systems*</span></span>

<span data-ttu-id="805cb-131">HoloLens の位置に基づいて SpatialCoordinateSystem にブートス トラップを使用して、 <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatiallocator" target="_blank">SpatialLocator</a>以下のセクションで説明されているか、添付または静止フレームの参照を作成するクラス。</span><span class="sxs-lookup"><span data-stu-id="805cb-131">To bootstrap into a SpatialCoordinateSystem based on the position of a HoloLens, use the <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatiallocator" target="_blank">SpatialLocator</a> class to create either an attached or stationary frame of reference, as described in the sections below.</span></span>

## <a name="place-holograms-in-the-world-using-a-spatial-stage"></a><span data-ttu-id="805cb-132">空間のステージを使用して、世界中の配置ホログラム</span><span class="sxs-lookup"><span data-stu-id="805cb-132">Place holograms in the world using a spatial stage</span></span>

<span data-ttu-id="805cb-133">Windows Mixed Reality イマーシブ ヘッドセットを非透過の座標系は、静的なを使用してアクセス<a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialstageframeofreference.current" target="_blank">SpatialStageFrameOfReference::Current</a>プロパティ。</span><span class="sxs-lookup"><span data-stu-id="805cb-133">The coordinate system for opaque Windows Mixed Reality immersive headsets is accessed using the static <a href="https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialstageframeofreference.current" target="_blank">SpatialStageFrameOfReference::Current</a> property.</span></span> <span data-ttu-id="805cb-134">この API には、座標系、については、プレーヤーが取り付けられているかどうかについてや、モバイル、安全な領域の境界の場合は、プレーヤーは、モバイル、歩き回るとかどうかを示す値、ヘッドセットは方向。</span><span class="sxs-lookup"><span data-stu-id="805cb-134">This API provides a coordinate system, information about whether the player is seated or mobile, the boundary of a safe area for walking around if the player is mobile, and an indication of whether or not the headset is directional.</span></span> <span data-ttu-id="805cb-135">空間のステージに更新プログラムのイベント ハンドラーもあります。</span><span class="sxs-lookup"><span data-stu-id="805cb-135">There is also an event handler for updates to the spatial stage.</span></span>

<span data-ttu-id="805cb-136">最初に、空間のステージを取得し、更新プログラムの定期受信します。</span><span class="sxs-lookup"><span data-stu-id="805cb-136">First, we get the spatial stage and subscribe for updates to it:</span></span> 

<span data-ttu-id="805cb-137">コードを**空間ステージの初期化**</span><span class="sxs-lookup"><span data-stu-id="805cb-137">Code for **Spatial stage initialization**</span></span>

```
SpatialStageManager::SpatialStageManager(
    const std::shared_ptr<DX::DeviceResources>& deviceResources, 
    const std::shared_ptr<SceneController>& sceneController)
    : m_deviceResources(deviceResources), m_sceneController(sceneController)
{
    // Get notified when the stage is updated.
    m_spatialStageChangedEventToken = SpatialStageFrameOfReference::CurrentChanged +=
        ref new EventHandler<Object^>(std::bind(&SpatialStageManager::OnCurrentChanged, this, _1));

    // Make sure to get the current spatial stage.
    OnCurrentChanged(nullptr);
}
```

<span data-ttu-id="805cb-138">OnCurrentChanged メソッドでは、アプリは空間段階の検査し、プレーヤーのエクスペリエンスを適宜更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="805cb-138">In the OnCurrentChanged method, your app should inspect the spatial stage and update the player experience accordingly.</span></span> <span data-ttu-id="805cb-139">この例では、ユーザーとビューのステージの範囲と移動プロパティの範囲で指定された開始位置と同様に、ステージの境界の視覚エフェクトを提供します。</span><span class="sxs-lookup"><span data-stu-id="805cb-139">In this example, we provide a visualization of the stage boundary, as well as the start position specified by the user and the stage's range of view and range of movement properties.</span></span> <span data-ttu-id="805cb-140">私たちも場合にフォールバック独自静止座標系ステージを指定することはできません。</span><span class="sxs-lookup"><span data-stu-id="805cb-140">We also fall back to our own stationary coordinate system, when a stage cannot be provided.</span></span>


<span data-ttu-id="805cb-141">コードを**空間段階の更新**</span><span class="sxs-lookup"><span data-stu-id="805cb-141">Code for **Spatial stage update**</span></span>

```
void SpatialStageManager::OnCurrentChanged(Object^ /*o*/)
{
    // The event notifies us that a new stage is available.
    // Get the current stage.
    m_currentStage = SpatialStageFrameOfReference::Current;

    // Clear previous content.
    m_sceneController->ClearSceneObjects();

    if (m_currentStage != nullptr)
    {
        // Obtain stage geometry.
        auto stageCoordinateSystem = m_currentStage->CoordinateSystem;
        auto boundsVertexArray = m_currentStage->TryGetMovementBounds(stageCoordinateSystem);

        // Visualize the area where the user can move around.
        std::vector<float3> boundsVertices;
        boundsVertices.resize(boundsVertexArray->Length);
        memcpy(boundsVertices.data(), boundsVertexArray->Data, boundsVertexArray->Length * sizeof(float3));
        std::vector<unsigned short> indices = TriangulatePoints(boundsVertices);
        m_stageBoundsShape =
            std::make_shared<SceneObject>(
                    m_deviceResources,
                    reinterpret_cast<std::vector<XMFLOAT3>&>(boundsVertices),
                    indices,
                    XMFLOAT3(DirectX::Colors::SeaGreen),
                    stageCoordinateSystem);
        m_sceneController->AddSceneObject(m_stageBoundsShape);

        // In this sample, we draw a visual indicator for some spatial stage properties.
        // If the view is forward-only, the indicator is a half circle pointing forward - otherwise, it
        // is a full circle.
        // If the user can walk around, the indicator is blue. If the user is seated, it is red.

        // The indicator is rendered at the origin - which is where the user declared the center of the
        // stage to be during setup - above the plane of the stage bounds object.
        float3 visibleAreaCenter = float3(0.f, 0.001f, 0.f);

        // Its shape depends on the look direction range.
        std::vector<float3> visibleAreaIndicatorVertices;
        if (m_currentStage->LookDirectionRange == SpatialLookDirectionRange::ForwardOnly)
        {
            // Half circle for forward-only look direction range.
            visibleAreaIndicatorVertices = CreateCircle(visibleAreaCenter, 0.25f, 9, XM_PI);
        }
        else
        {
            // Full circle for omnidirectional look direction range.
            visibleAreaIndicatorVertices = CreateCircle(visibleAreaCenter, 0.25f, 16, XM_2PI);
        }

        // Its color depends on the movement range.
        XMFLOAT3 visibleAreaColor;
        if (m_currentStage->MovementRange == SpatialMovementRange::NoMovement)
        {
            visibleAreaColor = XMFLOAT3(DirectX::Colors::OrangeRed);
        }
        else
        {
            visibleAreaColor = XMFLOAT3(DirectX::Colors::Aqua);
        }

        std::vector<unsigned short> visibleAreaIndicatorIndices = TriangulatePoints(visibleAreaIndicatorVertices);

        // Visualize the look direction range.
        m_stageVisibleAreaIndicatorShape =
            std::make_shared<SceneObject>(
                    m_deviceResources,
                    reinterpret_cast<std::vector<XMFLOAT3>&>(visibleAreaIndicatorVertices),
                    visibleAreaIndicatorIndices,
                    visibleAreaColor,
                    stageCoordinateSystem);
        m_sceneController->AddSceneObject(m_stageVisibleAreaIndicatorShape);
    }
    else
    {
        // No spatial stage was found.
        // Fall back to a stationary coordinate system.
        auto locator = SpatialLocator::GetDefault();
        if (locator)
        {
            m_stationaryFrameOfReference = locator->CreateStationaryFrameOfReferenceAtCurrentLocation();

            // Render an indicator, so that we know we fell back to a mode without a stage.
            std::vector<float3> visibleAreaIndicatorVertices;
            float3 visibleAreaCenter = float3(0.f, -2.0f, 0.f);
            visibleAreaIndicatorVertices = CreateCircle(visibleAreaCenter, 0.125f, 16, XM_2PI);
            std::vector<unsigned short> visibleAreaIndicatorIndices = TriangulatePoints(visibleAreaIndicatorVertices);
            m_stageVisibleAreaIndicatorShape =
                std::make_shared<SceneObject>(
                    m_deviceResources,
                    reinterpret_cast<std::vector<XMFLOAT3>&>(visibleAreaIndicatorVertices),
                    visibleAreaIndicatorIndices,
                    XMFLOAT3(DirectX::Colors::LightSlateGray),
                    m_stationaryFrameOfReference->CoordinateSystem);
            m_sceneController->AddSceneObject(m_stageVisibleAreaIndicatorShape);
        }
    }
}
```

<span data-ttu-id="805cb-142">時計回りの頂点のステージの境界を定義するセットが提供されます。</span><span class="sxs-lookup"><span data-stu-id="805cb-142">The set of vertices that define the stage boundary are provided in clockwise order.</span></span> <span data-ttu-id="805cb-143">Windows Mixed Reality シェルでは、ユーザーがそれに近づくと、境界にフェンスを描画します。ウォークの領域を目的に合わせて triangularize たい場合があります。</span><span class="sxs-lookup"><span data-stu-id="805cb-143">The Windows Mixed Reality shell draws a fence at the boundary when the user approaches it; you may wish to triangularize the walkable area for your own purposes.</span></span> <span data-ttu-id="805cb-144">Triangularize ステージには、次のアルゴリズムを使用できます。</span><span class="sxs-lookup"><span data-stu-id="805cb-144">The following algorithm can be used to triangularize the stage.</span></span>


<span data-ttu-id="805cb-145">コードを**空間ステージ triangularization**</span><span class="sxs-lookup"><span data-stu-id="805cb-145">Code for **Spatial stage triangularization**</span></span>

```
std::vector<unsigned short> SpatialStageManager::TriangulatePoints(std::vector<float3> const& vertices)
{
    size_t const& vertexCount = vertices.size();

    // Segments of the shape are removed as they are triangularized.
    std::vector<bool> vertexRemoved;
    vertexRemoved.resize(vertexCount, false);
    unsigned int vertexRemovedCount = 0;

    // Indices are used to define triangles.
    std::vector<unsigned short> indices;

    // Decompose into convex segments.
    unsigned short currentVertex = 0;
    while (vertexRemovedCount < (vertexCount - 2))
    {
        // Get next triangle:
        // Start with the current vertex.
        unsigned short index1 = currentVertex;

        // Get the next available vertex.
        unsigned short index2 = index1 + 1;

        // This cycles to the next available index.
        auto CycleIndex = [=](unsigned short indexToCycle, unsigned short stopIndex)
        {
            // Make sure the index does not exceed bounds.
            if (indexToCycle >= unsigned short(vertexCount))
            {
                indexToCycle -= unsigned short(vertexCount);
            }

            while (vertexRemoved[indexToCycle])
            {
                // If the vertex is removed, go to the next available one.
                ++indexToCycle;

                // Make sure the index does not exceed bounds.
                if (indexToCycle >= unsigned short(vertexCount))
                {
                    indexToCycle -= unsigned short(vertexCount);
                }

                // Prevent cycling all the way around.
                // Should not be needed, as we limit with the vertex count.
                if (indexToCycle == stopIndex)
                {
                    break;
                }
            }

            return indexToCycle;
        };
        index2 = CycleIndex(index2, index1);

        // Get the next available vertex after that.
        unsigned short index3 = index2 + 1;
        index3 = CycleIndex(index3, index1);

        // Vertices that may define a triangle inside the 2D shape.
        auto& v1 = vertices[index1];
        auto& v2 = vertices[index2];
        auto& v3 = vertices[index3];

        // If the projection of the first segment (in clockwise order) onto the second segment is 
        // positive, we know that the clockwise angle is less than 180 degrees, which tells us 
        // that the triangle formed by the two segments is contained within the bounding shape.
        auto v2ToV1 = v1 - v2;
        auto v2ToV3 = v3 - v2;
        float3 normalToV2ToV3 = { -v2ToV3.z, 0.f, v2ToV3.x };
        float projectionOntoNormal = dot(v2ToV1, normalToV2ToV3);
        if (projectionOntoNormal >= 0)
        {
            // Triangle is contained within the 2D shape.

            // Remove peak vertex from the list.
            vertexRemoved[index2] = true;
            ++vertexRemovedCount;

            // Create the triangle.
            indices.push_back(index1);
            indices.push_back(index2);
            indices.push_back(index3);

            // Continue on to the next outer triangle.
            currentVertex = index3;
        }
        else
        {
            // Triangle is a cavity in the 2D shape.
            // The next triangle starts at the inside corner.
            currentVertex = index2;
        }
    }

    indices.shrink_to_fit();
    return indices;
}
```

## <a name="place-holograms-in-the-world-using-a-stationary-frame-of-reference"></a><span data-ttu-id="805cb-146">静止した基準枠を使用して、世界中の配置ホログラム</span><span class="sxs-lookup"><span data-stu-id="805cb-146">Place holograms in the world using a stationary frame of reference</span></span>

<span data-ttu-id="805cb-147">[SpatialStationaryFrameOfReference](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialstationaryframeofreference.aspx)クラスのフレームを表しますが参照する[静止](coordinate-systems.md#stationary-frame-of-reference)周りを基準として、ユーザー、ユーザーの環境と移動します。</span><span class="sxs-lookup"><span data-stu-id="805cb-147">The [SpatialStationaryFrameOfReference](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialstationaryframeofreference.aspx) class represents a frame of reference that [remains stationary](coordinate-systems.md#stationary-frame-of-reference) relative to the user's surroundings as the user moves around.</span></span> <span data-ttu-id="805cb-148">このフレームの参照では、デバイスの近くに安定したままの状態の座標で優先されます。</span><span class="sxs-lookup"><span data-stu-id="805cb-148">This frame of reference prioritizes keeping coordinates stable near the device.</span></span> <span data-ttu-id="805cb-149">ホログラムを表示するときに、レンダリング エンジン内で基になるワールド座標系として機能すること、SpatialStationaryFrameOfReference の 1 つのキーの使用です。</span><span class="sxs-lookup"><span data-stu-id="805cb-149">One key use of a SpatialStationaryFrameOfReference is to act as the underlying world coordinate system within a rendering engine when rendering holograms.</span></span>

<span data-ttu-id="805cb-150">SpatialStationaryFrameOfReference を取得する、 [SpatialLocator](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatiallocator.aspx)クラスと呼び出し[CreateStationaryFrameOfReferenceAtCurrentLocation](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatiallocator.createstationaryframeofreferenceatcurrentlocation.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="805cb-150">To get a SpatialStationaryFrameOfReference, use the [SpatialLocator](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatiallocator.aspx) class and call [CreateStationaryFrameOfReferenceAtCurrentLocation](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatiallocator.createstationaryframeofreferenceatcurrentlocation.aspx).</span></span>

<span data-ttu-id="805cb-151">Windows Holographic のアプリのテンプレート コード: から</span><span class="sxs-lookup"><span data-stu-id="805cb-151">From the Windows Holographic app template code:</span></span>

```
           // The simplest way to render world-locked holograms is to create a stationary reference frame
           // when the app is launched. This is roughly analogous to creating a "world" coordinate system
           // with the origin placed at the device's position as the app is launched.
           referenceFrame = locator.CreateStationaryFrameOfReferenceAtCurrentLocation();
```
* <span data-ttu-id="805cb-152">静止した基準枠は、領域全体の基準とした最適位置を提供する設計されています。</span><span class="sxs-lookup"><span data-stu-id="805cb-152">Stationary reference frames are designed to provide a best-fit position relative to the overall space.</span></span> <span data-ttu-id="805cb-153">その参照フレーム内の個々 の位置は、ユーザーが少しのずれが許可されます。</span><span class="sxs-lookup"><span data-stu-id="805cb-153">Individual positions within that reference frame are allowed to drift slightly.</span></span> <span data-ttu-id="805cb-154">これは、普通は、デバイスは環境についての詳細を学習します。</span><span class="sxs-lookup"><span data-stu-id="805cb-154">This is normal as the device learns more about the environment.</span></span>
* <span data-ttu-id="805cb-155">ホログラムの個々 の正確な配置が必要な場合、SpatialAnchor を使用して、個々 のホログラム現実の世界での位置に固定する必要がある-たとえば、ポイントをユーザーことを示します特別な関心のあります。</span><span class="sxs-lookup"><span data-stu-id="805cb-155">When precise placement of individual holograms is required, a SpatialAnchor should be used to anchor the individual hologram to a position in the real world - for example, a point the user indicates to be of special interest.</span></span> <span data-ttu-id="805cb-156">アンカーの位置はないユーザーずれが修正できます。アンカー以降、次のフレームでは、修正が行われた後修正された位置が使用されます。</span><span class="sxs-lookup"><span data-stu-id="805cb-156">Anchor positions do not drift, but can be corrected; the anchor will use the corrected position starting in the next frame after the correction has occurred.</span></span>

## <a name="place-holograms-in-the-world-using-spatial-anchors"></a><span data-ttu-id="805cb-157">空間のアンカーを使用して、世界中の配置ホログラム</span><span class="sxs-lookup"><span data-stu-id="805cb-157">Place holograms in the world using spatial anchors</span></span>

<span data-ttu-id="805cb-158">[空間アンカー](coordinate-systems.md#spatial-anchors)ホログラムを現実の世界での特定の場所に配置する優れた方法は、システム アンカーのことを確認する時間の経過と共にのままにします。</span><span class="sxs-lookup"><span data-stu-id="805cb-158">[Spatial anchors](coordinate-systems.md#spatial-anchors) are a great way to place holograms at a specific place in the real world, with the system ensuring the anchor stays in place over time.</span></span> <span data-ttu-id="805cb-159">このトピックでは、アンカーのデータを操作する方法と作成して、アンカーを使用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="805cb-159">This topic explains how to create and use an anchor, and how to work with anchor data.</span></span>

<span data-ttu-id="805cb-160">任意の位置と、選択、SpatialCoordinateSystem 内向き、SpatialAnchor を作成できます。</span><span class="sxs-lookup"><span data-stu-id="805cb-160">You can create a SpatialAnchor at any position and orientation within the SpatialCoordinateSystem of your choosing.</span></span> <span data-ttu-id="805cb-161">デバイスは、現時点では、その座標系を検索できる必要があり、システムが空間アンカーの制限に達しましたのないでする必要がありますがします。</span><span class="sxs-lookup"><span data-stu-id="805cb-161">The device must be able to locate that coordinate system at the moment, and the system must not have reached its limit of spatial anchors.</span></span>

<span data-ttu-id="805cb-162">定義した後、SpatialAnchor の座標系は、正確な位置と向きの初期位置を保持する継続的に調整します。</span><span class="sxs-lookup"><span data-stu-id="805cb-162">Once defined, the coordinate system of a SpatialAnchor adjusts continually to retain the precise position and orientation of its initial location.</span></span> <span data-ttu-id="805cb-163">この SpatialAnchor は、その正確な場所で、ユーザーの環境に固定表示されるホログラムを表示するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="805cb-163">You can then use this SpatialAnchor to render holograms that will appear fixed in the user's surroundings at that exact location.</span></span>

<span data-ttu-id="805cb-164">アンカーを維持する調整の効果を拡大するには、アンカーが増加からの距離として。</span><span class="sxs-lookup"><span data-stu-id="805cb-164">The effects of the adjustments that keep the anchor in place are magnified as distance from the anchor increases.</span></span> <span data-ttu-id="805cb-165">そのため、そのアンカーの配信元から複数の約 3 メートル アンカーの基準とした内容の表示を避ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="805cb-165">Therefore, you should avoid rendering content relative to an anchor that is more than about 3 meters from that anchor's origin.</span></span>

<span data-ttu-id="805cb-166">[CoordinateSystem](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchor.coordinatesystem.aspx)プロパティが、デバイス、アンカーの正確な場所を調整するときに適用されるイージングを使用する、アンカー ポイントからコンテンツを配置することができます、座標系を取得します。</span><span class="sxs-lookup"><span data-stu-id="805cb-166">The [CoordinateSystem](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchor.coordinatesystem.aspx) property gets a coordinate system that lets you place content relative to the anchor, with easing applied when the device adjusts the anchor's precise location.</span></span>

<span data-ttu-id="805cb-167">使用して、 [RawCoordinateSystem](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchor.rawcoordinatesystem.aspx)プロパティと、対応する[RawCoordinateSystemAdjusted](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchor.rawcoordinatesystemadjusted.aspx)自分でこれらの調整を管理するイベントです。</span><span class="sxs-lookup"><span data-stu-id="805cb-167">Use the [RawCoordinateSystem](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchor.rawcoordinatesystem.aspx) property and the corresponding [RawCoordinateSystemAdjusted](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchor.rawcoordinatesystemadjusted.aspx) event to manage these adjustments yourself.</span></span>

### <a name="persist-and-share-spatial-anchors"></a><span data-ttu-id="805cb-168">保存や共有空間アンカー</span><span class="sxs-lookup"><span data-stu-id="805cb-168">Persist and share spatial anchors</span></span>

<span data-ttu-id="805cb-169">使用してローカル SpatialAnchor を永続化できる、 [SpatialAnchorStore](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchorstore.aspx)クラスし、同じ HoloLens デバイスに将来のアプリのセッションに戻りを取得します。</span><span class="sxs-lookup"><span data-stu-id="805cb-169">You can persist a SpatialAnchor locally using the [SpatialAnchorStore](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchorstore.aspx) class and then get it back in a future app session on the same HoloLens device.</span></span>

<span data-ttu-id="805cb-170">使用して<a href="https://docs.microsoft.com/azure/spatial-anchors/overview" target="_blank">Azure 空間アンカー</a>から、ローカルな SpatialAnchor は、アプリが複数の HoloLens、iOS や Android デバイスで見つけることができますし、持続性のあるクラウド アンカーを作成できます。</span><span class="sxs-lookup"><span data-stu-id="805cb-170">By using <a href="https://docs.microsoft.com/azure/spatial-anchors/overview" target="_blank">Azure Spatial Anchors</a>, you can create a durable cloud anchor from a local SpatialAnchor, which your app can then locate across multiple HoloLens, iOS and Android devices.</span></span>  <span data-ttu-id="805cb-171">各ユーザーは複数のデバイスで共通の空間アンカーを共有することで、同じ物理的な場所でそのアンカーの基準としたコンテンツを表示できます。</span><span class="sxs-lookup"><span data-stu-id="805cb-171">By sharing a common spatial anchor across multiple devices, each user can see content rendered relative to that anchor in the same physical location.</span></span>  <span data-ttu-id="805cb-172">これにより、リアルタイムのエクスペリエンスを共有します。</span><span class="sxs-lookup"><span data-stu-id="805cb-172">This allows for real-time shared experiences.</span></span>

<span data-ttu-id="805cb-173">使用することも<a href="https://docs.microsoft.com/azure/spatial-anchors/overview" target="_blank">Azure 空間アンカー</a> HoloLens、iOS および Android デバイスの間で非同期ホログラム永続化します。</span><span class="sxs-lookup"><span data-stu-id="805cb-173">You can also use <a href="https://docs.microsoft.com/azure/spatial-anchors/overview" target="_blank">Azure Spatial Anchors</a> for asynchronous hologram persistence across HoloLens, iOS and Android devices.</span></span>  <span data-ttu-id="805cb-174">持続性のあるクラウド空間アンカーを共有することで複数のデバイスはこれらのデバイスがまとめてと同時に存在しない場合でも、時間の経過と共に同じ永続化されたホログラムを確認できます。</span><span class="sxs-lookup"><span data-stu-id="805cb-174">By sharing a durable cloud spatial anchor, multiple devices can observe the same persisted hologram over time, even if those devices are not present together at the same time.</span></span>

<span data-ttu-id="805cb-175">5 分間試して、HoloLens のアプリで共有のエクスペリエンスの構築を開始する、<a href="https://docs.microsoft.com/azure/spatial-anchors/quickstarts/get-started-hololens" target="_blank">空間アンカー HoloLens の Azure クイック スタート</a>します。</span><span class="sxs-lookup"><span data-stu-id="805cb-175">To get started building shared experiences in your HoloLens app, try out the 5-minute <a href="https://docs.microsoft.com/azure/spatial-anchors/quickstarts/get-started-hololens" target="_blank">Azure Spatial Anchors HoloLens quickstart</a>.</span></span>

<span data-ttu-id="805cb-176">空間のアンカーを Azure で稼働しているを開発したとして、<a href="https://docs.microsoft.com/azure/spatial-anchors/concepts/create-locate-anchors-cpp-winrt" target="_blank">を作成し、HoloLens でアンカーを見つける</a>します。</span><span class="sxs-lookup"><span data-stu-id="805cb-176">Once you're up and running with Azure Spatial Anchors, you can then <a href="https://docs.microsoft.com/azure/spatial-anchors/concepts/create-locate-anchors-cpp-winrt" target="_blank">create and locate anchors on HoloLens</a>.</span></span>  <span data-ttu-id="805cb-177">チュートリアルに利用<a href="https://docs.microsoft.com/azure/spatial-anchors/create-locate-anchors-overview" target="_blank">Android および iOS</a>同様に、すべてのデバイスで同じアンカーを共有できるようにすることです。</span><span class="sxs-lookup"><span data-stu-id="805cb-177">Walkthroughs are available for <a href="https://docs.microsoft.com/azure/spatial-anchors/create-locate-anchors-overview" target="_blank">Android and iOS</a> as well, enabling you to share the same anchors on all devices.</span></span>

### <a name="create-spatialanchors-for-holographic-content"></a><span data-ttu-id="805cb-178">SpatialAnchors を holographic のコンテンツを作成します。</span><span class="sxs-lookup"><span data-stu-id="805cb-178">Create SpatialAnchors for holographic content</span></span>

<span data-ttu-id="805cb-179">このコード サンプルでは、変更後の Windows Holographic アプリ テンプレートの作成に固定する場合に、 **Pressed**ジェスチャが検出されました。</span><span class="sxs-lookup"><span data-stu-id="805cb-179">For this code sample, we modified the Windows Holographic app template to create anchors when the **Pressed** gesture is detected.</span></span> <span data-ttu-id="805cb-180">キューブは、レンダリング パス中に、アンカーに格納されます。</span><span class="sxs-lookup"><span data-stu-id="805cb-180">The cube is then placed at the anchor during the render pass.</span></span>

<span data-ttu-id="805cb-181">ヘルパー クラスでは、複数のアンカーがサポートされている、このコード サンプルを使用すると同数のキューブを配置できます。</span><span class="sxs-lookup"><span data-stu-id="805cb-181">Since multiple anchors are supported by the helper class, we can place as many cubes as we want using this code sample!</span></span>

<span data-ttu-id="805cb-182">アンカーの Id は、何か、アプリを制御することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="805cb-182">Note that the IDs for anchors are something you control in your app.</span></span> <span data-ttu-id="805cb-183">この例では順次アンカーのアプリのコレクションに格納されているアンカーの数に基づく名前付けスキームを作成しました。</span><span class="sxs-lookup"><span data-stu-id="805cb-183">In this example, we have created a naming scheme that is sequential based on the number of anchors currently stored in the app's collection of anchors.</span></span>

```
   // Check for new input state since the last frame.
   SpatialInteractionSourceState^ pointerState = m_spatialInputHandler->CheckForInput();
   if (pointerState != nullptr)
   {
       // Try to get the pointer pose relative to the SpatialStationaryReferenceFrame.
       SpatialPointerPose^ pointerPose = pointerState->TryGetPointerPose(currentCoordinateSystem);
       if (pointerPose != nullptr)
       {
           // When a Pressed gesture is detected, the anchor will be created two meters in front of the user.

           // Get the gaze direction relative to the given coordinate system.
           const float3 headPosition = pointerPose->Head->Position;
           const float3 headDirection = pointerPose->Head->ForwardDirection;

           // The anchor position in the StationaryReferenceFrame.
           static const float distanceFromUser = 2.0f; // meters
           const float3 gazeAtTwoMeters = headPosition + (distanceFromUser * headDirection);

           // Create the anchor at position.
           SpatialAnchor^ anchor = SpatialAnchor::TryCreateRelativeTo(currentCoordinateSystem, gazeAtTwoMeters);

           if ((anchor != nullptr) && (m_spatialAnchorHelper != nullptr))
           {
               // In this example, we store the anchor in an IMap.
               auto anchorMap = m_spatialAnchorHelper->GetAnchorMap();

               // Create an identifier for the anchor.
               String^ id = ref new String(L"HolographicSpatialAnchorStoreSample_Anchor") + anchorMap->Size;

               anchorMap->Insert(id->ToString(), anchor);
           }
       }
   }
```

### <a name="asynchronously-load-and-cache-the-spatialanchorstore"></a><span data-ttu-id="805cb-184">非同期的に読み込まれ、SpatialAnchorStore をキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="805cb-184">Asynchronously load, and cache, the SpatialAnchorStore</span></span>

<span data-ttu-id="805cb-185">この永続化、処理に役立つ SampleSpatialAnchorHelper クラスを作成する方法について説明を含みます。</span><span class="sxs-lookup"><span data-stu-id="805cb-185">Let's see how to write a SampleSpatialAnchorHelper class that helps handle this persistence, including:</span></span>
* <span data-ttu-id="805cb-186">Platform::string キーによってインデックスが作成、メモリ内表現のアンカーのコレクションを格納します。</span><span class="sxs-lookup"><span data-stu-id="805cb-186">Storing a collection of in-memory anchors, indexed by a Platform::String key.</span></span>
* <span data-ttu-id="805cb-187">アンカーは、システムの SpatialAnchorStore から読み込み、これは分離されますローカル メモリ内コレクションから。</span><span class="sxs-lookup"><span data-stu-id="805cb-187">Loading anchors from the system's SpatialAnchorStore, which is kept separate from the local in-memory collection.</span></span>
* <span data-ttu-id="805cb-188">そのためには、アプリが選択したときに、SpatialAnchorStore にアンカーのローカル メモリ内コレクションを保存しています。</span><span class="sxs-lookup"><span data-stu-id="805cb-188">Saving the local in-memory collection of anchors to the SpatialAnchorStore when the app chooses to do so.</span></span>

<span data-ttu-id="805cb-189">保存する方法を次に示します[SpatialAnchor](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchor.aspx)内のオブジェクト、 [SpatialAnchorStore](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchorstore.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="805cb-189">Here's how to save [SpatialAnchor](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchor.aspx) objects in the [SpatialAnchorStore](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchorstore.aspx).</span></span>

<span data-ttu-id="805cb-190">クラスは、起動時に、SpatialAnchorStore を非同期的に要求します。</span><span class="sxs-lookup"><span data-stu-id="805cb-190">When the class starts up, we request the SpatialAnchorStore asynchronously.</span></span> <span data-ttu-id="805cb-191">システム I/O API は、アンカー ストアを読み込むし、I/O は非ブロッキングようにこの API は非同期されるようになります。</span><span class="sxs-lookup"><span data-stu-id="805cb-191">This involves system I/O as the API loads the anchor store, and this API is made asynchronous so that the I/O is non-blocking.</span></span>

```
   // Request the spatial anchor store, which is the WinRT object that will accept the imported anchor data.
   return create_task(SpatialAnchorManager::RequestStoreAsync())
       .then([](task<SpatialAnchorStore^> previousTask)
   {
       std::shared_ptr<SampleSpatialAnchorHelper> newHelper = nullptr;

       try
       {
           SpatialAnchorStore^ anchorStore = previousTask.get();

           // Once the SpatialAnchorStore has been loaded by the system, we can create our helper class.

           // Using "new" to access private constructor
           newHelper = std::shared_ptr<SampleSpatialAnchorHelper>(new SampleSpatialAnchorHelper(anchorStore));

           // Now we can load anchors from the store.
           newHelper->LoadFromAnchorStore();
       }
       catch (Exception^ exception)
       {
           PrintWstringToDebugConsole(
               std::wstring(L"Exception while loading the anchor store: ") +
               exception->Message->Data() +
               L"\n"
               );
       }

       // Return the initialized class instance.
       return newHelper;
   });
```

<span data-ttu-id="805cb-192">アンカーの保存に使用できる SpatialAnchorStore が与えられます。</span><span class="sxs-lookup"><span data-stu-id="805cb-192">You will be given a SpatialAnchorStore that you can use to save the anchors.</span></span> <span data-ttu-id="805cb-193">これは、IMapView を関連付ける SpatialAnchors いるデータ値では文字列であるキーの値です。</span><span class="sxs-lookup"><span data-stu-id="805cb-193">This is an IMapView that associates key values that are Strings, with data values that are SpatialAnchors.</span></span> <span data-ttu-id="805cb-194">サンプル コードで保存すれば、ヘルパー クラスのパブリック関数を通じてアクセスできるプライベート クラス メンバー変数にします。</span><span class="sxs-lookup"><span data-stu-id="805cb-194">In our sample code, we store this in a private class member variable that is accessible through a public function of our helper class.</span></span>

```
   SampleSpatialAnchorHelper::SampleSpatialAnchorHelper(SpatialAnchorStore^ anchorStore)
   {
       m_anchorStore = anchorStore;
       m_anchorMap = ref new Platform::Collections::Map<String^, SpatialAnchor^>();
   }
```

>[!NOTE]
><span data-ttu-id="805cb-195">忘れずに保存および読み込みアンカー ストア保留/再開イベントをフックします。</span><span class="sxs-lookup"><span data-stu-id="805cb-195">Don't forget to hook up the suspend/resume events to save and load the anchor store.</span></span>

```
   void HolographicSpatialAnchorStoreSampleMain::SaveAppState()
   {
       // For example, store information in the SpatialAnchorStore.
       if (m_spatialAnchorHelper != nullptr)
       {
           m_spatialAnchorHelper->TrySaveToAnchorStore();
       }
   }
```

```
   void HolographicSpatialAnchorStoreSampleMain::LoadAppState()
   {
       // For example, load information from the SpatialAnchorStore.
       LoadAnchorStore();
   }
```

### <a name="save-content-to-the-anchor-store"></a><span data-ttu-id="805cb-196">コンテンツをアンカー ストアに保存します。</span><span class="sxs-lookup"><span data-stu-id="805cb-196">Save content to the anchor store</span></span>

<span data-ttu-id="805cb-197">システムでは、アプリが中断、ときに、空間、アンカーをアンカー ストアに保存する必要があります。</span><span class="sxs-lookup"><span data-stu-id="805cb-197">When the system suspends your app, you need to save your spatial anchors to the anchor store.</span></span> <span data-ttu-id="805cb-198">アプリの実装に必要であることを検索するには、アンカーのストアにその他の時間は、アンカーを保存することもできます。</span><span class="sxs-lookup"><span data-stu-id="805cb-198">You may also choose to save anchors to the anchor store at other times, as you find to be necessary for your app's implementation.</span></span>

<span data-ttu-id="805cb-199">SpatialAnchorStore をメモリ内のアンカーを保存してください準備ができたらは、コレクションをループ処理し、それぞれを保存しようとしています。</span><span class="sxs-lookup"><span data-stu-id="805cb-199">When you're ready to try saving the in-memory anchors to the SpatialAnchorStore, you can loop through your collection and try to save each one.</span></span>

```
   // TrySaveToAnchorStore: Stores all anchors from memory into the app's anchor store.
   //
   // For each anchor in memory, this function tries to store it in the app's AnchorStore. The operation will fail if
   // the anchor store already has an anchor by that name.
   //
   bool SampleSpatialAnchorHelper::TrySaveToAnchorStore()
   {
       // This function returns true if all the anchors in the in-memory collection are saved to the anchor
       // store. If zero anchors are in the in-memory collection, we will still return true because the
       // condition has been met.
       bool success = true;

       // If access is denied, 'anchorStore' will not be obtained.
       if (m_anchorStore != nullptr)
       {
           for each (auto& pair in m_anchorMap)
           {
               auto const& id = pair->Key;
               auto const& anchor = pair->Value;

               // Try to save the anchors.
               if (!m_anchorStore->TrySave(id, anchor))
               {
                   // This may indicate the anchor ID is taken, or the anchor limit is reached for the app.
                   success=false;
               }
           }
       }

       return success;
   }
```

### <a name="load-content-from-the-anchor-store-when-the-app-resumes"></a><span data-ttu-id="805cb-200">アンカー ストアからアプリを再開したときにコンテンツを読み込む</span><span class="sxs-lookup"><span data-stu-id="805cb-200">Load content from the anchor store when the app resumes</span></span>

<span data-ttu-id="805cb-201">アプリの再開時、または implementaiton のアプリのために必要な他の任意の時点で、アンカー ストアの IMapView から SpatialAnchors の独自のメモリ内データベースに転送することによって、AnchorStore に以前に保存されたアンカーを戻すことができます。</span><span class="sxs-lookup"><span data-stu-id="805cb-201">When your app resumes, or at any other time necessary for your app's implementaiton, you can restore anchors that were previously saved to the AnchorStore by transferring them from the anchor store's IMapView to your own in-memory database of SpatialAnchors.</span></span>

<span data-ttu-id="805cb-202">SpatialAnchorStore から表現のアンカーを復元するには、互いをメモリ内コレクションに興味があるを復元します。</span><span class="sxs-lookup"><span data-stu-id="805cb-202">To restore anchors from the SpatialAnchorStore, restore each one that you are interested in to your own in-memory collection.</span></span>

<span data-ttu-id="805cb-203">SpatialAnchors; の独自のメモリ内データベースを作成する必要があります。何らかの方法に文字列を作成する SpatialAnchors に関連付けます。</span><span class="sxs-lookup"><span data-stu-id="805cb-203">You need your own in-memory database of SpatialAnchors; some way to associate Strings with the SpatialAnchors that you create.</span></span> <span data-ttu-id="805cb-204">サンプル コードで選択、Windows::Foundation::Collections::IMap を使用して、アンカーを格納するしやすく、SpatialAnchorStore に同じキーとデータの値を使用します。</span><span class="sxs-lookup"><span data-stu-id="805cb-204">In our sample code, we choose to use a Windows::Foundation::Collections::IMap to store the anchors, which makes it easy to use the same key and data value for the SpatialAnchorStore.</span></span>

```
   // This is an in-memory anchor list that is separate from the anchor store.
   // These anchors may be used, reasoned about, and so on before committing the collection to the store.
   Windows::Foundation::Collections::IMap<Platform::String^, Windows::Perception::Spatial::SpatialAnchor^>^ m_anchorMap;
```

>[!NOTE]
><span data-ttu-id="805cb-205">復元されるアンカーできない可能性があります場所を特定できるでしょうか。</span><span class="sxs-lookup"><span data-stu-id="805cb-205">An anchor that is restored might not be locatable right away.</span></span> <span data-ttu-id="805cb-206">たとえば、別の部屋にまたは、別の構築に完全にアンカーがあります。</span><span class="sxs-lookup"><span data-stu-id="805cb-206">For example, it might be an anchor in a separate room or in a different building altogether.</span></span> <span data-ttu-id="805cb-207">使用する前に locatability、AnchorStore から取得したアンカーをテストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="805cb-207">Anchors retrieved from the AnchorStore should be tested for locatability before using them.</span></span>

<br>

>[!NOTE]
><span data-ttu-id="805cb-208">このコード例では、AnchorStore からすべてのアンカーを取得します。</span><span class="sxs-lookup"><span data-stu-id="805cb-208">In this example code, we retrieve all anchors from the AnchorStore.</span></span> <span data-ttu-id="805cb-209">これは要件ではありません。アプリと同様を選択アンカーの特定のサブセットを実装に意味のある文字列キー値を使用しています。</span><span class="sxs-lookup"><span data-stu-id="805cb-209">This is not a requirement; your app could just as well pick and choose a certain subset of anchors by using String key values that are meaningful to your implementation.</span></span>

```
   // LoadFromAnchorStore: Loads all anchors from the app's anchor store into memory.
   //
   // The anchors are stored in memory using an IMap, which stores anchors using a string identifier. Any string can be used as
   // the identifier; it can have meaning to the app, such as "Game_Leve1_CouchAnchor," or it can be a GUID that is generated
   // by the app.
   //
   void SampleSpatialAnchorHelper::LoadFromAnchorStore()
   {
       // If access is denied, 'anchorStore' will not be obtained.
       if (m_anchorStore != nullptr)
       {
           // Get all saved anchors.
           auto anchorMapView = m_anchorStore->GetAllSavedAnchors();
           for each (auto const& pair in anchorMapView)
           {
               auto const& id = pair->Key;
               auto const& anchor = pair->Value;
               m_anchorMap->Insert(id, anchor);
           }
       }
   }
```

### <a name="clear-the-anchor-store-when-needed"></a><span data-ttu-id="805cb-210">必要なときに、アンカー ストアをクリアします。</span><span class="sxs-lookup"><span data-stu-id="805cb-210">Clear the anchor store, when needed</span></span>

<span data-ttu-id="805cb-211">場合によっては、アプリの状態をオフにして新しいデータを書き込む必要があります。</span><span class="sxs-lookup"><span data-stu-id="805cb-211">Sometimes, you need to clear app state and write new data.</span></span> <span data-ttu-id="805cb-212">その方法を次に示します、 [SpatialAnchorStore](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchorstore.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="805cb-212">Here's how you do that with the [SpatialAnchorStore](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialanchorstore.aspx).</span></span>

<span data-ttu-id="805cb-213">ヘルパー クラスを使用する必要はありませんほぼをラップする関数をクリアします。</span><span class="sxs-lookup"><span data-stu-id="805cb-213">Using our helper class, it's almost unnecessary to wrap the Clear function.</span></span> <span data-ttu-id="805cb-214">このヘルパー クラスが SpatialAnchorStore インスタンスを所有しているの役割を与えられているため、サンプルの実装では、そのために選択します。</span><span class="sxs-lookup"><span data-stu-id="805cb-214">We choose to do so in our sample implementation, because our helper class is given the responsibility of owning the SpatialAnchorStore instance.</span></span>

```
   // ClearAnchorStore: Clears the AnchorStore for the app.
   //
   // This function clears the AnchorStore. It has no effect on the anchors stored in memory.
   //
   void SampleSpatialAnchorHelper::ClearAnchorStore()
   {
       // If access is denied, 'anchorStore' will not be obtained.
       if (m_anchorStore != nullptr)
       {
           // Clear all anchors from the store.
           m_anchorStore->Clear();
       }
   }
```

### <a name="example-relating-anchor-coordinate-systems-to-stationary-reference-frame-coordinate-systems"></a><span data-ttu-id="805cb-215">以下に例を示します。アンカーの座標系に関連する静止した基準枠座標系</span><span class="sxs-lookup"><span data-stu-id="805cb-215">Example: Relating anchor coordinate systems to stationary reference frame coordinate systems</span></span>

<span data-ttu-id="805cb-216">たとえば、アンカーがあり、その大部分の他のコンテンツを既に使用している SpatialStationaryReferenceFrame に関連するアンカーの座標システムで何かにするとします。</span><span class="sxs-lookup"><span data-stu-id="805cb-216">Let's say that you have an anchor, and you want to relate something in your anchor's coordinate system to the SpatialStationaryReferenceFrame that you’re already using for most of your other content.</span></span> <span data-ttu-id="805cb-217">使用することができます[TryGetTransformTo](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialcoordinatesystem.trygettransformto.aspx)静止した基準枠のアンカーの座標系から変換を取得します。</span><span class="sxs-lookup"><span data-stu-id="805cb-217">You can use [TryGetTransformTo](https://msdn.microsoft.com/library/windows/apps/windows.perception.spatial.spatialcoordinatesystem.trygettransformto.aspx) to obtain a transform from the anchor’s coordinate system to that of the stationary reference frame:</span></span>

```
   // In this code snippet, someAnchor is a SpatialAnchor^ that has been initialized and is valid in the current environment.
   float4x4 anchorSpaceToCurrentCoordinateSystem;
   SpatialCoordinateSystem^ anchorSpace = someAnchor->CoordinateSystem;
   const auto tryTransform = anchorSpace->TryGetTransformTo(currentCoordinateSystem);
   if (tryTransform != nullptr)
   {
       anchorSpaceToCurrentCoordinateSystem = tryTransform->Value;
   }
```

<span data-ttu-id="805cb-218">このプロセスは、2 つの方法に便利です。</span><span class="sxs-lookup"><span data-stu-id="805cb-218">This process is useful to you in two ways:</span></span>
1. <span data-ttu-id="805cb-219">2 つのフレームを参照する場合、相互に関連した認識できるように指示し、;</span><span class="sxs-lookup"><span data-stu-id="805cb-219">It tells you if the two reference frames can be understood relative to one another, and;</span></span>
2. <span data-ttu-id="805cb-220">そのためが提供されている場合にもう 1 つの座標システムから直接移動する変換。</span><span class="sxs-lookup"><span data-stu-id="805cb-220">If so, it provides you a transform to go directly from one coordinate system to the other.</span></span>

<span data-ttu-id="805cb-221">この情報は、2 つの参照フレーム間でオブジェクトの空間関係の理解しています。</span><span class="sxs-lookup"><span data-stu-id="805cb-221">With this information, you have an understanding of the spatial relation between objects between the two reference frames.</span></span>

<span data-ttu-id="805cb-222">レンダリングには、元の参照フレームまたはアンカーに従ってオブジェクトをグループ化して多くの場合より良い結果を取得できます。</span><span class="sxs-lookup"><span data-stu-id="805cb-222">For rendering, you can often obtain better results by grouping objects according to their original reference frame or anchor.</span></span> <span data-ttu-id="805cb-223">グループごとに別個の描画パスを実行します。</span><span class="sxs-lookup"><span data-stu-id="805cb-223">Perform a separate drawing pass for each group.</span></span> <span data-ttu-id="805cb-224">View 行列は、最初に同一の座標系を使用して作成されるモデルの変換でオブジェクトをより正確なです。</span><span class="sxs-lookup"><span data-stu-id="805cb-224">The view matrices are more accurate for objects with model transforms that are created initially using the same coordinate system.</span></span>

## <a name="create-holograms-using-a-device-attached-frame-of-reference"></a><span data-ttu-id="805cb-225">デバイス接続の参照を使用してホログラムを作成します。</span><span class="sxs-lookup"><span data-stu-id="805cb-225">Create holograms using a device-attached frame of reference</span></span>

<span data-ttu-id="805cb-226">ホログラムを表示するために必要な場合があるを[がアタッチされたまま](coordinate-systems.md#attached-frame-of-reference)デバイスの場所、デバイスがその向きを決定することのみのときに、情報メッセージまたは情報メッセージをデバッグなどのパネルに、いない領域の位置。</span><span class="sxs-lookup"><span data-stu-id="805cb-226">There are times when you want to render a hologram that [remains attached](coordinate-systems.md#attached-frame-of-reference) to the device's location, for example a panel with debugging information or an informational message when the device is only able to determine its orientation and not its position in space.</span></span> <span data-ttu-id="805cb-227">これを行うには、接続されている基準を使用します。</span><span class="sxs-lookup"><span data-stu-id="805cb-227">To accomplish this, we use an attached frame of reference.</span></span>

<span data-ttu-id="805cb-228">SpatialLocatorAttachedFrameOfReference クラスは、現実世界ではなく、デバイスに対して相対的である座標系を定義します。</span><span class="sxs-lookup"><span data-stu-id="805cb-228">The SpatialLocatorAttachedFrameOfReference class defines coordinate systems which are relative to the device rather than to the real-world.</span></span> <span data-ttu-id="805cb-229">このフレームは、方向、ユーザーのポイントは、参照フレームが作成されたときに直面していましたが、ユーザーの環境の基準とした固定の見出しが。</span><span class="sxs-lookup"><span data-stu-id="805cb-229">This frame has a fixed heading relative to the user's surroundings that points in the direction the user was facing when the reference frame was created.</span></span> <span data-ttu-id="805cb-230">このフレームのリファレンス内のすべての向きは、ユーザー、デバイスを回転しても、その固定の見出しを基準とは。</span><span class="sxs-lookup"><span data-stu-id="805cb-230">From then on, all orientations in this frame of reference are relative to that fixed heading, even as the user rotates the device.</span></span>

<span data-ttu-id="805cb-231">HoloLens、このフレームの座標系の原点は配置されているユーザーの頭の回転の中心に回転、ヘッドの位置が受けないようにします。</span><span class="sxs-lookup"><span data-stu-id="805cb-231">For HoloLens, the origin of this frame's coordinate system is located at the center of rotation of the user's head, so that its position is not affected by head rotation.</span></span> <span data-ttu-id="805cb-232">アプリには、このポイント ホログラムも、ユーザーの位置を基準としたオフセットを指定できます。</span><span class="sxs-lookup"><span data-stu-id="805cb-232">Your app can specify an offset relative to this point to position holograms in front of the user.</span></span>

<span data-ttu-id="805cb-233">SpatialLocatorAttachedFrameOfReference を取得するには、SpatialLocator クラスを使用し、CreateAttachedFrameOfReferenceAtCurrentHeading を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="805cb-233">To get a SpatialLocatorAttachedFrameOfReference, use the SpatialLocator class and call CreateAttachedFrameOfReferenceAtCurrentHeading.</span></span>

<span data-ttu-id="805cb-234">これが全体の範囲の Windows Mixed Reality デバイスに適用されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="805cb-234">Note that this applies to the entire range of Windows Mixed Reality devices.</span></span>

### <a name="use-a-reference-frame-attached-to-the-device"></a><span data-ttu-id="805cb-235">デバイスに接続されている参照フレームを使用して、</span><span class="sxs-lookup"><span data-stu-id="805cb-235">Use a reference frame attached to the device</span></span>

<span data-ttu-id="805cb-236">これらのセクションでは、この API を使用してデバイスに接続されたフレームの参照を有効にする、Windows Holographic のアプリケーション テンプレートで変更点について説明します。</span><span class="sxs-lookup"><span data-stu-id="805cb-236">These sections talk about what we changed in the Windows Holographic app template to enable a device-attached frame of reference using this API.</span></span> <span data-ttu-id="805cb-237">この「添付」ホログラムが固定または固定のホログラムに連動し、デバイスは、世界中でその位置を検索する一時的にできない場合にも使用される可能性がありますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="805cb-237">Note that this "attached" hologram will work alongside stationary or anchored holograms, and may also be used when the device is temporarily unable to find its position in the world.</span></span>

<span data-ttu-id="805cb-238">最初に、SpatialStationaryFrameOfReference ではなく、SpatialLocatorAttachedFrameOfReference を格納するテンプレートを変更します。</span><span class="sxs-lookup"><span data-stu-id="805cb-238">First, we changed the template to store a SpatialLocatorAttachedFrameOfReference instead of a SpatialStationaryFrameOfReference:</span></span>

<span data-ttu-id="805cb-239">**HolographicTagAlongSampleMain.h**:</span><span class="sxs-lookup"><span data-stu-id="805cb-239">From **HolographicTagAlongSampleMain.h**:</span></span>

```
   // A reference frame attached to the holographic camera.
   Windows::Perception::Spatial::SpatialLocatorAttachedFrameOfReference^   m_referenceFrame;
```

<span data-ttu-id="805cb-240">**HolographicTagAlongSampleMain.cpp**:</span><span class="sxs-lookup"><span data-stu-id="805cb-240">From **HolographicTagAlongSampleMain.cpp**:</span></span>

```
   // In this example, we create a reference frame attached to the device.
   m_referenceFrame = m_locator->CreateAttachedFrameOfReferenceAtCurrentHeading();
```

<span data-ttu-id="805cb-241">更新中に今すぐフレーム予測使用から取得されたタイムスタンプの座標系を取得します。</span><span class="sxs-lookup"><span data-stu-id="805cb-241">During the update, we now obtain the coordinate system at the time stamp obtained from with the frame prediction.</span></span>

```
   // Next, we get a coordinate system from the attached frame of reference that is
   // associated with the current frame. Later, this coordinate system is used for
   // for creating the stereo view matrices when rendering the sample content.
   SpatialCoordinateSystem^ currentCoordinateSystem =
       m_referenceFrame->GetStationaryCoordinateSystemAtTimestamp(prediction->Timestamp);
```

### <a name="get-a-spatial-pointer-pose-and-follow-the-users-gaze"></a><span data-ttu-id="805cb-242">空間ポインター姿勢を取得し、次のユーザーの視線入力</span><span class="sxs-lookup"><span data-stu-id="805cb-242">Get a spatial pointer pose, and follow the user's Gaze</span></span>

<span data-ttu-id="805cb-243">ユーザーのフォローを例ホログラムする[視線](gaze.md)と同様に、holographic シェルが、ユーザーの視線の先に従います。</span><span class="sxs-lookup"><span data-stu-id="805cb-243">We want our example hologram to follow the user's [gaze](gaze.md), similar to how the holographic shell can follow the user's gaze.</span></span> <span data-ttu-id="805cb-244">これは、同じタイムスタンプから、SpatialPointerPose を取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="805cb-244">For this, we need to get the SpatialPointerPose from the same time stamp.</span></span>

```
SpatialPointerPose^ pose = SpatialPointerPose::TryGetAtTimestamp(currentCoordinateSystem, prediction->Timestamp);
```

<span data-ttu-id="805cb-245">この SpatialPointerPose がに従ってホログラムを配置するための情報、[ユーザーの現在の針路](gaze,-gestures,-and-motion-controllers-in-directx.md)します。</span><span class="sxs-lookup"><span data-stu-id="805cb-245">This SpatialPointerPose has the information needed to position the hologram according to the [user's current heading](gaze,-gestures,-and-motion-controllers-in-directx.md).</span></span>

<span data-ttu-id="805cb-246">快適性の理由から、時間の期間にわたって実行されるように、位置の変更を滑らかにするのに線形補間 ("lerp") を使用します。</span><span class="sxs-lookup"><span data-stu-id="805cb-246">For reasons of user comfort, we use linear interpolation ("lerp") to smooth the change in position such that it occurs over a period of time.</span></span> <span data-ttu-id="805cb-247">これは、視線の先にホログラムのロックよりも、ユーザーに快適です。</span><span class="sxs-lookup"><span data-stu-id="805cb-247">This is more comfortable for the user than locking the hologram to their gaze.</span></span> <span data-ttu-id="805cb-248">Lerping tag-along ホログラムの位置では移動; をダンプしてホログラムを安定化することもできます。このダンプ私たちは、ユーザーに通常とは、ユーザーの頭の見えない動きと見なされますためジッター ホログラムと表示されます。</span><span class="sxs-lookup"><span data-stu-id="805cb-248">Lerping the tag-along hologram's position also allows us to stabilize the hologram by dampening the movement; if we did not do this dampening, the user would see the hologram jitter because of what are normally considered to be imperceptible movements of the user's head.</span></span>

<span data-ttu-id="805cb-249">**StationaryQuadRenderer::PositionHologram**:</span><span class="sxs-lookup"><span data-stu-id="805cb-249">From **StationaryQuadRenderer::PositionHologram**:</span></span>

```
   const float& dtime = static_cast<float>(timer.GetElapsedSeconds());

   if (pointerPose != nullptr)
   {
       // Get the gaze direction relative to the given coordinate system.
       const float3 headPosition  = pointerPose->Head->Position;
       const float3 headDirection = pointerPose->Head->ForwardDirection;

       // The tag-along hologram follows a point 2.0m in front of the user's gaze direction.
       static const float distanceFromUser = 2.0f; // meters
       const float3 gazeAtTwoMeters = headPosition + (distanceFromUser * headDirection);

       // Lerp the position, to keep the hologram comfortably stable.
       auto lerpedPosition = lerp(m_position, gazeAtTwoMeters, dtime * c_lerpRate);

       // This will be used as the translation component of the hologram's
       // model transform.
       SetPosition(lerpedPosition);
   }
```

>[!NOTE]
><span data-ttu-id="805cb-250">デバッグのパネルの場合、ビューが隠されることができるように、少し側にオフ ホログラム位置を変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="805cb-250">In the case of a debugging panel, you might choose to reposition the hologram off to the side a little so that it does not obstruct your view.</span></span> <span data-ttu-id="805cb-251">次を行う方法の例に示します。</span><span class="sxs-lookup"><span data-stu-id="805cb-251">Here's an example of how you might do that.</span></span>

<span data-ttu-id="805cb-252">**StationaryQuadRenderer::PositionHologram**:</span><span class="sxs-lookup"><span data-stu-id="805cb-252">For **StationaryQuadRenderer::PositionHologram**:</span></span>

```
       // If you're making a debug view, you might not want the tag-along to be directly in the
       // center of your field of view. Use this code to position the hologram to the right of
       // the user's gaze direction.
       /*
       const float3 offset = float3(0.13f, 0.0f, 0.f);
       static const float distanceFromUser = 2.2f; // meters
       const float3 gazeAtTwoMeters = headPosition + (distanceFromUser * (headDirection + offset));
       */
```

### <a name="rotate-the-hologram-to-face-the-camera"></a><span data-ttu-id="805cb-253">カメラに直面するホログラムを回転させる</span><span class="sxs-lookup"><span data-stu-id="805cb-253">Rotate the hologram to face the camera</span></span>

<span data-ttu-id="805cb-254">単にクワッド; をこの例では、ホログラムを配置するには不十分です。ユーザーが直面するオブジェクトを回転する必要がありますもできます。</span><span class="sxs-lookup"><span data-stu-id="805cb-254">It is not enough to simply position the hologram, which in this case is a quad; we must also rotate the object to face the user.</span></span> <span data-ttu-id="805cb-255">ビルボード処理のこの型では、ユーザーの環境の一部を維持するホログラムのため、ワールド空間で回転が発生することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="805cb-255">Note that this rotation occurs in world space, because this type of billboarding allows the hologram to remain a part of the user's environment.</span></span> <span data-ttu-id="805cb-256">ビュー空間ビルボード処理やわらげるできないためはホログラムが画面の向きをロックその場合は、ステレオのレンダリングが妨害しないビュー空間ビルボードのトランス フォームを取得するには左右の view 行列間を補間も必要があります。</span><span class="sxs-lookup"><span data-stu-id="805cb-256">View-space billboarding is not as comfortable because the hologram becomes locked to the display orientation; in that case, you would also have to interpolate between the left and right view matrices in order to acquire a view-space billboard transform that does not disrupt stereo rendering.</span></span> <span data-ttu-id="805cb-257">ここでは、ユーザーが直面する X、Z 軸に回転させます。</span><span class="sxs-lookup"><span data-stu-id="805cb-257">Here, we rotate on the X and Z axes to face the user.</span></span>

<span data-ttu-id="805cb-258">**StationaryQuadRenderer::Update**:</span><span class="sxs-lookup"><span data-stu-id="805cb-258">From **StationaryQuadRenderer::Update**:</span></span>

```
   // Seconds elapsed since previous frame.
   const float& dTime = static_cast<float>(timer.GetElapsedSeconds());

   // Create a direction normal from the hologram's position to the origin of person space.
   // This is the z-axis rotation.
   XMVECTOR facingNormal = XMVector3Normalize(-XMLoadFloat3(&m_position));

   // Rotate the x-axis around the y-axis.
   // This is a 90-degree angle from the normal, in the xz-plane.
   // This is the x-axis rotation.
   XMVECTOR xAxisRotation = XMVector3Normalize(XMVectorSet(XMVectorGetZ(facingNormal), 0.f, -XMVectorGetX(facingNormal), 0.f));

   // Create a third normal to satisfy the conditions of a rotation matrix.
   // The cross product  of the other two normals is at a 90-degree angle to
   // both normals. (Normalize the cross product to avoid floating-point math
   // errors.)
   // Note how the cross product will never be a zero-matrix because the two normals
   // are always at a 90-degree angle from one another.
   XMVECTOR yAxisRotation = XMVector3Normalize(XMVector3Cross(facingNormal, xAxisRotation));

   // Construct the 4x4 rotation matrix.

   // Rotate the quad to face the user.
   XMMATRIX rotationMatrix = XMMATRIX(
       xAxisRotation,
       yAxisRotation,
       facingNormal,
       XMVectorSet(0.f, 0.f, 0.f, 1.f)
       );

   // Position the quad.
   const XMMATRIX modelTranslation = XMMatrixTranslationFromVector(XMLoadFloat3(&m_position));

   // The view and projection matrices are provided by the system; they are associated
   // with holographic cameras, and updated on a per-camera basis.
   // Here, we provide the model transform for the sample hologram. The model transform
   // matrix is transposed to prepare it for the shader.
   XMStoreFloat4x4(&m_modelConstantBufferData.model, XMMatrixTranspose(rotationMatrix * modelTranslation));
```

### <a name="render-the-attached-hologram"></a><span data-ttu-id="805cb-259">アタッチされたホログラムをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="805cb-259">Render the attached hologram</span></span>

<span data-ttu-id="805cb-260">この例では、ホログラムを配置する場所は、SpatialLocatorAttachedReferenceFrame の座標システムでホログラムを表示するために選択もできます。</span><span class="sxs-lookup"><span data-stu-id="805cb-260">For this example, we also choose to render the hologram in the coordinate system of the SpatialLocatorAttachedReferenceFrame, which is where we positioned the hologram.</span></span> <span data-ttu-id="805cb-261">(別の座標系を使用して表示することにしましたが場合は、必要がありますをその座標系をデバイスに接続された参照フレームの座標システムからの変換を取得します。)</span><span class="sxs-lookup"><span data-stu-id="805cb-261">(If we had decided to render using another coordinate system, we would need to acquire a transform from the device-attached reference frame's coordinate system to that coordinate system.)</span></span>

<span data-ttu-id="805cb-262">**HolographicTagAlongSampleMain::Render**:</span><span class="sxs-lookup"><span data-stu-id="805cb-262">From **HolographicTagAlongSampleMain::Render**:</span></span>

```
   // The view and projection matrices for each holographic camera will change
   // every frame. This function refreshes the data in the constant buffer for
   // the holographic camera indicated by cameraPose.
   pCameraResources->UpdateViewProjectionBuffer(
       m_deviceResources,
       cameraPose,
       m_referenceFrame->GetStationaryCoordinateSystemAtTimestamp(prediction->Timestamp)
       );
```

<span data-ttu-id="805cb-263">以上で作業は終了です。</span><span class="sxs-lookup"><span data-stu-id="805cb-263">That's it!</span></span> <span data-ttu-id="805cb-264">ホログラムはようになりました「追跡」ユーザーの視線入力方向の前に 2 つのメーターの位置。</span><span class="sxs-lookup"><span data-stu-id="805cb-264">The hologram will now "chase" a position that is 2 meters in front of the user's gaze direction.</span></span>

>[!NOTE]
><span data-ttu-id="805cb-265">この例は、またその他のコンテンツを読み込みます - StationaryQuadRenderer.cpp を参照してください。</span><span class="sxs-lookup"><span data-stu-id="805cb-265">This example also loads additional content - see StationaryQuadRenderer.cpp.</span></span>

## <a name="handling-tracking-loss"></a><span data-ttu-id="805cb-266">処理の追跡が失われる</span><span class="sxs-lookup"><span data-stu-id="805cb-266">Handling tracking loss</span></span>

<span data-ttu-id="805cb-267">デバイスは、世界で自体を見つけられない、アプリで"追跡損失"が発生します。</span><span class="sxs-lookup"><span data-stu-id="805cb-267">When the device cannot locate itself in the world, the app experiences "tracking loss".</span></span> <span data-ttu-id="805cb-268">Windows Mixed Reality アプリでは、位置指定の追跡システムには、このような中断を処理できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="805cb-268">Windows Mixed Reality apps should be able to handle such disruptions to the positional tracking system.</span></span> <span data-ttu-id="805cb-269">これらの中断が見られることと既定 SpatialLocator LocatabilityChanged イベントを使用して、応答を作成します。</span><span class="sxs-lookup"><span data-stu-id="805cb-269">These disruptions can be observed, and responses created, by using the LocatabilityChanged event on the default SpatialLocator.</span></span>

<span data-ttu-id="805cb-270">**AppMain::SetHolographicSpace:**</span><span class="sxs-lookup"><span data-stu-id="805cb-270">From **AppMain::SetHolographicSpace:**</span></span>

```
   // Be able to respond to changes in the positional tracking state.
   m_locatabilityChangedToken =
       m_locator->LocatabilityChanged +=
           ref new Windows::Foundation::TypedEventHandler<SpatialLocator^, Object^>(
               std::bind(&HolographicApp1Main::OnLocatabilityChanged, this, _1, _2)
               );
```

<span data-ttu-id="805cb-271">アプリが LocatabilityChanged イベントを受け取るときに、必要に応じて、動作を変更できます。</span><span class="sxs-lookup"><span data-stu-id="805cb-271">When your app receives a LocatabilityChanged event, it can change behavior as needed.</span></span> <span data-ttu-id="805cb-272">など PositionalTrackingInhibited 状態でアプリは通常の操作を一時停止し、レンダリング、 [tag-along ホログラム](coordinate-systems-in-directx.md#create-holograms-using-a-device-attached-frame-of-reference)警告メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="805cb-272">For example, in the PositionalTrackingInhibited state, your app can pause normal operation and render a [tag-along hologram](coordinate-systems-in-directx.md#create-holograms-using-a-device-attached-frame-of-reference) that displays a warning message.</span></span>

<span data-ttu-id="805cb-273">Windows Holographic のアプリ テンプレートが付属 LocatabilityChanged ハンドラーが既に自動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="805cb-273">The Windows Holographic app template comes with a LocatabilityChanged handler already created for you.</span></span> <span data-ttu-id="805cb-274">既定では、その警告が表示されます、デバッグ コンソールで位置指定の追跡が使用できない場合。</span><span class="sxs-lookup"><span data-stu-id="805cb-274">By default, it displays a warning in the debug console when positional tracking is unavailable.</span></span> <span data-ttu-id="805cb-275">必要に応じて、アプリからの応答を提供するには、このハンドラーにコードを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="805cb-275">You can add code to this handler to provide a response as needed from your app.</span></span>

<span data-ttu-id="805cb-276">**AppMain.cpp:**</span><span class="sxs-lookup"><span data-stu-id="805cb-276">From **AppMain.cpp:**</span></span>

```
   void HolographicApp1Main::OnLocatabilityChanged(SpatialLocator^ sender, Object^ args)
   {
       switch (sender->Locatability)
       {
       case SpatialLocatability::Unavailable:
           // Holograms cannot be rendered.
           {
               String^ message = L"Warning! Positional tracking is " +
                                           sender->Locatability.ToString() + L".\n";
               OutputDebugStringW(message->Data());
           }
           break;

       // In the following three cases, it is still possible to place holograms using a
       // SpatialLocatorAttachedFrameOfReference.
       case SpatialLocatability::PositionalTrackingActivating:
           // The system is preparing to use positional tracking.

       case SpatialLocatability::OrientationOnly:
           // Positional tracking has not been activated.

       case SpatialLocatability::PositionalTrackingInhibited:
           // Positional tracking is temporarily inhibited. User action may be required
           // in order to restore positional tracking.
           break;

       case SpatialLocatability::PositionalTrackingActive:
           // Positional tracking is active. World-locked content can be rendered.
           break;
       }
   }
```

## <a name="spatial-mapping"></a><span data-ttu-id="805cb-277">空間マッピング</span><span class="sxs-lookup"><span data-stu-id="805cb-277">Spatial mapping</span></span>

<span data-ttu-id="805cb-278">[空間マッピング](spatial-mapping-in-directx.md)Api を使用する画面のメッシュのモデルの変換を取得する座標系を使用します。</span><span class="sxs-lookup"><span data-stu-id="805cb-278">The [spatial mapping](spatial-mapping-in-directx.md) APIs make use of coordinate systems to get model transforms for surface meshes.</span></span>

## <a name="see-also"></a><span data-ttu-id="805cb-279">関連項目</span><span class="sxs-lookup"><span data-stu-id="805cb-279">See also</span></span>
* [<span data-ttu-id="805cb-280">座標系</span><span class="sxs-lookup"><span data-stu-id="805cb-280">Coordinate systems</span></span>](coordinate-systems.md)
* [<span data-ttu-id="805cb-281">空間のアンカー</span><span class="sxs-lookup"><span data-stu-id="805cb-281">Spatial anchors</span></span>](spatial-anchors.md)
* <span data-ttu-id="805cb-282"><a href="https://docs.microsoft.com/azure/spatial-anchors" target="_blank">Azure の空間アンカー</a></span><span class="sxs-lookup"><span data-stu-id="805cb-282"><a href="https://docs.microsoft.com/azure/spatial-anchors" target="_blank">Azure Spatial Anchors</a></span></span>
* [<span data-ttu-id="805cb-283">視線、ジェスチャ、および DirectX でモーション コント ローラー</span><span class="sxs-lookup"><span data-stu-id="805cb-283">Gaze, gestures, and motion controllers in DirectX</span></span>](gaze,-gestures,-and-motion-controllers-in-directx.md)
* [<span data-ttu-id="805cb-284">DirectX での空間のマッピング</span><span class="sxs-lookup"><span data-stu-id="805cb-284">Spatial mapping in DirectX</span></span>](spatial-mapping-in-directx.md)