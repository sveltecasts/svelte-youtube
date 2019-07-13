<script context="module">
  // 2. This code loads the IFrame Player API code asynchronously.
  var tag = document.createElement("script");

  tag.src = "https://www.youtube.com/iframe_api";
  var firstScriptTag = document.getElementsByTagName("script")[0];
  firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);

  window.onYouTubeIframeAPIReady = function() {
    //console.log('hello')
    window.dispatchEvent(new Event("YouTubeIframeAPIReady"))
  }
</script>

<script>
  let player;
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();
  let divId = "player_"+parseInt(Math.random() * 100000).toString()
  export let videoId;
  window.addEventListener("YouTubeIframeAPIReady", function(){
      console.log('load player..')
      player =  new YT.Player(divId, {
          height: '390',
          width: '640',
          videoId: videoId,
          events: {
            //'onReady': onPlayerReady,
            'onStateChange': onPlayerStateChange
          }
        });
  })
  function onPlayerStateChange({data}){
    dispatch("StateChange",data)
  }
  export function playVideo(){
    player.playVideo()
  }
</script>
YT-Component VID: {videoId}<br>
<div id={divId}></div><br>
