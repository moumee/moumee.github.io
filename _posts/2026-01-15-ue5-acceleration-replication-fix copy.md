---
title: "Unreal Engine 5 Acceleration Replication Fix"
date: 2026-01-15 00:00:00 +0900
categories: [Devlog, Unreal Engine 5, Multiplayer]
tags: [ue5, multiplayer]

---

I was working on a solo multiplayer ue5 project and faced upon a problem where acceleration does not replicate properly. Due to this problem, some of my animation state transitions which were using a bool to check if the character has acceleration were delayed a lot.

In **ACharacter** class, you can find **ShouldReplicateAcceleration** function.

```cpp
 /** Whether this Character should include acceleration data in its replicated movement */
 virtual bool ShouldReplicateAcceleration() const { return CharacterCVars::EnableCharacterAccelerationReplication != 0; }
```

If you inspect the function and find its usages, you can see that it is used in **ACharacter::GatherCurrentMovement** function.

```cpp
void ACharacter::GatherCurrentMovement()
{
    Super::GatherCurrentMovement();

    if (IsReplicatingMovement())
    {
    FRepMovement& MutableRepMovement = GetReplicatedMovement_Mutable();

    if (ShouldReplicateAcceleration()) 
    {
        MutableRepMovement.bRepAcceleration = true;
        MutableRepMovement.Acceleration = CharacterMovement->GetCurrentAcceleration();
    }
    else
    {
        MutableRepMovement.bRepAcceleration = false;
    }
    }
}
```

To enable this acceleration replication feature, you can either override the **ACharacter::ShouldReplicateAcceleration** function or use the CVar **p.EnableCharacterAccelerationReplication 1**.

This feature was crucial for my project since it uses distance matched stops and heavily relies on the transition based on whether there is acceleration or not.

Below is the before & after result video of acceleration replication.

<video src="/assets/video/BeforeAfterAccelRep.mp4" controls width="720"></video>
