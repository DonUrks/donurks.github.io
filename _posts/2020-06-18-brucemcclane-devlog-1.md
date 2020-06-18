---
title: BruceMcClane devlog 1 - wfc and deadend rooms
date: 2020-06-18 23:11:00 +0200
layout: post
categories: [Gamedev, Devlog, BruceMcClane]
description: "My first try on wave function collapse and deadends with doors!"
---
#### My first try on wave function collapse
Heavy based on [mxgmn/WaveFunctionCollapse](https://github.com/mxgmn/WaveFunctionCollapse)

I have designed a training room for the wfc algorithm:

![WFC training room](/assets/bmc/devlog1/wfc_training.PNG)

The result is nice but still needs a lot of fine-tuning and content (furniture) so I postponed WFC for now :)

{% include mp4.html height="400" file="/assets/bmc/devlog1/wfc_generation" %}

{% include hr.html %}

#### Deadend rooms
First I connected all the rooms that are on the target path. This created rooms that the player can never reach.

![Deadend no doors](/assets/bmc/devlog1/deadend_nodoors.PNG)

My solution goes through all the rooms and sees if a room without a door is adjacent to a room with a door. If so, I connect these rooms. If there are several possibilities, a random room is picked. I do this until every room has a door.

```csharp
foreach (var room in withoutDoorRooms)
{
    var roomPossibilities = new List<Parts.Room>();

    // neighbor rooms
    foreach (var delta in deltas)
    {
        var checkRoom = rooms[checkPosition.x, checkPosition.y];
        if (checkRoom.HasDoor())
        {
            roomPossibilities.Add(checkRoom);
        }
    }

    var randomIndex = Random.Range(0, roomPossibilities.Count);
    var currentRoom = roomPossibilities[randomIndex];

    SetDoors(room, currentRoom);
}
```
![Deadend doors](/assets/bmc/devlog1/deadend_doors.PNG)
