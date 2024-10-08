# UE5-BP-InspectMode

Steps:
1) Create UI widget WBP_Player
- Add crosshair and F prompt
- Add Set F prompt custom event
- Create the widget, promote and attach in the player BP

2) Create inputs
- Add actions
-- IA_EnterInspect
-- IA_ExitInspect
-- IA_RotateInspect
-- IA_MysteryInspect
- Add context IMC_Inspect

3) Create BP_InspectItem (alternative to interfaces)
- Set cup mesh
- Set collision to IgnoreOnlyPawn
- Briefly test inputs by swapping context

4) Line Trace to Inspect
- Add a scene component under camera, about +40 X, to hold inspected item
- Create a bool Is Inspecting
- In Tick, If Not Is Inspecting, Line Trace to find the cup
-- If successful, cast to BP and promote to Current Inspected Item. Else, set a "no target" empty variable to it.
-- Also set the F prompt properly
- IA_EnterInspect
-- If not Is Inspecting and If Check Current Inspected Item is valid
-- Set Is Inspecting to True
-- Set F Prompt to False
-- Reset World Rotation of Inspect Origin
-- Save initial transform of item before moving it
-- Attach item to origin with Snap
-- Swap input mapping contexts
- IA_ExitInspect
--If inspecting, set it to False
--Detach and set Transform back to initial
--Swap input mapping contexts

5) Rotate item
- IA_RotateInspect
- Use CombineRotators when combining rotations cumulatively
-- A: InspectOrigin Relative Rotation
-- B: Action X -> Z (Yaw), Action Y -> Y (Pitch)
- Set Relative Rotation with the result

6) Mystery Line Trace
- Add mystery mesh (gem) to inspect item (cup)
- Set mesh collision also to IgnoreOnlyPawn
- Tag mesh as "Mystery"
- In player's Tick, If Inspecting:
- Line Trace
-- From Camera to Mystery component
-- Set Trace Complex
- Check if Has Component Mystery
- Set Bool Is Mystery Inspecting
- Set F Prompt = Is Inspecting Mystery

7) Mystery Inspect (Interact / Grab)
- If Mysery Inspecting, Set Bool to False
- Fire custom event Mystery Inspect on item
-- Set visibility of component to false
-- Set collision to none
- Set F Prompt = False


Japanese Cup:
https://sketchfab.com/3d-models/japanese-tea-cup-506f16847d0a4565bdf5d4fa4c358546

Gem:
https://sketchfab.com/3d-models/gem-3f5bc61910fb4be2b5a9671c4ff9cce3

Enhanced Input Doc:
https://dev.epicgames.com/documentation/en-us/unreal-engine/enhanced-input-in-unreal-engine