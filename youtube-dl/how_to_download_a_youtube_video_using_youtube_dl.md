# How to download a youtube video using youtube-dl

Copy the URL for the video of interest and pass it to youtube-dl as shown below to download the video to your working directory

```bash
$ youtube-dl https://www.youtube.com/watch?v=BaW_jenozKc
```

If you want the output file to have the proper name and extension of the video file, use the command below (with your video's url swapped in).

```bash
$ youtube-dl -o '%(title)s.%(ext)s' https://www.youtube.com/watch?v=BaW_jenozKc --restrict-filenames
```

## Troubleshooting

If you can't play the video after downloading it, try using `ffmpeg` to convert it to an `.mp4` via 

```bash
$ ffmpeg -i input_file_that_isnt_working.mkv output_file.mp4
```