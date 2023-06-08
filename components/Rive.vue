  
  <template>
    <div class="inline">
      <canvas ref="canvas" width="500" height="500"></canvas>
    </div>
    <div class="inline">
      <button id="idle">Start Truck</button>
      <button id="wipers">Start Wipers</button>
      <div id="loop">Looped Animation: TBD</div>
    </div>
  </template>
  
  <script>
  import { Rive } from '@rive-app/canvas'
  
  export default {
    name: 'App',
    components: {
      Rive
    },
    props: {
      src: String,
    },
    mounted () {
      let idleButton = document.getElementById('idle')
      let wipersButton = document.getElementById('wipers')
      let loopDiv = document.getElementById('loop')
      let truck = new Rive({
        src: this.$props.src,
        artboard: 'Jeep',
        canvas: this.$refs.canvas,
        onPlay: event => {
          const names = event.data;
          names.forEach(name => {
            if (name === 'idle') {
              idleButton.textContent = 'Stop Truck';
            } else if (name === 'windshield_wipers') {
              wipersButton.textContent = 'Stop Wipers';
            }
          });
        },
        onStop: event => {
          const names = event.data;
          names.forEach(name => {
            if (name === 'idle') {
              idleButton.textContent = 'Start Truck';
            } else if (name === 'windshield_wipers') {
              wipersButton.textContent = 'Start Wipers';
            }
          });
        },
        onLoop: event => {
          loopDiv.textContent = `Looped Animation: ${event.data.animation}`;
        }
      });
  
      idleButton.addEventListener('click', () => {
        if (truck.playingAnimationNames.includes('idle')) {
          truck.pause('idle');
        } else {
          truck.play('idle');
        }
      });
  
      wipersButton.addEventListener('click', () => {
        truck.play()
        if (truck.playingAnimationNames.includes('windshield_wipers')) {
          truck.stop('windshield_wipers');
        } else {
          truck.play('windshield_wipers');
        }
      });
    }
  }
  </script>