---
layout: post
title: 'Download youtube videos via powershell'
published: true
tags: "powershell youtube ffmpeg"
---

Each week I record a podcast with [@JohnBubriski](https://twitter.com/johnbubriski) where we talk about the latest goings on with our vidya game startup: [Frag Castle Games](http://fragcastle.com). We record each episode via Google Hangouts so they get published to youtube. This takes away a lot of the headaches that recording a podcast via Skype can have, but it adds a smaller one: getting our audio as an MP3 so we can ship it off to everyone who listens via iTunes/RSS.

I had been downloading the mp4's of each broadcast via the [youtube video manager page](https://www.youtube.com/my_videos?o=U) and then converting them to MP3's via ffmpeg. This is pretty quick, but still a manual process. I wanted something that I could kick off and run in the background.

So I hacked up a really simple powershell script that uses [youtube-dl](http://rg3.github.io/youtube-dl) and [ffmpeg](https://www.ffmpeg.org/download.html) to do all of this.

Installing both of the requirements for this script is really simple if you have [Chocolatey](http://chocolatey.org) installed.

{% codeblock lang:bash %}
$ cinst youtube-dl ffmpeg
{% endcodeblock %}

Now, you can copy/pasta this script and start downloading and converting your videos to audio!

{% codeblock lang:powershell %}
Param (
  [Parameter(Mandatory=$True)]
  [string]$url
)

# this assumes you have youtube-dl installed
# win: choco install youtube-dl
# others: http://rg3.github.io/youtube-dl/download.html

# this also assumes you have ffmpeg installed
# win: choco install ffmpeg
# others: https://www.ffmpeg.org/download.html

# store the local file name for conversion and cleanup
$local = youtube-dl --get-filename -o "%(title)s.%(ext)s" $url
$local_name_only = [System.IO.Path]::GetFileNameWithoutExtension($local)

# download the file from youtube
youtube-dl -o "%(title)s.%(ext)s" $url

# convert to mp3
ffmpeg -i "./$local" -vn -b:a 192k -c:a libmp3lame "$local_name_only.mp3"

# cleanup
if($LastExitCode -eq 0) {
  rm "./$local"
} else {
  Write-Host "something went fubar. not cleaning up."
}

{% endcodeblock %}
