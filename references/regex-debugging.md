# Regex Debugging Pattern Reference

When scanning HTML/CSS files for slop patterns, regex groups need careful handling:

## Common Issues

### Q5: Skipped Heading Level Detection
- **Problem**: `re.search(r"^h[1-6]\s", line)` raises `IndexError: no such group` because `^h[1-6]` has no capturing group
- **Fix**: Use `re.search(r"<h[1-6>", line)` to match the tag, then extract level from `tag[1]`
- **Example**: `m = re.search(r"<h[1-6>", line)` → `level = m.group(0)[1]` gives '1' for `<h1>`

### Pattern Group Capture
- **Problem**: `re.search(pattern, string)` with `()` capturing groups; access via `m.group(1)`, `m.group(2)`, etc.
- **Problem**: `^` anchor at start of line may not match if line has leading whitespace or HTML tags
- **Fix**: Use appropriate anchors (`^` for start, `\A` for string start) and check match before accessing groups

### Cross-platform Path Handling
- **Problem**: Scanning files across mixed POSIX/Windows path styles (`/mnt/c/project/` vs `C:\project\`)
- **Fix**: Use `Path.home()` for user directory, `Path()` for relative paths, avoid hardcoded drive letters
- **Verification**: After `cp -r`, always verify with `ls -laR` and test-scan the copied directory

## Fix Order for Regex Issues

1. **Immediate**: Replace failing regex with working alternative (don't block the scan)
2. **Root cause**: Understand why the original pattern failed (capture groups, anchors, escape sequences)
3. **Document**: Add the pattern and fix to this reference for future sessions
4. **Test**: Verify the fix works on sample files before considering it resolved

## Verified Patterns (safe to use)

- `r"<h[1-6]"}` — matches any heading tag, extract level from char at index 1
- `r"font-size\s*:\s*([\d.]+)px"` — safe font-size extraction with group
- `r"letter-spacing\s*:\s*([\d.]+)em"` — safe letter-spacing extraction with group
- `r"background-clip.*text.*gradient|gradient.*text.*background-clip"` — gradient text detection