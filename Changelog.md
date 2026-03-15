# Changelog

## [1.1]

### Added
- `Color4:Blend(blend, mode?, t?)` — blends two colors using a Photoshop-style blend mode. Supports 10 modes: `Normal`, `Multiply`, `Screen`, `Overlay`, `SoftLight`, `HardLight`, `Difference`, `Exclusion`, `Darken`, `Lighten`
- `Color4:Clone()` — returns a new `Color4` instance with identical channel values
- `Color4.FlatColor` — a read-only preset palette with all base colors. Every access automatically returns a fresh clone of the preset, so modifying the result never affects the original

---

## [1.0] — Initial Release

### Release of the library
