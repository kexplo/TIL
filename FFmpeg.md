# FFmpeg

FFmpeg snippets

## Concatenate

```txt
# filelist.txt
file '/path/to/file1.ts'
file '/path/to/file2.ts'
file '/path/to/file3.ts'
```

```bash
$ ffmpeg -f concat -i list.txt -c copy merged.ts
```

Or

```bash
$ ffmpeg -i "concat:file1.ts|file2.ts|file3.ts" -c copy merged.ts
```

reference: https://trac.ffmpeg.org/wiki/Concatenate

## TS to MP4

```bash
$ ffmpeg -i input.ts -acodec copy -vcodec copy output.mp4
```