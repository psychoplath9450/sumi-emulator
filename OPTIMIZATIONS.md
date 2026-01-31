# Pokemon Red E-Ink Optimization Guide

## Overview

At **2Hz refresh rate**, every screen change costs ~500ms. The goal is to minimize unnecessary screen changes while maintaining playability.

---

## 🎯 HIGH IMPACT OPTIMIZATIONS

### 1. ✅ Skip Intro (Already Done)
**Impact: HUGE** - Saves 30+ seconds of rapidly changing screens  
**Status:** Your "No Intro" ROM already has this

### 2. 🔴 Remove Battle Flash/Transition
**Impact: HUGE** - The biggest e-ink killer  
**File:** `engine/battle/battle_transitions.asm`

The battle transition flashes the screen 6-8 times with inverted colors. On e-ink this looks terrible and wastes 3-4 seconds.

**Fix:** Replace with instant cut to battle
```asm
; In BattleTransition_FlashScreen_:
; Change the palette loop to just return immediately
BattleTransition_FlashScreen_:
    ret  ; Skip all flashing
```

Also in `engine/battle/animations.asm`:
```asm
AnimationFlashScreen:
    ret  ; Skip the invert-flash effect
```

### 3. 🔴 Instant Text (No Typewriter)
**Impact: HIGH** - Text normally prints character-by-character  
**File:** `home/print_text.asm`

**Fix:** Make PrintLetterDelay return immediately:
```asm
PrintLetterDelay::
    ret  ; Text appears instantly
```

Or set the frame delay to 0:
```asm
; In the .waitOneFrame section, change:
    ld a, 1
; To:
    ld a, 0
```

### 4. 🔴 Skip Battle Animations
**Impact: HIGH** - Move animations cause lots of screen changes  
**File:** `engine/battle/animations.asm`

The game already has an option for this (Options → Battle Animation → Off), but you can force it:
```asm
; In wOptions initialization (engine/menus/main_menu.asm)
; Set BIT_BATTLE_ANIMATION by default
    ld a, 1 << BIT_BATTLE_ANIMATION  ; bit 7 = animations off
    ld [wOptions], a
```

### 5. 🔴 Remove Screen Shake
**Impact: MEDIUM** - Damage causes screen shake  
**File:** `engine/battle/animations.asm`

```asm
; Replace all shake functions with ret
ShakeScreenVertically:
    ret

ShakeScreenHorizontallyHeavy:
    ret

ShakeScreenHorizontallySlow:
    ret

AnimationShakeScreenHorizontallySlow:
    ret
```

### 6. 🔴 Remove Wild Encounter "Grass Rustle"
**Impact: MEDIUM** - Animation before battle  
**File:** `engine/battle/wild_encounters.asm`

The grass rustling animation can be skipped by jumping directly to the battle.

---

## 🟡 MEDIUM IMPACT OPTIMIZATIONS

### 7. HP Bar - Instant Update
**Impact: MEDIUM** - HP bar animates down tick by tick  
**File:** `engine/gfx/hp_bar.asm`

**Fix:** Set HP difference directly instead of animating:
```asm
UpdateHPBar2:
    ; Just copy new HP to current, skip animation
    ld a, [wHPBarNewHP]
    ld [wHPBarOldHP], a
    ld a, [wHPBarNewHP+1]
    ld [wHPBarOldHP+1], a
    ret
```

### 8. Remove "Got Away Safely" Delay
**Impact: LOW-MEDIUM** - Unnecessary pause after fleeing  
**File:** `engine/battle/core.asm`

The game waits after displaying "Got away safely!" - this can be shortened.

### 9. Evolution Animation
**Impact: MEDIUM** - Evolution has flashing and morphing  
**File:** `engine/pokemon/evos_moves.asm`

Can be replaced with instant evolution (just swap sprites).

### 10. Learning New Move Animation
**Impact: LOW** - Brief animation when learning moves  
Skip the celebratory animation.

---

## 🟢 NICE-TO-HAVE OPTIMIZATIONS

### 11. Map Transition Speed
Fade in/out between maps can be instant.

### 12. PC Box Animation
Opening/closing PC can skip the animation.

### 13. Pokedex Cry Delay
Skip the "cries" since there's no speaker anyway.

### 14. Pokeball Throw Animation
The ball wobble can be reduced to just show catch/fail result.

### 15. Item Use Animation
Using potions/items has a brief effect - can be instant.

---

## 🛠️ IMPLEMENTATION APPROACHES

### Option A: ROM Patching (IPS/BPS)
Create patches that modify the original ROM:
- Pro: Can be distributed legally
- Pro: Works with any base ROM
- Con: Requires patching tools

### Option B: Emulator-Side Patches
Apply patches at runtime in the emulator:
- Pro: No ROM modification needed
- Pro: Can toggle on/off
- Con: More complex emulator code

### Option C: Full Rebuild from pokered
Build a custom ROM from the disassembly:
- Pro: Most flexibility
- Pro: Can combine with other enhancements
- Con: Requires build environment (rgbds)

---

## 📋 SPECIFIC BYTE PATCHES

Here are some specific hex patches for a standard Pokemon Red (USA) ROM:

### Skip Battle Flash (Approximate)
The battle transition code is around offset `0x3F3D5`. The exact bytes depend on the ROM version.

### Force Instant Text
In `wOptions` (RAM $D355), bit 0-2 control text speed:
- `001` = Fast
- `011` = Medium  
- `101` = Slow
- `000` = Instant (if we patch PrintLetterDelay)

### Disable Battle Animations by Default
Set bit 7 of `wOptions` on game start.

---

## 🎮 ROM PATCHES TO STACK

Your optimized ROM should combine:

1. ✅ **No Intro** (you have this)
2. 🔴 **No Battle Flash** - critical for e-ink
3. 🔴 **Instant Text** - major QoL improvement
4. 🔴 **No Battle Animations** - force OFF by default
5. 🔴 **No Screen Shake** - e-ink hates shake
6. 🟡 **Instant HP Bar** - smoother experience
7. 🟡 **Faster Map Transitions** - less waiting

---

## 📊 ESTIMATED IMPACT

| Optimization | Screen Changes Saved | Time Saved Per Instance |
|-------------|---------------------|------------------------|
| Skip Intro | ~60 frames | 30 seconds |
| No Battle Flash | ~16 frames | 8 seconds per battle |
| Instant Text | ~5-20 per dialog | 2-10 seconds per dialog |
| No Battle Animations | ~30-100 per move | 15-50 seconds per battle |
| No Screen Shake | ~4-8 per hit | 2-4 seconds per hit |
| Instant HP Bar | ~10-50 per damage | 5-25 seconds per damage |

**A typical wild battle could go from 45+ seconds to under 10 seconds!**

---

## 🔧 BUILD INSTRUCTIONS (if using pokered)

```bash
# Install rgbds (assembler)
# On Ubuntu/Debian:
sudo apt install rgbds

# Clone pokered
git clone https://github.com/pret/pokered

# Apply modifications to .asm files

# Build
make red
# Outputs: pokered.gbc
```

---

## 🌐 EXISTING HACKS TO CONSIDER

1. **Shin Pokemon Red** - Has many QoL features, instant text option
2. **Pokemon Fast Red** - Running shoes, faster movement
3. **Pokemon Yume** - Many QoL enhancements

You could potentially combine patches from these with e-ink specific ones.

---

## NEXT STEPS

1. I can create specific IPS/BPS patches for these optimizations
2. We can integrate patch detection into the emulator
3. We can add a "patch menu" to the browser emulator to toggle options
4. Eventually build a fully optimized ROM from pokered source

Which approach would you like to pursue?
