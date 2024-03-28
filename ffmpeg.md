ffmpeg
Add a static image to an audio clip to upload to youtube
.\ffmpeg -r 1 -loop 1 -i "D:\Download\transcribe.jpeg" -i "D:\Download\WhatsApp Audio 2024-03-25 at 9.44.21 AM.mp4" -acodec copy -r 1 -shortest -vf scale=1280:720 ep1.flv
