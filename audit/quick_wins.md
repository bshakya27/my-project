# Top 5 Quick Wins (1-2 Hours Total)

Quick wins are small changes that make a big visual difference with minimal code. These are the easiest improvements to implement and will immediately boost the visual feel.

---

## 1. **Add Glow Effects to Interactive Objects** (~15 min)

### What to Add
Add a subtle glow/shadow behind the core, forge, and towers to make them pop from the background.

### Implementation
Use `ctx.shadowColor` and `ctx.shadowBlur` before drawing each object:
```javascript
ctx.shadowColor = 'rgba(255, 200, 100, 0.6)';
ctx.shadowBlur = 15;
ctx.shadowOffsetX = 0;
ctx.shadowOffsetY = 0;
```

For towers, use a blue glow. For forge, use orange. For core, use gold.

### Before & After
- **Before:** Core, forge, towers sit flat on background
- **After:** Objects appear to float and have presence

### Visual Impact: +0.5 points

---

## 2. **Add Hit Flash to Enemies** (~15 min)

### What to Add
When an enemy is hit, flash it white for 100ms to show damage was taken.

### Implementation
Add a `hitFlashTime` field to each enemy. When hit, set it to 0.1. Decrement each frame.

When drawing enemies, if `hitFlashTime > 0`, blend toward white:
```javascript
const flashAlpha = Math.sin(hitFlashTime * Math.PI * 20) * 0.3; // oscillate
ctx.globalAlpha = 1 - flashAlpha;
ctx.fillStyle = '#ffffff';
ctx.beginPath();
ctx.arc(e.x, e.y, e.radius, 0, Math.PI * 2);
ctx.fill();
ctx.globalAlpha = 1;
```

### Before & After
- **Before:** Enemy takes hit, nothing visible
- **After:** Enemy flashes white briefly, then fades back to normal color

### Visual Impact: +1.0 points

---

## 3. **Add Projectile Trails** (~20 min)

### What to Add
Draw a faint line from the projectile's previous position to its current position, creating a motion trail.

### Implementation
Add `prevX` and `prevY` fields to projectiles (initialize from spawn position).

Before moving the projectile, store its position:
```javascript
p.prevX = p.x;
p.prevY = p.y;
```

After moving, draw a line trail:
```javascript
ctx.strokeStyle = p.owner === 'player' ? 'rgba(255, 243, 176, 0.3)' : 'rgba(176, 224, 255, 0.3)';
ctx.lineWidth = 1.5;
ctx.beginPath();
ctx.moveTo(p.prevX, p.prevY);
ctx.lineTo(p.x, p.y);
ctx.stroke();
```

### Before & After
- **Before:** Projectiles are hard to track
- **After:** Clear trails show projectile path and speed

### Visual Impact: +1.0 points

---

## 4. **Add Screen Shake on Core Damage** (~10 min)

### What to Add
Small camera shake when the core takes damage, emphasizing the impact.

### Implementation
Add a `screenShake` value to state, decrement it each frame.

When core is damaged:
```javascript
state.screenShake = 0.15; // shake intensity, in pixels
```

In render, apply offset before drawing:
```javascript
const shake = Math.random() * state.screenShake * 2 - state.screenShake;
ctx.save();
ctx.translate(shake, shake);
// draw everything
ctx.restore();

state.screenShake *= 0.95; // decay
```

### Before & After
- **Before:** Core damage is silent, no impact
- **After:** Screen wobbles, emphasizing danger

### Visual Impact: +0.8 points

---

## 5. **Enhance Projectile Visibility** (~10 min)

### What to Add
Make projectiles larger (6px → 8px), add an outline, and increase their contrast against the background.

### Implementation
In `drawProjectiles()`:
```javascript
for (const p of state.projectiles) {
  const color = p.owner === 'player' ? '#ffea54' : '#64d5ff'; // brighter colors
  ctx.fillStyle = color;
  ctx.strokeStyle = 'rgba(0, 0, 0, 0.3)';
  ctx.lineWidth = 2;
  ctx.beginPath();
  ctx.arc(p.x, p.y, 6, 0, Math.PI * 2); // larger radius
  ctx.fill();
  ctx.stroke();
  
  // Add a small glowing halo
  ctx.strokeStyle = color + '40'; // semi-transparent version
  ctx.lineWidth = 3;
  ctx.beginPath();
  ctx.arc(p.x, p.y, 8, 0, Math.PI * 2);
  ctx.stroke();
}
```

### Before & After
- **Before:** Projectiles are tiny, nearly invisible
- **After:** Clear, visible projectiles with glow

### Visual Impact: +0.8 points

---

## Summary

| # | Change | Time | Impact |
|---|--------|------|--------|
| 1 | Glow effects | 15 min | +0.5 |
| 2 | Hit flash | 15 min | +1.0 |
| 3 | Projectile trails | 20 min | +1.0 |
| 4 | Screen shake | 10 min | +0.8 |
| 5 | Better projectiles | 10 min | +0.8 |
| **TOTAL** | **5 changes** | **~70 min** | **+4.1 points** |

**Current score: 3.5/10**  
**After quick wins: 7.6/10** ✨

These are the highest-ROI improvements. Implement these first, then assess whether high-impact upgrades are needed.

