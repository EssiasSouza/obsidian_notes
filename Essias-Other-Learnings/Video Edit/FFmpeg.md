# How to Convert OBS Audio with FFmpeg

If a video recorded with OBS plays correctly in VLC but has distorted or noisy audio in VEGAS, you can use FFmpeg to convert the audio to AAC without re-encoding the video.

## 1. Open a Terminal

Place `video 1.mp4` in the same folder where you will run FFmpeg.

## 2. Run the FFmpeg command

```bash
ffmpeg -i "video 1.mp4" -c:v copy -c:a aac -b:a 320k "video 1_fixed.mp4"
```

## 3. What the command does

* `-i "video 1.mp4"` — selects the original video.
* `-c:v copy` — copies the video stream without re-encoding it, so there is no video quality loss.
* `-c:a aac` — converts the audio to AAC.
* `-b:a 320k` — uses a high-quality 320 kbps audio bitrate.
* `"video 1_fixed.mp4"` — creates the converted file.

## 4. Import the new file into VEGAS

After FFmpeg finishes, open **`video 1_fixed.mp4`** in VEGAS and check the audio.

The video quality should be identical to the original because the video stream was copied rather than re-encoded.
