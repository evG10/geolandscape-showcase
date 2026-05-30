# Publishing Checklist

Before publishing this showcase as a public GitHub repository:

- [x] Add first real screenshots.
- [ ] Add close-up screenshots for roads/intersections and river/water handling.
- [ ] Add a short demo video or GIF.
- [ ] Check that no API keys or tokens are visible.
- [ ] Check that no private source code is copied into the showcase.
- [ ] Check that no Unreal `.uasset`, `Binaries`, `Intermediate`, `Saved` or cache folders are committed.
- [ ] Update the repository URL in your resume.

## Suggested Repository Name

```text
geolandscape-showcase
```

## Suggested GitHub Description

```text
Showcase for a private Unreal Engine 5 plugin that generates landscapes, cities, roads, buildings and water from real geodata.
```

## Suggested Topics

```text
unreal-engine unreal-engine-5 cpp geodata openstreetmap landscape-generation editor-plugin procedural-generation
```

## Commands

Run these commands inside `geolandscape_showcase` after creating a public GitHub repository:

```powershell
git init
git add .
git commit -m "Initial GeoLandscape showcase"
git branch -M main
git remote add origin https://github.com/evG10/geolandscape-showcase.git
git push -u origin main
```
