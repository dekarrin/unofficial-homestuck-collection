<template>
  <transition name="jumpbox">
    <div class="jumpBox" v-show="isActive" @keydown.esc="close()" @focusout="onFocusOut" tabindex="-1">
      <div class="wrapper">
        <a :href="jumpboxText" class="jumpboxLink" ref="link">
        </a>
        <input class="jumpBoxInput" ref="input" type="text" spellcheck="false" v-model="jumpboxText"  @keydown.enter="focusLink()" />
      </div>
    </div>
  </transition>
</template>

<script>

export default {
  name: 'jumpBox',
  props: [
		'tab'
  ],
  data() {
    return {
      isActive: false,
      jumpboxText: ""
    }
  },
  computed: {
  },
  methods: {
		open() {
      this.isActive = true
      this.$nextTick(()=>{
        this.jumpboxText = this.tab.url
        setTimeout(()=>{
          this.$refs.input.focus()
          this.$refs.input.select()
        }, 10)
      })
    },
    onFocusOut(event) {
      if (document.getElementById('contextMenu').contains(event.relatedTarget)) {
        this.$root.$children[0].$refs.contextMenu.lendFocus(event.target)
      }
      else if (!this.$el.contains(event.relatedTarget)) this.close()
    },
		close(){
      this.isActive = false
		},
		toggle(){
      if (this.isActive) this.close()
      else this.open()
    },
    focusLink(){
      this.$refs.link.click()
    }
  },
  mounted() {
  },
	watch: {
		'tab.history'() {
			this.close()
		}
	}  
}
</script>

<style lang="scss" scoped>
  .jumpBox {
    font-family: initial;
    font-weight: initial;
    font-size: initial;
    color: #000;
    
    position: fixed;
    width: 100%;
    top: calc(var(--headerHeight) + 50px);
    z-index: 4;

    .wrapper {
      box-shadow: 0 0 4px 0 var(--ctx-shadow);
      border: 4px solid var(--ctx-frame);
      flex-flow: row nowrap;
      width: max-content;
      margin: 0 auto;
      display: flex;

      .jumpboxLink {
        background: #fff;
        font-size: 25px;
        text-decoration: none;
        color: #000;

        justify-content: center;
        align-items: center;
        display: flex;

        &::after {
          text-align: center;
          width: 34px;
          margin: 0;
        }
      }
      input {
        padding: 2px 0;
        font-size: 22px;
        height: 30px;
        width: 500px;
        border: none;
      }
    }
  }

  .jumpbox-enter-active, .jumpbox-leave-active {
		transition: all 0.1s;
	}
	.jumpbox-enter, .jumpbox-leave-to {
    opacity: 0;
		transform: translateY(-10px);
	}
</style>