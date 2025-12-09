# Entrance Development Workflow

This fork is maintained for the enlightenment-live overlay.

## Repository Setup

**Your Fork:** https://github.com/OxusByte45/entrance  
**Upstream:** https://github.com/Obsidian-StudiosInc/entrance  
**Local Clone:** `~/dev/enlightenment/entrance/`

## Branch Structure

- `master` - Stable, matches what's in enlightenment-live overlay
- `development` - Active development branch
- Feature branches - `feature/your-feature-name`

## Development Workflow

### 1. Making Changes

```bash
cd ~/dev/enlightenment/entrance

# Create a feature branch
git checkout development
git checkout -b feature/your-improvement

# Make your changes
# Edit files...

# Test build
meson setup build --prefix /usr --sbindir /usr/sbin
ninja -C build

# Commit
git add -A
git commit -m "Describe your changes"
```

### 2. Testing with Portage

```bash
# Option A: Test the live ebuild
sudo emerge -av =x11-misc/entrance-9999

# Option B: Create a local build
cd build
sudo ninja install

# Test entrance
sudo /usr/sbin/entrance --nodaemon  # Test run
```

### 3. Pushing Changes

```bash
# Push feature branch
git push origin feature/your-improvement

# Create PR on GitHub to merge into development
# After testing, merge to master

# Update master
git checkout master
git merge development
git push origin master
```

### 4. Updating from Upstream (if needed)

```bash
# Fetch upstream changes
git fetch upstream

# Merge upstream into your development
git checkout development
git merge upstream/master

# Resolve conflicts if any
git push origin development
```

## Important Paths

When building/installing entrance:

- **Binary:** `/usr/sbin/entrance` (daemon)
- **Client:** `/usr/share/bin/entrance_client` (GUI)
- **Config:** `/etc/entrance/entrance.conf`
- **Themes:** `/usr/share/entrance/themes/`
- **Sessions:** Scans `/usr/share/xsessions/` and `/usr/share/wayland-sessions/`

## Meson Build Options

```bash
meson setup build \
    --prefix /usr \
    --bindir /usr/share/bin \
    --sbindir /usr/sbin \
    --datadir /usr/share \
    --sysconfdir /etc \
    -Dlogind=true \
    -Dpam=true \
    -Dnls=true \
    -Ddebug=false
```

## Testing Checklist

Before pushing to master:

- [ ] Builds successfully with meson
- [ ] Installs to correct paths
- [ ] Works with display-manager service
- [ ] X11 sessions work
- [ ] Wayland sessions work
- [ ] elogind integration working
- [ ] PAM authentication working
- [ ] Update enlightenment-live ebuild if needed

## Updating the Overlay

After pushing changes to master:

```bash
cd /var/db/repos/enlightenment-live

# If paths or dependencies changed, update ebuild
nano x11-misc/entrance/entrance-9999.ebuild

# Regenerate manifest
sudo ebuild x11-misc/entrance/entrance-9999.ebuild digest

# Commit and push overlay changes
git add -A
git commit -m "entrance: update for xyz changes"
git push origin master
```

## Known Improvements Needed

1. **Wayland Compositor Integration**
   - Currently relies on session to provide compositor
   - Could add native Wayland display server support

2. **elogind Session Management**
   - Basic support works
   - Could improve session tracking and seat management

3. **Settings UI**
   - Currently broken/hidden
   - Needs complete rewrite

4. **Themes**
   - Could add more modern EFL themes
   - Better high-DPI support

5. **Documentation**
   - Add man pages
   - Improve configuration examples

## Resources

- EFL Documentation: https://www.enlightenment.org/docs
- Meson Build System: https://mesonbuild.com/
- Display Manager Standards: https://www.freedesktop.org/wiki/Software/display-managers/

## Contact

Report issues or contribute at:
- Fork: https://github.com/OxusByte45/entrance
- Overlay: https://github.com/OxusByte45/enlightenment-live
