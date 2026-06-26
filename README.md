# Cart Chaos - Physics Sandbox Prototype

A high-fidelity, physics-driven co-op gameplay prototype developed in Unity 6, focusing on rigid locomotion, physical item manipulation, and advanced physics-based vehicle (shopping cart) mechanics. Designed as a modular sandbox to test physical interactions, drifting, and object carriage constraints.

---

## 🚀 Key Technical Highlights (For CV / Portfolio)

This prototype was built to explore the boundaries of deterministic physics and physical player interaction in Unity, replacing generic character controllers with a custom physics-driven architecture:

### 1. Rigidbody-Driven Locomotion System
* **Implementation:** Developed a custom locomotion controller using Unity's `Rigidbody` and `CapsuleCollider` (replacing the standard non-physics `CharacterController`).
* **Physics Integration:** Supports realistic reactions to external forces, collisions, velocity dampening, and custom gravity multipliers for a snappier jump/fall trajectory. 
* **State Management:** Fully handles walking, sprinting, and smooth crouching transitions with programmatic collider height adjustments and overhead obstruction checks (raycasting upwards before allowing the player to stand).

### 2. ConfigurableJoint-Based Physics Grabbing (Half-Life / REPO style)
* **Mechanics:** Carried items do not snap into a static local hierarchy. Instead, they are bound using a dynamic `ConfigurableJoint` and custom spring drives (`positionSpring` & `positionDamper`).
* **Environment Collisions:** Because held items remain active physical Rigidbodies, they realistically collide with doors, walls, and obstacles rather than clipping through them.
* **Break Constraints:** Implemented automatic joint-breaking distance checks if a carried item becomes trapped behind a solid structure, forcing the player to drop the item organically.

### 3. First-Person Camera with Sub-Mesh Culling
* **Dual-Hierarchy Rotation:** Uses a decoupled setup where horizontal camera movement rotates the player body, while vertical look is clamped and applied locally.
* **Culling Architecture:** Solves the classic first-person camera clipping issue (seeing inside the character's head mesh) by setting up a custom sub-mesh rendering layer (`InvisibleToSelf`). The camera culls the head mesh while rendering the torso, limbs, and casting realistic character shadows.

### 4. Advanced Shopping Cart Physics & Lateral Friction
* **Drift Dynamics:** The cart utilizes direct Rigidbody force application for forward pushing and torque for turning.
* **Side-Slip Friction:** Implemented lateral velocity calculations to apply a variable transverse grip factor. This allows the cart to slide and drift satisfyingly when rounding tight corners at speed.
* **Basket Cargo Tracking:** Tracks items sitting inside the cart using a trigger collider and filtering out null/destroyed objects dynamically to keep a reliable tally of cargo.

---

## 🛠️ Architecture & Scripts

The codebase is organized into decoupled, modular components located under `Assets/Scripts/`:

* **[PhysicsPlayerController.cs](file:///E:/UnityHub/downloadds/Project1/Assets/Scripts/Player/PhysicsPlayerController.cs):** Rigidbody locomotion, jumping, crouching, and snappy custom gravity.
* **[FirstPersonCamera.cs](file:///E:/UnityHub/downloadds/Project1/Assets/Scripts/Player/FirstPersonCamera.cs):** Dual-axis mouse look, cursor locking, and height-offset culling.
* **[PhysicsGrab.cs](file:///E:/UnityHub/downloadds/Project1/Assets/Scripts/Player/PhysicsGrab.cs):** Physics spring joint handler, raycast grab system, and distance break triggers.
* **[CartController.cs](file:///E:/UnityHub/downloadds/Project1/Assets/Scripts/Cart/CartController.cs):** Rigidbody force driver and drift controller for the cart.
* **[CartInteraction.cs](file:///E:/UnityHub/downloadds/Project1/Assets/Scripts/Cart/CartInteraction.cs):** Mount/dismount handles, and player-to-cart WASD control router.
* **[CartBasketDetector.cs](file:///E:/UnityHub/downloadds/Project1/Assets/Scripts/Cart/CartBasketDetector.cs):** Trigger scanning for grabbable items placed inside the basket.
* **[LootItem.cs](file:///E:/UnityHub/downloadds/Project1/Assets/Scripts/Items/LootItem.cs):** Lightweight tag and metadata container for grabbable physics items.

---

## 🎮 Setup & Playtesting Instructions

1. **Unity Version:** Developed and tested on Unity 6 (6.5+ recommended).
2. **Scene Setup:** Open `Assets/Scenes/Prototype1.unity`.
3. **Layer Configuration:** 
   * Add a layer named **`InvisibleToSelf`** and apply it exclusively to your player model's head/face mesh.
   * Uncheck `InvisibleToSelf` from the main camera's **Culling Mask** to enable head-exclusion culling.
4. **Grabbing Layer:**
   * Assign grabbable items (e.g., box models with `Rigidbody` and `LootItem` attached) to a layer (e.g. `Default` or `Loot`). Ensure this layer matches the **Grab Layer** mask in the `PhysicsGrab` inspector.
