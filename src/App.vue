<template>
    <div class="container">
        <div class="row">
            <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
                <h1>Animations</h1>
                <hr>
                <select v-model="alertAnimation" class="form-control">
                <option value="fade">fade</option>
                <option value="Slide">Slide</option>
                </select>
                <br><br>
                <button class="btn btn-primary" @click="show = !show">Show Alert</button>
                <br><br>
                <transition :name="alertAnimation">
                <div class="alert alert-info" v-show="show">This is sone Info</div>
                </transition>
                <transition name="slide" type="animation" appear>
                <div class="alert alert-info" v-if="show">This is sone Info</div>
                </transition>
                <transition 
                  enter-active-class="animated bounce"
                  leave-active-class="animated shake"                
                >
                <div class="alert alert-info" v-if="show">This is sone Info</div>
                </transition>
                <transition :name="alertAnimation" mode="out-in">
                <div class="alert alert-info" v-if="show" key="info">This is sone Info</div>
                <div class="alert alert-warning" v-else key="warning">This is sone warning</div>
                </transition>
                <hr>
                <button class="btn btn-primary" @click="load = !load">Load / Remove Element</button>
                <br><br>
                <transition 
                  @before-enter="beforeEnter"
                  @enter="enter"
                  @after-enter="afterEnter"
                  @enter-cancelled="enterCancelled"
                  
                  @before-leave="beforeLeave"
                  @leave="leave"
                  @after-leave="afterLeave"
                  @leave-cancelled="leaveCancelled" 
                  :css="false">
                  <div style="width: 300px; height: 100px; background-color: lightgreen" v-if="load"></div>
                </transition>
                <hr>
                <button class="btn btn-primary" 
                  @click="selectedComponent == 'app-success-alert' ? selectedComponent = 'app-danger-alert' : selectedComponent = 'app-success-alert'">Toogle Coponents</button>
                <br><br>
                <transition name="fade" mode="out-in">
                <component :is="selectedComponent"></component>
                </transition>
                <hr>
                <button class="btn btn-primary" @click="addItem">Add Item</button>
                <br><br>
                <ul class="list-group">
                <transition-group name="slide">
                  <li
                   class="list-group-item" v-for="(number, index) in numbers" :key="number" @click="removeItem(index)" 
                  style="cursor: pointer">{{ number }}
                  </li>
                </transition-group>
               </ul>
            </div>
        </div>
    </div>
</template>

<script>
import DangerAlertVue from "./DangerAlert.vue";
import SuccessAlertVue from "./SuccessAlert.vue";

    export default {
        data() {
            return {
              show: false,
              load: true,
              alertAnimation: 'fade',
              elementWidth: 100,
              selectedComponent: 'app-success-alert',
              numbers: [1, 2, 3, 4, 5]
            }
        },
        methods: {
           beforeEnter(el) {
             console.log('beforeEnter');
             this.elementWidth = 100;
             el.style.width = this.elementWidth + 'px';
           },
           enter(el,done) {
             console.log('enter');
             let round = 1;
             const interval = setInterval(() => {
               el.style.width = (this.elementWidth + round * 10) + 'px';
               round++;
               if (round > 20) {
                 clearInterval(interval);
                  done();
               }
             }, 20); 
           },
           afterEnter() {
             console.log('afterEnter');
           
           },
           enterCancelled() {
             console.log('enterCancelled');
          
           },
           beforeLeave(el) {
             console.log('beforeLeave');
             this.elementWidth = 300;
             el.style.width = this.elementWidth + 'px';
            
           },
           leave(el,done) {
             console.log('leave');
             let round = 1;
             const interval = setInterval(() => {
               el.style.width = (this.elementWidth - round * 10) + 'px';
               round++;
               if (round > 20) {
                 clearInterval(interval);
                  done();
               }
             }, 20);
           },
           afterLeave() {
             console.log('afterLeave');
           
           },
            leaveCancelled() {
              console.log('leaveCancelled');
            },
            addItem() {
              const pos = Math.floor(Math.random() * this.numbers.length);
              this.numbers.splice(pos, 0, this.numbers.length + 1)
            },
            removeItem(index) {
              this.numbers.splice(index, 1);
            }
          },
          components: {
            appDangerAlert: DangerAlertVue,
            appSuccessAlert: SuccessAlertVue
          }
        }
    
</script>

<style>
   .fade-enter {
     opacity: 0;
   }

   .fade-enter-active {
     transition: opacity 1s;
   }

   .fade-leave {
     /* opacity: 1; */
   }

   .fade-leave-active {
     transition: opacity 1s;
      opacity: 0;
   }

   .slide-enter {
     opacity: 0;
     /* transform: translateY(20px); */
   }

   .slide-enter-active {
     animation: slide-in 1s ease-out forwards;
     transition: opacity .5s;
   }

   .slide-leave {

   }

   .slide-leave-active {
     animation: slide-out 1s ease-out forwards;
     transition: opacity 1s;
     opacity: 0;
     position: absolute;
   }

   .slide-move {
     transition: transform 1s;
   }

   @keyframes slide-in {
       from {
         transform: translateY(20px);
       }
       to {
         transform: translateY(0);
       }
   }

   @keyframes slide-out {
       from {
         transform: translateY(0);
       }
       to {
         transform: translateY(20px);
       }
   }
</style>