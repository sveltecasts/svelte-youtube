
<script>
  let player;
  import { createEventDispatcher , onMount} from "svelte";
  const dispatch = createEventDispatcher();
  let divId = "player_"+parseInt(Math.random() * 100000).toString()
  export let videoId;
  export let height = '390';
  export let width = '640';
  onMount(() => {
      let ytTagUrl = "https://www.youtube.com/iframe_api";
      if(!isMyScriptLoaded(ytTagUrl)){
    // 2. This code loads the IFrame Player API code asynchronously.
        var tag = document.createElement("script");
        tag.src = ytTagUrl;
        var firstScriptTag = document.getElementsByTagName("script")[0];
        firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);
       }
      
      window.onYouTubeIframeAPIReady = function() {
        //console.log('hello')
        window.dispatchEvent(new Event("YouTubeIframeAPIReady"))
      }

      window.addEventListener("YouTubeIframeAPIReady", function(){
          console.log('load player..')
          player =  new YT.Player(divId, {
              height,
              width,
              videoId: videoId,
              events: {
                //'onReady': onPlayerReady,
                'onStateChange': onPlayerStateChange
              }
            });
      })

  })

function isMyScriptLoaded(url ="") {
    var scripts = document.getElementsByTagName('script');
    for (var i = scripts.length; i--;) {
        if (scripts[i].src == url) return true;
    }
    return false;
}

  function onPlayerStateChange({data}){
    dispatch("StateChange",data)
  }
  export function playVideo(){
    player.playVideo()
  }
</script>
<div id={divId}></div>
