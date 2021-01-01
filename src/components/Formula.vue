<template>
  <div class="text-center">
    <q-icon
      name="fas fa-check-circle"
      color="green"
      v-if="testState == 'Correct'"
    />
    <q-icon
      name="fas fa-times-circle"
      color="red"
      v-else-if="testState == 'Miss'"
    />
    <q-icon name="fas fa-minus-circle" color="gray" v-else />
    <audio :src="`sound/${settings.speech}.mp3`" ref="audio"></audio>
  </div>
</template>

<script lang="ts">
declare const webkitSpeechRecognition: typeof SpeechRecognition;
type TestState = 'Untested' | 'Miss' | 'Correct';
import { defineComponent, PropType, ref, watch } from '@vue/composition-api';
import { IFormula } from './formula';
export default defineComponent({
  name: 'Formula',
  props: {
    settings: {
      type: Object as PropType<IFormula>,
      required: true
    }
  },
  setup(props, { emit }) {
    const audio = ref<HTMLAudioElement>();
    const testState = ref<TestState>('Untested');
    const recognition = new webkitSpeechRecognition();
    recognition.onresult = event => {
      for (let i = 0; i < event.results.length; i++) {
        const result = event.results.item(i);
        for (let j = 0; j < result.length; j++) {
          const alternative = result.item(j);
          emit('recognised', alternative.transcript, alternative.confidence);
          const value = parseInt(alternative.transcript, 10);
          if (value == props.settings.left * props.settings.right) {
            testState.value = 'Correct';
            emit('answered', props.settings, true);
            return;
          }
        }
      }
      testState.value = 'Miss';
      emit('answered', props.settings, false);
    };
    watch(props, () => {
      if (props.settings.playing) {
        if (audio.value) {
          audio.value.addEventListener(
            'ended',
            () => recognition.start(),
            false
          );
          audio.value
            .play()
            .then(() => emit('played', props.settings))
            .catch(err => console.error(err));
        }
      }
    });
    return { props, audio, testState };
  }
});
</script>
