# GDD: Player Progression System

### Progression System Summary (GDD Overview)

This document outlines a player progression system designed for a modern ARPG. The core philosophy is to reward **strategic player choice** over sheer time investment. A player's level functions not as the primary source of power, but as a **key that gates access to greater power** through a deep loot system and a meaningful skill tree.

The system is built on three pillars:

1.  **Exponential XP Curve:** Ensures a fast-paced early game to maximize engagement and a challenging, long-term endgame for player retention.

	While other progression models (such as linear or logarithmic curves) were considered, the **exponential XP curve was specifically chosen** for this system. This decision ensures a better match for the classic RPG style, providing immediate early-game progress and a sustained, challenging grind for the long-term endgame.

2.  **Dynamic XP Modifier:** Actively guides players toward level-appropriate content by severely penalizing the farming of trivial enemies while rewarding skilled players who defeat higher-level foes.

3.  **Multi-Faceted Reward Structure:** Power is gained through three avenues:
    *   **Stats:** A balanced attribute system that defines a character's core archetype.
    *   **Skill Points:** The primary motivation for leveling, allowing tangible improvements to abilities.
    *   **Loot:** The ultimate driver of endgame power, with the best gear locked behind level requirements.

This design creates a robust and sustainable game loop where players are constantly motivated to challenge themselves to unlock the next tier of gear and skills, ensuring a deep and rewarding experience.

## 1. Design Philosophy

This document details the player progression system for a new ARPG. The core philosophy is that a player's power should be a direct reflection of their **strategic choices**, not just their time investment. A player's level is not the ultimate source of their power but rather the **key that unlocks access to greater power** through gear (Loot) and skill specialization.

This system is engineered to:
*   Naturally **guide the player** towards challenging and engaging content.
*   **Reward player risk-taking and skill**.
*   **Prevent stagnation** in trivial farming zones.
*   Create an addictive and sustainable game loop from the early game through the endgame.

## 2. Pillar I: The Player's Journey (Experience Curve)

The foundation of the player's journey is the experience (XP) requirement for leveling up. We have adopted an **exponential curve** to ensure a dynamic game pace.

### 2.1. Required XP Formula

The XP required to reach Level `LVL` is calculated using the following formula:

``` XP = BASE * ((LVL - 1) ^ EXP) + OFFSET ```

![Alt Curves](./curve_formulas.png)

*   **BASE:** Controls the curve's starting value.
*   **EXP:** The exponent that defines the curve's acceleration. A value of `1.5` ensures that the cost of each level increases significantly in the late game.
*   **OFFSET:** A flat value to fine-tune the curve if necessary.

![Alt Curve parameters](./curve_params.png)

### 2.2. Design Implications

*   **Early Game (Levels 1-40):** The curve is relatively gentle, allowing new players to level up quickly and feel a sense of constant progress, fostering initial engagement.
*   **Late Game (Levels 80+):** The curve steepens dramatically. Leveling becomes a major achievement, setting the expectation that gear and skills—not the level itself—are the primary drivers of power.

![Alt Table curve values](./lvl_vs_xp.png)

![Alt Curve Graphics LVL vs XP](./curve_graphs.png)

## 3. Pillar II: The Pacing Engine (XP Modifier by Level Differential)

To actively guide the player and control the game's pace, the XP gained from an enemy is not fixed. It is dynamically modified based on the level difference between the player and the enemy.

### 3.1. The Modifier Curve

The XP multiplier follows a carefully designed curve with three key phases:

![Alt Graph XP Multiplier vs. Level Difference](./graph_xp_mul_vs_lvl_dif.png)


*   **Phase 1: The Relevance Floor (Anti-Grind Mechanism)**
    *   **Description:** When a player is 8 or more levels higher than an enemy, the XP gained is reduced to a nominal value (e.g., 1 XP).
    *   **Psychological Effect:** The player learns, both visually and mechanically, that farming "gray" (trivial) enemies is a complete waste of time. This combats monotony and pushes the player to move forward.

*   **Phase 2: The Combat Zone (The S-Curve Ramp)**
    *   **Description:** Between a level difference of +7 and 0, the XP reward increases along an S-shaped curve. This creates a smooth transition from near-zero XP to the full 100% base reward.
    *   **Psychological Effect:** This defines the player's "comfort zone." Enemies within this range are the primary target for efficient and safe farming.

*   **Phase 3: The Heroic Incentive (Risk & Reward)**
    *   **Description:** For each level an enemy is higher than the player, a linear XP bonus is awarded (e.g., +10% per level).
    *   **Psychological Effect:** This directly rewards skill. A player who, through superior gear or flawless execution, defeats a higher-level enemy receives a tangible reward and feels powerful.

### 3.2. Modifier Table

| Difference (Player - Enemy) | XP Multiplier |
| :--- | :--- |
| +8 or more | 0.01x (1 XP) |
| +7 | 0.32x |
| +2 | 0.96x |
| 0 | 1.00x |
| -2 | 1.20x |
| -5 | 1.50x |
| -10 | 2.00x |
| -20 | 3.00x |

### XP Calculation Formula by Level Disparity

Let $L_P$ be the Player Level, $L_E$ the Enemy Level, and $XP_B$ the Enemy Base XP.

The Level Difference is defined as:
$$\Delta L = L_P - L_E$$

The Multiplier ($M$) is calculated in two conditional cases:

#### Case 1: Player Level $\leq$ Enemy Level ($\Delta L \leq 0$)
$$M_{Bonus} = \min \left( 1.0 + (-\Delta L \cdot 0.2), \quad 2.0 \right)$$

#### Case 2: Player Level $>$ Enemy Level ($\Delta L > 0$)
$$M_{Penalty} = 1.0 - \left( \frac{\Delta L}{5} \right)^{1.5}$$

***

#### Final Result (Clamping)

The final Multiplier ($M_{Final}$) is the result of the relevant case:
$$M_{Final} = \max \left( M_{Case}, \quad 0.01 \right)$$

The Final XP ($XP_F$) is calculated and clamped:
$$XP_F = \max \left( 1, \quad \lfloor XP_B \cdot M_{Final} \rfloor \right)$$

## 4. Pillar III: The Reward Structure

The effort of earning XP translates into power through three interconnected systems: Stats, Skills, and Loot.

### 4.1. Attribute System (Stats)

The character's core is defined by four primary attributes that scale linearly and are well-balanced:
*   **STR (Strength):** Powers physical damage (P_ATK), physical defense (P_DEF), and physical energy (SP).
*   **STA (Stamina):** The primary source of survivability (HP) and base defenses.
*   **DEX (Dexterity):** Governs agility, including attack speed (Speed), critical strike chance (Crit Chance), and contributes to physical energy (SP).
*   **INT (Intelligence):** Powers magical damage (M_ATK), magical defense (M_DEF), and magical energy (MP).

This design avoids a "God Stat" problem, forcing the player to make meaningful decisions about their specialization.

`[INSERT GRAPH: Stacked Bar Chart of Base Stats by Level]`

### 4.2. Skill Points

**The primary motivation for leveling up is to earn Skill Points.** Each "Ding!" grants points that the player invests in skill trees to improve the potency, add secondary effects, or reduce the costs of their active and passive abilities. This makes every level feel like an immediate and tangible reward.

### 4.3. Gear (Loot)

Gear is the main driver of power in the endgame. **Levels function as gates** that restrict access to the best equipment.
*   **Game Loop:** A player finds a legendary sword that requires Level 90. This creates a clear goal and motivates the player to use the XP system's "Heroic Incentive" to reach that level faster. Once equipped, that sword allows them to tackle even harder content, where they can find even better loot.

## 5. Risk Analysis & Balancing Levers

A well-designed system anticipates its own failure points.

*   **Risk 1: Multiplayer Power-Leveling.**
    *   **Problem:** A high-level player could abuse the "Heroic Incentive" to boost a low-level friend at an unintended, game-breaking pace.
    *   **Balancing Lever:** Implement a group XP formula that averages party member levels or applies a penalty if the level disparity within the group is too large.

*   **Risk 2: The "Wall of Frustration."**
    *   **Problem:** If enemy difficulty scales exponentially, the linear XP bonus may not be enough to justify the risk, creating a "wall" where players cannot progress.
    *   **Balancing Lever (Skills & Loot):** Skill and item affix design is crucial. Utility skills (stuns, slows, shields) and defensive gear effects (life steal, evasion) are the tools that will allow a skilled player to overcome a raw stat disadvantage.

*   **Risk 3: The "Useless Tank" Build.**
    *   **Problem:** A "Full STA" build might be immortal but have such low damage output that the game becomes boring to play.
    *   **Balancing Lever (Skills):** Design tank-specific skills whose damage scales not with STR/INT, but with defensive stats like Max HP or P_DEF. This ensures that all specializations are both viable and fun.

## 6. Conclusion

This progression system creates a robust symbiosis between player effort and reward. It intelligently guides the player, rewards boldness, and establishes a solid framework for a deep and lasting endgame driven by the hunt for legendary gear and the optimization of skill builds. The system is ready for implementation and subsequent fine-tuning during playtesting phases.