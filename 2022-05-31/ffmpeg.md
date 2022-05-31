# FFmpeg Commands

Used to create https://www.youtube.com/watch?v=njWYDQdS6d0&ab_channel=Robpol86

## Timestamp, Still Images, and Split

```bash
ffmpeg -i vac.mp4 \
    -vf "drawtext=text='%{pts\:hms}':x=(w-text_w)/2:y=h-th-70:fontfile=/usr/share/fonts/truetype/dejavu/DejaVuSansMono-Bold.ttf:fontsize=40:fontcolor=white" \
    -map 0:v:0 -map 0:a:0 -g 10 \
    -c:v libx264 -qp 0 \
    -c:a flac \
    -to 1:46.9 vac_ts.mkv
```

```bash
ffmpeg -loop 1 -framerate 29.97 -t 1.5 -i vac1.jpg -t 1.5 -f lavfi -i aevalsrc=0 \
    -loop 1 -framerate 29.97 -t 1.5 -i vac2.jpg -t 1.5 -f lavfi -i aevalsrc=0 \
    -i vac_ts.mkv -filter_complex '[0:v:0][1:a:0] [2:v:0][3:a:0] [4:v:0][4:a:0] concat=n=3:v=1:a=1' \
    -g 10 -c:v libx264 -qp 0 -c:a flac \
    vac_img.mkv
```

```bash
ffmpeg -i vac_img.mkv -c copy -f segment -reset_timestamps 1 \
    -segment_times 8.5,17.5,26.4,96 \
    vac-split-%d.mkv
```

## Speed Up

Rename 1 and 3 by adding `-og`.

```bash
speed_up () (
    i="$1"
    factor="$2"
    txt="x=50:y=50:fontfile=/usr/share/fonts/truetype/dejavu/DejaVuSansMono-Bold.ttf:fontsize=140:fontcolor=white"
    set -x
    ffmpeg -y -i "vac-split-$i-og.mkv" \
        -filter_complex "[0:v]setpts=PTS/${factor}[v]; [0:a]atempo=${factor}[a]; [v]drawtext=text='>>':${txt}[v2]" \
        -map '[v2]' -map '[a]' -g 10 -c:v libx264 -qp 0 -level:v 62 -c:a flac "vac-split-$i.mkv"
)
speed_up 1 5 && speed_up 3 12
```

## Concat

```bash
for f in vac-split-?.mkv; do
    ffprobe -show_format -show_streams -v error "$f" > "ffprobe_$f.txt"
done
vimdiff ffprobe_vac-split-{0,1,2,3,4}.mkv.txt
```

```bash
ffmpeg -i vac-split-0.mkv -i vac-split-1.mkv -i vac-split-2.mkv -i vac-split-3.mkv -i vac-split-4.mkv \
    -filter_complex "[0:v:0][0:a:0] [1:v:0][1:a:0] [2:v:0][2:a:0] [3:v:0][3:a:0] [4:v:0][4:a:0] concat=n=5:v=1:a=1[v][a]" \
    -map '[v]' -map '[a]' -c:v libx264 -qp 0 -c:a flac vac_cat.mkv
```

## Discord

```bash
ffmpeg -i vac_cat.mkv \
    -metadata title="OEMTOOLS Extractor Vacuum Gauge Mod" \
    -map 0:a:0 -map 0:v:0 \
    -c:v libx264 -crf 25 -preset veryslow -profile:v high -level:v 31 -pix_fmt yuv420p \
    -vf "colorspace=bt709:iall=bt601-6-625:fast=1" -color_range 1 -colorspace 1 -color_primaries 1 -color_trc 1 \
    -c:a aac -b:a 128k \
    oemtools_gauge_mod.mp4
```
