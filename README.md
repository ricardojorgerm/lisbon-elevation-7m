# lisbon-elevation-7m

## Description

A dataset for the terrain elevation of the Metropolitan Area of Lisbon, Portugal (AML)
No surface features (i.e. buildings and vegetation removed)

Resolution: 7m
Coordinate system for the files: 
- EHdr/BIL WGS-84
- .tif EPSG:3763

## Data source

MDT-2m
Modelo Digital de Terreno, 2 metros
© Direção-Geral do Território, 2024, sob licença CC-BY-4.0

### Instrumentation:

LiDAR 
- Riegl VQ-780II-S
- PhaseOne iXM-RS 150F
- PhaseOne iXM-RS 100
Resolution: 10 points/m2, planimetric resolution 50cm +/- 30cm error, elevation error: +/- 10cm

### Source dataset:

Resolution: 2m planimetric, only Terrain tagged points
Coordinate system: EPSG:3763
Orthometric on the Portuguese (Cascais) datum

### Transformations:

This repository's dataset resolution: downsampled to 7m by median interpolation
- EHdr/BIL: reprojected to WGS 84, nearest-neighbour copy
No adjustment between the original Portuguese quasi-geoid grid and the EGM96 grid (estimated at <1m error)

```
gdalwarp -overwrite \
  -r med -tr "7" "7" -tap \
  -ot Int16 -dstnodata "$NODATA" \
  -co TILED=YES -co COMPRESS=DEFLATE \
  "$WORK_DIR/mdt_2m.vrt" "$WORK_DIR/mdt_7m_3763.tif"

gdalwarp -overwrite \
  -s_srs EPSG:3763 -t_srs EPSG:4326 \
  -r near -ot Int16 -dstnodata "$NODATA" \
  -of EHdr \
  "$WORK_DIR/mdt_7m_3763.tif" "$OUT_DIR/$OUT_NAME.bil"
```
