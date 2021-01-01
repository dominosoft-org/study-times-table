<template>
  <q-page class="row items-center justify-evenly">
    <div class="text-center">
      <q-markup-table dense>
        <thead>
          <tr>
            <th>＼</th>
            <th>1</th>
            <th>2</th>
            <th>3</th>
            <th>4</th>
            <th>5</th>
            <th>6</th>
            <th>7</th>
            <th>8</th>
            <th>9</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(step, stepIndex) in formulaList"
            :key="'Step' + stepIndex"
          >
            <th>{{ stepIndex + 1 }}</th>
            <td v-for="item in step" :key="item.left + '×' + item.right">
              <formula
                :settings="item"
                @played="onPlayed"
                @recognised="onRecognised"
                @answered="onAnswered"
              />
            </td>
          </tr>
        </tbody>
      </q-markup-table>
      <q-btn class="q-my-lg" @click="onStart">Start</q-btn>
      <div v-html="speeched.join('<br/>')" />
    </div>
    <audio src="../assets/correct.mp3" ref="soundCorrect" />
    <audio src="../assets/miss.mp3" ref="soundMiss" />
  </q-page>
</template>

<script lang="ts">
import Formula from 'components/Formula.vue';
import { IFormula } from 'components/formula';
import { defineComponent, reactive, ref } from '@vue/composition-api';
export default defineComponent({
  name: 'PageIndex',
  components: { Formula },
  setup() {
    const formulaList = reactive([
      [
        { left: 1, right: 1, playing: false, finished: false, speech: '1-1' },
        { left: 1, right: 2, playing: false, finished: false, speech: '1-2' },
        { left: 1, right: 3, playing: false, finished: false, speech: '1-3' },
        { left: 1, right: 4, playing: false, finished: false, speech: '1-4' },
        { left: 1, right: 5, playing: false, finished: false, speech: '1-5' },
        { left: 1, right: 6, playing: false, finished: false, speech: '1-6' },
        { left: 1, right: 7, playing: false, finished: false, speech: '1-7' },
        { left: 1, right: 8, playing: false, finished: false, speech: '1-8' },
        { left: 1, right: 9, playing: false, finished: false, speech: '1-9' }
      ],
      [
        { left: 2, right: 1, playing: false, finished: false, speech: '2-1' },
        { left: 2, right: 2, playing: false, finished: false, speech: '2-2' },
        { left: 2, right: 3, playing: false, finished: false, speech: '2-3' },
        { left: 2, right: 4, playing: false, finished: false, speech: '2-4' },
        { left: 2, right: 5, playing: false, finished: false, speech: '2-5' },
        { left: 2, right: 6, playing: false, finished: false, speech: '2-6' },
        { left: 2, right: 7, playing: false, finished: false, speech: '2-7' },
        { left: 2, right: 8, playing: false, finished: false, speech: '2-8' },
        { left: 2, right: 9, playing: false, finished: false, speech: '2-9' }
      ],
      [
        { left: 3, right: 1, playing: false, finished: false, speech: '3-1' },
        { left: 3, right: 2, playing: false, finished: false, speech: '3-2' },
        { left: 3, right: 3, playing: false, finished: false, speech: '3-3' },
        { left: 3, right: 4, playing: false, finished: false, speech: '3-4' },
        { left: 3, right: 5, playing: false, finished: false, speech: '3-5' },
        { left: 3, right: 6, playing: false, finished: false, speech: '3-6' },
        { left: 3, right: 7, playing: false, finished: false, speech: '3-7' },
        { left: 3, right: 8, playing: false, finished: false, speech: '3-8' },
        { left: 3, right: 9, playing: false, finished: false, speech: '3-9' }
      ],
      [
        { left: 4, right: 1, playing: false, finished: false, speech: '4-1' },
        { left: 4, right: 2, playing: false, finished: false, speech: '4-2' },
        { left: 4, right: 3, playing: false, finished: false, speech: '4-3' },
        { left: 4, right: 4, playing: false, finished: false, speech: '4-4' },
        { left: 4, right: 5, playing: false, finished: false, speech: '4-5' },
        { left: 4, right: 6, playing: false, finished: false, speech: '4-6' },
        { left: 4, right: 7, playing: false, finished: false, speech: '4-7' },
        { left: 4, right: 8, playing: false, finished: false, speech: '4-8' },
        { left: 4, right: 9, playing: false, finished: false, speech: '4-9' }
      ],
      [
        { left: 5, right: 1, playing: false, finished: false, speech: '5-1' },
        { left: 5, right: 2, playing: false, finished: false, speech: '5-2' },
        { left: 5, right: 3, playing: false, finished: false, speech: '5-3' },
        { left: 5, right: 4, playing: false, finished: false, speech: '5-4' },
        { left: 5, right: 5, playing: false, finished: false, speech: '5-5' },
        { left: 5, right: 6, playing: false, finished: false, speech: '5-6' },
        { left: 5, right: 7, playing: false, finished: false, speech: '5-7' },
        { left: 5, right: 8, playing: false, finished: false, speech: '5-8' },
        { left: 5, right: 9, playing: false, finished: false, speech: '5-9' }
      ],
      [
        { left: 6, right: 1, playing: false, finished: false, speech: '6-1' },
        { left: 6, right: 2, playing: false, finished: false, speech: '6-2' },
        { left: 6, right: 3, playing: false, finished: false, speech: '6-3' },
        { left: 6, right: 4, playing: false, finished: false, speech: '6-4' },
        { left: 6, right: 5, playing: false, finished: false, speech: '6-5' },
        { left: 6, right: 6, playing: false, finished: false, speech: '6-6' },
        { left: 6, right: 7, playing: false, finished: false, speech: '6-7' },
        { left: 6, right: 8, playing: false, finished: false, speech: '6-8' },
        { left: 6, right: 9, playing: false, finished: false, speech: '6-9' }
      ],
      [
        { left: 7, right: 1, playing: false, finished: false, speech: '7-1' },
        { left: 7, right: 2, playing: false, finished: false, speech: '7-2' },
        { left: 7, right: 3, playing: false, finished: false, speech: '7-3' },
        { left: 7, right: 4, playing: false, finished: false, speech: '7-4' },
        { left: 7, right: 5, playing: false, finished: false, speech: '7-5' },
        { left: 7, right: 6, playing: false, finished: false, speech: '7-6' },
        { left: 7, right: 7, playing: false, finished: false, speech: '7-7' },
        { left: 7, right: 8, playing: false, finished: false, speech: '7-8' },
        { left: 7, right: 9, playing: false, finished: false, speech: '7-9' }
      ],
      [
        { left: 8, right: 1, playing: false, finished: false, speech: '8-1' },
        { left: 8, right: 2, playing: false, finished: false, speech: '8-2' },
        { left: 8, right: 3, playing: false, finished: false, speech: '8-3' },
        { left: 8, right: 4, playing: false, finished: false, speech: '8-4' },
        { left: 8, right: 5, playing: false, finished: false, speech: '8-5' },
        { left: 8, right: 6, playing: false, finished: false, speech: '8-6' },
        { left: 8, right: 7, playing: false, finished: false, speech: '8-7' },
        { left: 8, right: 8, playing: false, finished: false, speech: '8-8' },
        { left: 8, right: 9, playing: false, finished: false, speech: '8-9' }
      ],
      [
        { left: 9, right: 1, playing: false, finished: false, speech: '9-1' },
        { left: 9, right: 2, playing: false, finished: false, speech: '9-2' },
        { left: 9, right: 3, playing: false, finished: false, speech: '9-3' },
        { left: 9, right: 4, playing: false, finished: false, speech: '9-4' },
        { left: 9, right: 5, playing: false, finished: false, speech: '9-5' },
        { left: 9, right: 6, playing: false, finished: false, speech: '9-6' },
        { left: 9, right: 7, playing: false, finished: false, speech: '9-7' },
        { left: 9, right: 8, playing: false, finished: false, speech: '9-8' },
        { left: 9, right: 9, playing: false, finished: false, speech: '9-9' }
      ]
    ] as IFormula[][]);
    const soundCorrect = ref<HTMLAudioElement>();
    const soundMiss = ref<HTMLAudioElement>();
    const speeched = ref([] as string[]);
    const getRandomInt = (max: number): number => {
      return Math.floor(Math.random() * Math.floor(max));
    };
    const startNext = () => {
      const list = formulaList
        .reduce((pre, current) => {
          pre.push(...current);
          return pre;
        }, [])
        .filter(e => !e.finished);
      const next = getRandomInt(list.length);
      list[next].playing = true;
    };
    const onStart = () => {
      startNext();
    };
    const onPlayed = (settings: IFormula) => {
      settings.playing = false;
      while (speeched.value.length > 4) {
        speeched.value = speeched.value.slice(speeched.value.length - 4);
      }
    };
    const onRecognised = (transcript: string, confidence: number) => {
      speeched.value.push(`${transcript} (${confidence}) `);
    };
    const onAnswered = (settings: IFormula, correct: boolean) => {
      speeched.value[speeched.value.length - 1] += correct
        ? '正解！'
        : 'ブッブー！';
      const sound = correct ? soundCorrect.value : soundMiss.value;
      settings.finished = true;
      sound?.play().then(() => setTimeout(() => startNext(), 1500));
    };
    return {
      formulaList,
      soundCorrect,
      soundMiss,
      speeched,
      onStart,
      onPlayed,
      onRecognised,
      onAnswered
    };
  }
});
</script>
