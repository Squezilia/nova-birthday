<template>
    <Loader v-model="isPageLoaded" />
    <main class="page" :style="`--bg: ${background}; --c: ${color}; --fW: ${fontWeight}; --fF: ${fontFamily};`">
        <JumpText :loaded="false" :delay="frameData.delay" :text="frameData.text" :id="`frame-${frame}`" v-for="(frameData, frame) in animation" />
    </main>
</template>
  
<script lang="ts">
import { defineComponent } from 'vue';

import { get } from 'lodash';

import Loader from '@/components/view/Loader.vue';
import JumpText from '@/components/view/JumpText.vue';

interface IFrameData {
    text: string;
    delay: number;
    transform: string;
    duration: number;
}

export default defineComponent({
    data() {
        return {
            isPageLoaded: false,
            background: "#121212",
            color: "#fff",
            fontWeight: 400,
            fontFamily: "'Poppins', sans-serif",
            animation: [
                /* 0 */ { text: "1 Kasım 2022", delay: 1, transform: "down", duration: 6000 },
                /* 1 */ { text: "Mehmet Emir Bozat", delay: .5, transform: "up", duration: 3000 },

                /* 2 */ { text: "Nova Software", delay: .5, transform: "up", duration: 2000 },
                /* 3 */ { text: "Sadece hobi olarak girdiğim bu site", delay: .2, transform: "left", duration: 2500 },
                /* 4 */ { text: "bize yeni bir kapı açtı.", delay: .4, transform: "right", duration: 2500 },

                /* 5 */ { text: "Wersup Client", delay: .5, transform: "down", duration: 2000 },
                /* 6 */ { text: "Bir gecede çöp olan bu client için", delay: .2, transform: "left", duration: 2500 },
                /* 7 */ { text: "bütün Wersdown saygı duruşundaydık.", delay: .2, transform: "down", duration: 2500 },

                /* 8 */ { text: "Wersdown Client", delay: .5, transform: "up", duration: 2000 },
                /* 9 */ { text: "Wersup'ın varisi olan bu client", delay: .2, transform: "right", duration: 2500 },
                /* 10 */ { text: "gün yüzü göremeden gitti.", delay: .2, transform: "left", duration: 2500 },

                /* 11 */ { text: "Wersdown Software", delay: .5, transform: "right", duration: 2000 },
                /* 12 */ { text: "Halen geliştirmede olan bu platform", delay: .2, transform: "up", duration: 2500 },
                /* 13 */ { text: "Team Wersdown'ın en mükemmel ürünü olacak!", delay: .2, transform: "down", duration: 10000 },

                /* 14 */ { text: "D̷͍̝̙͚̼̂̔̌͊̿̐V̴̡̼̤͖̎͝s̷͔̲͈̿b̴̰̎̔̓̚w̴͙̼̣̐̀̊̔̾̑7̴̢͉̖͙̌̊̈͜t̴͎̜̝͙͘ͅE̵̛̞̋̓̎̓͠D̸̜́̋̊̈̎͘Ṋ̵͛͘͜p̶̯͎̪̠͕̩͋͌̀͜ḽ̴̭͔̮̼̺̝̎͋͂͛ä̷̜̜̳̣̜̭̦́͐͗̓R̸̪̃̓́̏̈͝2̷̨͇̬̳͖̮͒4̴̼̊̈͆͒́͝G̶̡̢̛̭̞̝̃͗̋̚͘r̸͇̂̕̕g̵̢̻̟̹̝͒̂͋͋̎A̵̞̬͊6̶̬̻̳͚͇̺̿ͅg̷̺̣̱̋̃͂̓͒͝=̵̡͖͇͈̦͇͝=̷͉̞̙̠̯̌̓͋͊͂̕͜", delay: .5, transform: "left", duration: 2000 },
                /* 15 */ { text: "6̶̺̹͇̖̰̌̈̀̃̌T̵̗͙̙͖͖̰͗Ȕ̷͖̗͉̿͝w̶̦̯̲̗̠͈̐̾̿ř̴̥̇́̌͝j̷̛̭̯̣̱̒̐̾Ȇ̶̻̤̭͕̤̲̽͌̓v̵̧̳̘̦̲̱͉̍̒̄͠y̵͕̬̺̫̍̐̇̄͌͆M̴̡̅͗́̾̀ư̷̢̛̘̘̪̓̽͝e̵̺̪̣͙̎̀̚V̸̧̼̘͓̝̫̖̈́̀̌̆̆͂x̴̨̺̙͙͕̻̋̎̏̄̅͂̾͜ḋ̵̲̘͉͌̀̈́̓V̶̧̢̥̣̀͛̋̏́̈́͗G̶̦̦̳͔̈́̅̈́͋͛̒͑h̵̥̮͍̝̯̣̓̏̊̕E̴̱̰̟̹̤͙̱̊͆̍͐͛͋͒Y̷̙̼͖͇̱̏̅̄W̴͔̐̓̍̚͝w̵̰͒͌̀̐̋B̶̟̪͍̭̦̅̃͌͠9̷̻̳̰̳̜̪͊̔̀̇̌͐͠F̵̞̖̃̏̑3̶̢̺͋̑͝m̸̠̳̼͉̋̉́3̴̢͖͚̤͖̹̤͗ť̷̠̦̭͂̏l̷̦̮͔̫͚̓3̵̨͙̈́̂̈́̌e̷͕͚̣̻̰̘͛͘A̸̧̲̰̙̒̈́͌̈͝͝ş̸̠͓̩̯̓̋̃̊̇x̸̛͇̣̩͒͠8̷͓͓̈è̵̢̟̜̬̭̝̎́n̵̨̛͔̳̺͔̭̈́̓̈́͝͠1̶̼̻̮͑̆̽͗̔F̷̬̾̈́́̾̈͜ͅF̴̢͕̱͇̳̱́̀L̷͇̱̦͋̏̎͠ͅU̶͍͖͕͉̭͑̀̊͝=̵̜͔͔̭͊́", delay: .2, transform: "up", duration: 2000 },
                /* 16 */ { text: "V̸͉̀̆̕O̸̫̔̈͑̚8̶̫̟̰̅̆̍͐̾́͘s̶̳̭̀͊̕W̴̥̠̝̳͂̒̏̕w̸̟͖͙̎̉̈́̾̚M̷̢̰͓̱͚̀͒̋́̀͂N̵̻͛͌͑̏2̷̜̣̍̒͑̈́̃͝ͅT̷̛̘̼̥͍̰͆͊̆͘͜͠3̷͉̦̼̎̊̀͒͘̕7̴͕̾̍Ǵ̶͍͔̥̜̺̞̋̂̚͘͠q̶̓͜͜ͅ9̴̡̗̞̫̲̮̎̐͋̀̿̑͘ͅo̷̫̥̖̘̞͒͊͑̈́̒ĭ̵͈̄̽́̔͝w̷̻͗̀͘P̴̛͍̝̞̰͙͐̆̿S̵̪̪̳͒́́̋͝j̷̮̻͔͈͚̎̕̕͜6̷̧͎̫̂͆̑͛Ò̸͔̳̩͇̥͛̽̒͐l̵̡͚̱̥̭͉̳̊͋͝L̷͍̍͂̀̉͌̇H̵̜̕m̶̹͋̍͑̚O̶̘̅̈̀u̷̢͈̪̟͓̽̊̌͜D̷̢̲͐̋̾̉̓k̷̩̟̩̯̾̿͌̐R̴̯̙͊̅̍̈́͗S̴͚͔̩͇̱̺̼̀̈̅̄͑̚B̴̹͕͓͕̩̈́̉̑̌͗͘͠É̷̯̳͙̬̘͔̺̄̈́̆0̸̨͚̤̫̇̎̌͋̚/̵̝̽̒̒̆͘b̸̹̮̫̬̾̈͝Ȃ̷̻̾̕9̴̛̼̣̓̍̍̕͝a̶͔̤̰͇̳̔͝ķ̴̭̙̈́̄̉w̵̠͙͎͖̽͝=̵̯̮̮̆̔", delay: .2, transform: "down", duration: 4000 },
            ],
            backgroundAnimation: [
                { background: "#222222", color: "#3275ff", frame: 2, weight: 900 },
                { background: "#222222", color: "#fff", frame: 3, weight: 700 },

                { background: "#000", color: "#9523EE", frame: 5, weight: 900 },
                { background: "#121212", color: "#fff", frame: 6, weight: 400 },

                { background: "#1a1622", color: "#7267CB", frame: 8, weight: 900 },
                { background: "#1a1622", color: "#fff", frame: 9, weight: 400 },

                { background: "#121212", color: "#8F68FF", frame: 11, weight: 900 },
                { background: "#121212", color: "#fff", frame: 12, weight: 700 },

                { background: "#F00", color: "#000", frame: 14, weight: 900, family: 'sans-serif' },
                { background: "#000", color: "#000", frame: 16 },
            ],
            script: [
                {
                    frame: 12, callback: (frameData: IFrameData) => {
                        // Call Audio
                    }
                },
                { frame: 16, callback: (frameData: IFrameData) => {
                    setTimeout(() => {
                        alert("Ḓ̸̓̂̐̌͜͝o̶̺̻̲̙̟͇̒͐ğ̸͚̗̹̗͓̍̏̆ū̷̼̩̉͝m̵̤̿̓̿͗͐͠ ̵͉̟̩̹̅͌̾͒͆̚g̴̨̨̭͇̘͆̃͌ṳ̸̦̜̟͇͇̈̀̚ͅn̴̟̱̠͊̍̃̒̉ü̸͉͓̭͈͉̍͂̍͋͂̉ͅn̶̦̱͎̭̳͙̆̄͗ ̷̡͈̲̻͓͓́͠ͅk̸̜͚̐͆̔͗u̵̘͍͔͖̰̣͛͘t̸̼̘͇͔͋̓̿̆̀̑l̸̰͔̼̘͇̓̄̒̌̽͝ư̶̢̧̡̳̻͕̫̏ ̸̡͐̈̚ȏ̶̳͔͆͝l̷̯̬͕̾̃͠ͅs̴̝͔̳̊͐͜ű̶̧̦̫͎͓̑ǹ̵̡̲̞͒ ̴̤͌Ḿ̴̛͉̥̣̮̜͓̋̾͝͝e̴̡̟̘͉̭̗̩̅͋̃̿̂ḫ̵̳̲͉͙̏̀̇͛m̷͎̱̠̗̞̮̯̀e̵̪̣̚t̸̟͙̋̓͘ ̶̛̭͚̄E̵͖̿͛̄̿͝m̴̨̛̾̐͐́͛̀ĭ̴̟̳̙̍̽͑̽r̷̖̻͛̄͊ ̴̡̲͔̞̜̱̖̈̈́̓̏̍̌̚B̴̳͈͓͓̥͊̌͊ȯ̷̗̯͔͎̽͒͋z̵̨̓̏à̶͓̭͔̱̼ͅt̶͕̳̟̘̲̽ͅ!̴̖̦͙̰͈͈͊̎");
                        alert("Ḧ̶̛͉̝̝̹̯̪̈́̓̾́̿́͑̇͂̀̒͜͝e̷̢͍̔̓͛̓̾͑́̏̌̈́͘͝r̸͓͓̗̣͖̦̪̫̃́̑̂̋̽̃͆̂͒͛͘ ̶̡̠̝̣̹̥̹͙̣͙̓́̋̂͗̕͝͝͝ş̷̥̖̟̣̝̭͇͖̣͕͓͒̎̉̅͐̎͜͠ȩ̵̨̖̲̥̗̼̥͍̎͂̀̒̚y̷̧̨̜̝͖̜͉̥͙̻̤͔̋̋͌̌͊͂͜ ̴̢̛̹͉͚̲̮̝̭̙͍͊͂͑̽͒̊̍̚͜i̸̡̢̯̩̟͚̲͖͗͜͜ç̴̧̘̙̫͖̠̣̤̥͙͝ỉ̸̛̳̝̬͚̖̩̱̪̤̥͉̊̍̆̐̉̑̓̐͝n̴͖͈̮̯̙̺͖̖̠̪̳̒̏̽̌̓̋͂̐̅̕ͅ ̷̥̜̃̾͒͂̇͂͗ţ̴̠̲̲͇̰̗̹̬̥͈̣̀̽̐̃̓e̵̡͊̀͒͑̄ş̵̨̢͉̻̜̤̥̤͆͛̈́̎͒̎̕͜ę̵̡̛̺̘̼̌͊̈́k̴̨̧̯͐̏̔́̓̃̅̔̈̈͑̂́̕͠k̴̢̛͍̰̞̒̐́̔̽̈̍̓́̒̚ü̵̪̩͎̥̯͗̓̚r̵̨̪̻͎͔̻͖̘͖͊̓̍̄̍́͂̕͠l̴̮̦̲͓͌̉ȩ̷̢̺̜̙͓͙̗̦͖̪́͗̃́́͊̀̀̅͆̏͐̐̇͑ͅȑ̵̛͉͋̆̄̋̊̔͋̎̈́̔̉̚");
                        /* localStorage.setItem('visited', "true"); */
                    }, 3000)
                } }
            ]
        }
    },
    mounted() {
        if (localStorage.getItem("visited")) {
            alert("B̸̜̓̈́ȅ̷̘͚̊n̴͕̯͕̍̆̾̃͂͘͝i̵͎̘̕͝ ̶̝͚̙̗͔͉̇͆̈̾͜ẏ̶̧̢͙̠̫̦̱̆͑̾͘̚a̴̩̩͚͍̋͝ļ̵̟̹̲͌͂̽ń̶̳͙͑̀͂̎͘ı̴̮͙̳̄͐z̷͇̎̀̈́ ̶̟̞̰̭̮̹̉b̶̜̣̂̏́̌͗͠ı̷͇͋r̶̩̾̆̓̄̽ạ̷̧̠̯͖̋̀͆͌̌̑̾k̶̬͕̈̇͑̈.̷̟̹̑́͝.̵͐̂͘͜.̶̣̭͚̺͕̬͓̏");
            window.location.href = "https://google.com";
        }
    },
    components: {
        Loader,
        JumpText
    },
    methods: {
        startAnimation() {
            let duration = 1000;
            this.animation.forEach((frameData, frame) => {
                setTimeout(() => {
                    const animate = () => {
                        // Animate Background
                        this.backgroundAnimation.forEach((bgFrame) => {
                            if (get(bgFrame, "frame") == frame) {
                                this.background = bgFrame.background;
                                this.color = bgFrame.color;

                                if(bgFrame.weight) this.fontWeight = bgFrame.weight;
                                if(bgFrame.family) this.fontFamily = bgFrame.family;
                            }
                        })

                        // Run Frame Script
                        this.script.forEach((sFrame) => {
                            if (get(sFrame, 'frame') == frame) {
                                sFrame.callback(frameData);
                            }
                        })

                        // Animate Text
                        let text = document.getElementById(`frame-${frame}`);

                        text?.classList.add(frameData.transform);

                        text?.classList.add('loaded')

                        setTimeout(() => {
                            text?.classList.add('toggle');
                        }, 100);
                    }

                    if (frame > 0) {
                        let lastFrame = document.getElementById(`frame-${frame - 1}`);
                        let lastFrameData = this.animation[frame - 1];

                        lastFrame?.classList.remove(lastFrameData.transform)

                        lastFrame?.classList.add('noToggle');

                        setTimeout(() => {
                            lastFrame?.classList.remove('loaded')
                            setTimeout(() => {
                                animate();
                            }, 200)
                        }, 300)
                        return;
                        
                    }
                    animate();
                }, duration);
                duration += frameData.duration;
            })
        }
    },
    watch: {
        isPageLoaded(newV) {
            if (newV) this.startAnimation()
        }
    }
})
</script>
  
<style lang="scss">
@import '@/style/main.scss';
</style>