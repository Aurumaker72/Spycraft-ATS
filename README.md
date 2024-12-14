# Spycraft-ATS
Some work on documenting the spycraft ATS format

# Context

pixFormat is set based on the `dwGBitMask` field of the `DDPIXELFORMAT` structure returned by `IDirectDrawSurface::GetPixelFormat`.  It seems to be set once at initialization time (see DIRSCRN.C:124).

`pixFormat == 1`: RGB565

`pixFormat == 0`: RGB555


# Format

| Length (bytes) or ctype  | Description | Notes
| --------------  | ----------- | ------ |
| 4 | Signature | Must be `41 54 53 20`.
| 1 | Version | `2` in 95319.ATS.
| 1 | Color Depth | Can be `8`, `16`, or `24`. <br> `16` in 95319.ATS.
| 4 | Extra Data (version 1) | Unused.
| 4 | Extra Data (version 2) | Palette entry count.
| If video pixel format RGB565 and 2-bit color depth specified  | 
| ? | ? | This path shouldn't be hit. Undocumented for now.
| Else | 
| Palette entry count * 4 | Palette data | One entry is 4 bytes big
| 2 | ATS Reel Count | A new empty ATS structure is created with this data. If the version is `2`, the ATS structure gets its palette from the ATS file.
| For each reel, as specified by the reel count variable... |
| 2 | Reel Frame Count | A new reel is created with this data. It receives a pointer to the previously created ATS structure, along with info about its index in the ATS reel buffer and this variable.
| For each frame in the reel, as specified by the frame count variable... |
| 2 | Width |
| 2 | Height |
| 2 | Viewport X Origin
| 2 | Viewport Y Origin
| 2 | Skip color | TODO: Document. This is heavily used in the sprite drawing code and seems to be some kind of mask.
| `rowBytes` is declared as the frame width, or double the width if 16-bit color depth is present. <br> Note that the variable checked against, `bpp` has been overwritten to be `8` by now if version is `2`.
| `rowBytes` * Height | Video buffer
