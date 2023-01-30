It really doesn't matter what this docker is going to do underneath, but the objectives of using it. These are:
- [[Make calculations]]
- [[Save files]]


These objectives are not trivial so we will get a series of possible [[Complications]].

*Note: This file size is mearly for test. When running on production this will process TB.* 

# Dockerfile structure

[Dockerfile](https://github.com/HEP-EPN/ECHEPdocker/blob/main/Dockerfile) is the file responsable of building the image. You can see the video explanation [here](https://cern.zoom.us/rec/play/G42ogoPrJz33Lf3ptw2eYuo6XHlJ2oZKDn7F67MadYsKdQbau97kg5-BQBHkEmWfHqbcKObDLlZw4rtN._luwwFggN1NYxTXK?canPlayFromShare=true&from=share_recording_detail&continueMode=true&componentName=rec-play&originRequestUrl=https%3A%2F%2Fcern.zoom.us%2Frec%2Fshare%2FmXXo-DwURRtjF2q8epmpA-yD8WJL4zfweLGh5xiQXPit7u3blcVw62_zKox5JtxF.Jcy1F2_QA9hCzr6u) at min 7:25. Remember that the [[Docker Files]] that are preinstalled in the image are heavy [2GB].

We will start with the base image for our Dockerfile which is root-docker.

## Root Docker 

![[Root Docker]]

## HEPdocker

![[HEPdocker]]

