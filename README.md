# Color4
A feature-rich color library for [Retro Gadgets](https://docs.retrogadgets.game/).

---

## Installation

Copy `Color4.luau` into your gadget's file system and require it:

```lua
local Color4 = require("Color4")
type Color4 = Color4.Color4
```

---

## Constructors

### `Color4.FromRGB(r, g, b, a?)`
Creates a color from red, green, blue and optional alpha channels.
- `r`, `g`, `b` — `0–255`
- `a` — `0–255`, defaults to `255`

```lua
local color = Color4.FromRGB(255, 80, 0)
local colorWithAlpha = Color4.FromRGB(255, 80, 0, 128)
```

---

### `Color4.FromHEX(hex)`
Creates a color from a HEX string. Supports both 6-character (`#RRGGBB`) and 8-character (`#RRGGBBAA`) formats.

```lua
local color = Color4.FromHEX("#FF5000")
local colorWithAlpha = Color4.FromHEX("#FF500080")
```

---

### `Color4.FromHSV(h, s, v, a?)`
Creates a color from Hue, Saturation, Value.
- `h` — `0–360`
- `s`, `v` — `0–1`
- `a` — `0–255`, defaults to `255`

```lua
local color = Color4.FromHSV(20, 1, 1)
```

---

### `Color4.FromHSL(h, s, l, a?)`
Creates a color from Hue, Saturation, Lightness.
- `h` — `0–360`
- `s`, `l` — `0–1`
- `a` — `0–255`, defaults to `255`

```lua
local color = Color4.FromHSL(20, 1, 0.5)
```

---

### `Color4.FromHWB(h, w, b, a?)`
Creates a color from Hue, Whiteness, Blackness.  
If `w + b > 1`, both values are normalized automatically.
- `h` — `0–360`
- `w`, `b` — `0–1`
- `a` — `0–255`, defaults to `255`

```lua
local color = Color4.FromHWB(20, 0, 0)
```

---

## Conversions

### `color:ToHSV()` → `h, s, v`
Returns Hue (`0–360`), Saturation (`0–1`), Value (`0–1`).

```lua
local h, s, v = color:ToHSV()
```

---

### `color:ToHSL()` → `h, s, l`
Returns Hue (`0–360`), Saturation (`0–1`), Lightness (`0–1`).

```lua
local h, s, l = color:ToHSL()
```

---

### `color:ToHWB()` → `h, w, b`
Returns Hue (`0–360`), Whiteness (`0–1`), Blackness (`0–1`).

```lua
local h, w, b = color:ToHWB()
```

---

### `color:ToHEX(withAlpha?)` → `string`
Returns a HEX string. Pass `true` to include the alpha channel.

```lua
color:ToHEX()       -- "#FF5000"
color:ToHEX(true)   -- "#FF5000FF"
```

---

## Color Manipulation

All manipulation methods return a **new** `Color4` instance and do not modify the original.  
`Lighten`, `Darken`, `Saturate` and `Desaturate` operate in HSL space for natural results.

### `color:Lighten(amount)` / `color:Darken(amount)`
Increases or decreases the Lightness component.
- `amount` — `0–1`

```lua
local lighter = color:Lighten(0.2)
local darker  = color:Darken(0.2)
```

---

### `color:Saturate(amount)` / `color:Desaturate(amount)`
Increases or decreases the Saturation component.
- `amount` — `0–1`

```lua
local vivid = color:Saturate(0.3)
local muted = color:Desaturate(0.3)
```

---

### `color:Grayscale()`
Removes all saturation using weighted luminance (`0.299 R + 0.587 G + 0.114 B`).

```lua
local gray = color:Grayscale()
```

---

### `color:Invert()`
Inverts all RGB channels (`255 - channel`). Alpha is preserved.

```lua
local inverted = color:Invert()
-- or via unary minus metamethod:
local inverted = -color
```

---

### `color:RotateHue(degrees)`
Rotates the Hue by the given number of degrees. Wraps around `0–360`.

```lua
local rotated = color:RotateHue(90)
```

---

## Harmony

All harmony methods return new `Color4` instances with the same Saturation, Value and Alpha as the source color, only the Hue is offset.

### `color:Complementary()` → `Color4`
Returns the color directly opposite on the color wheel (Hue + 180°).

```lua
local comp = color:Complementary()
```

---

### `color:Triadic()` → `Color4, Color4`
Returns two colors equally spaced around the wheel (Hue + 120° and Hue + 240°).

```lua
local a, b = color:Triadic()
```

---

### `color:Analogous(angle?)` → `Color4, Color4`
Returns two colors adjacent on the wheel. `angle` defaults to `30`.

```lua
local a, b = color:Analogous()
local a, b = color:Analogous(45)
```

---

### `color:SplitComplementary()` → `Color4, Color4`
Returns two colors flanking the complementary (Hue + 150° and Hue + 210°).

```lua
local a, b = color:SplitComplementary()
```

---

### `color:Tetradic()` → `Color4, Color4, Color4`
Returns three colors forming a rectangle on the wheel (Hue + 90°, 180°, 270°).

```lua
local a, b, c = color:Tetradic()
```

---

## Interpolation

### `Color4.Lerp(a, b, t)` / `a:Lerp(b, t)`
Linearly interpolates between two colors in RGB space.  
`t` is clamped to `0–1`. Returns a new `Color4`.

```lua
local red  = Color4.FromRGB(255, 0, 0)
local blue = Color4.FromRGB(0, 0, 255)

local mid = Color4.Lerp(red, blue, 0.5)  -- dot syntax
local mid = red:Lerp(blue, 0.5)          -- colon syntax
```

---

## Engine

### `color:Convert()`
Converts to a native `ColorRGBA` value for use in Retro Gadgets.

```lua
Screen.Color = color:Convert()
```

---

## Metamethods

### Arithmetic

All operators clamp results to `0–255` per channel.  
The second operand can be another `Color4` or a plain `number`.

| Expression | Description |
|---|---|
| `a + b` | Add channels |
| `a - b` | Subtract channels |
| `a * b` | Multiply (Color×Color normalized, Color×number scaled) |
| `2 * a` | Scalar multiplication (number on the left works too) |
| `a / b` | Divide channels |
| `a % n` | Modulo per channel |
| `a ^ n` | Power per channel |
| `-a` | Invert RGB (same as `:Invert()`) |

```lua
local brighter = color + 30
local blended  = colorA * colorB
local half     = color * 0.5
local inverted = -color
```

---

### Comparison

```lua
if colorA == colorB then
    -- true when all four channels match
end
```

---

### `tostring`

```lua
tostring(color)  -- "rgba(255, 80, 0, 255)"
```

---

## Type

The library exports a `Color4` type for use in typed Luau code:

```lua
local Color4 = require("Color4")
type Color4 = Color4.Color4

local function tint(color: Color4, amount: number): Color4
    return color:Lighten(amount)
end
```
