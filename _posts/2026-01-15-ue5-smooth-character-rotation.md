---
title: "Unreal Engine 5 Smooth Character Rotation"
date: 2026-01-15 00:00:00 +2200
categories: [Devlog, Unreal Engine 5]
tags: [ue5, animation]

---

This is a short article of how I made smoother character rotation for a third person character movement.

In the Lyra sample project, visual rotation is separated from the capsule (actor) rotation. Interpolating the actual actor rotation can lead to unstable replication and an undesired control feel. By smoothing only the visual rotation, the logic can be simulated locally in each client’s animation blueprint without introducing additional replicated state.

A similar approach can also be found in the Game Animation Sample Project (GASP), where the Steering node is used to control the character’s facing direction at the animation level, rather than directly interpolating the actor rotation.

So I combined both concepts to smooth out my third person character movement. Similar to the Steering node, I made an additional rotation which smoothly lags behind the capsule (actor) rotation. And then I used the Rotate Root Bone node in the anim blueprint to give the actor mesh a visual yaw offset similar to how Lyra implemented turn in place.

```cpp
// Anim Instance header code

UPROPERTY(Transient, VisibleAnywhere, BlueprintReadOnly, Category="Animation")
float RootYawOffset;

FRotator SteeringRotation;

FRotator MeshRotation;


// Anim Instance cpp simplified code

void UExampleAnimInstance::NativeInitializeAnimation()
{
    Super::NativeInitializeAnimation();

    RootYawOffset = 0.f;
    SteeringRotation = GetSkelMeshComponent()->GetComponentRotation();
}

void UExampleAnimInstance::NativeUpdateAnimation(float DeltaSeconds)
{
    // Get other values etc...

    MeshRotation = GetSkelMeshComponent()->GetComponentRotation();
}

void UExampleAnimInstance::NativeThreadSafeUpdateAnimation(float DeltaSeconds)
{
    Super::NativeThreadSafeUpdateAnimation(DeltaSeconds);

    // Compute variables bHasVelocity, bHasAcceleration etc...

    if (bHasVelocity || bHasAcceleration)
    {
        SteeringRotation = FMath::RInterpTo(SteeringRotation, MeshRotation, DeltaSeconds, 10.f);
        RootYawOffset = FMath::FindDeltaAngleDegrees(MeshRotation.Yaw, SteeringRotation.Yaw);
    }
}
```

In the anim instance header, I made three variables that are needed for this method. In the NativeUpdateAnimation function, the pawn's mesh rotation is cached into the MeshRotation FRotator variable. And In the NativeThreadSafeUpdateAnimation function, the SteeringRotation is calculated using RInterpTo function which makes it smoothly follow the MeshRotation. And then RootYawOffset is calculated from the yaw delta of MeshRotation and SteeringRotation. Finally RootYawOffset value is applied to the mesh using the rotate root bone node in the anim blueprint.

Below is the before & after result of rotation smoothing.

<video src="/assets/video/SmoothYawOffsetRotation.mp4" controls width="720"></video>
