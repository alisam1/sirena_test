<template>
  <div class="autocomplete">
    <div class="input" @click="toggleVisible" v-text="selectedItem ? selectedItem[filterby] : ''"></div>
    <div class="placeholder" v-if="selectedItem == null" v-text="title"></div>
    <button class="close" @click="selectedItem = null" v-if="selectedItem">x</button>
    <div class="popover" v-show="visible">
      <input type="text"
        ref="input"
        v-model="query"
        @keydown.up="up"
        @keydown.down="down"
        @keydown.enter="selectItem"
        placeholder="Start Typing...">
      <div class="options" ref="optionsList">
        <ul>
          <li v-for="(match, index) in matches"
            :key="index"
            :class="{ 'selected': (selected == index)}"
            @click="itemClicked(index)"
            v-text="match[filterby]"></li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    props: {
      items: {
        default: [],
        type: Array
      },
      filterby: {
        type: String
      },
      title: {
        default: 'Select One...',
        type: String
      },
      shouldReset: {
        type: Boolean,
        default: true
      }
    },
    data() {
      return {
        itemHeight: 39,
        selectedItem: null,
        selected: 0,
        query: '',
        visible: false
      };
    },
    methods: {
      toggleVisible() {
        this.visible = !this.visible;

        setTimeout(() => {
          this.$refs.input.focus();
        }, 50);
      },
      itemClicked(index) {
        this.selected = index;
        this.selectItem();
      },
      selectItem() {
        if (!this.matches.length) {
          return;
        }

        this.selectedItem = this.matches[this.selected];
        this.visible = false;

        if (this.shouldReset) {
          this.query = '';
          this.selected = 0;
        }

        this.$emit('selected', JSON.parse(JSON.stringify(this.selectedItem)));
      },
      up() {
        if (this.selected == 0) {
          return;
        }

        this.selected -= 1;
        this.scrollToItem();
      },
      down() {
        if (this.selected >= this.matches.length - 1) {
          return;
        }

        this.selected += 1;
        this.scrollToItem();
      },
      scrollToItem() {
        this.$refs.optionsList.scrollTop = this.selected * this.itemHeight;
      }
    },
    computed: {
      matches() {
        this.$emit('change', this.query);

        if (this.query == '') {
          return [];
        }

        return this.items.filter((item) => item[this.filterby].toLowerCase().includes(this.query.toLowerCase()))
      }
    }
  }
</script>

<style lang="scss">
.autocomplete {
    width: 50%;
    margin: 0 auto;
    position: relative;

     @media (max-width: 650px) {
       width: 90%;
      }
}
.input {
    font-family: 'Open Sans', sans-serif;
    width: 100%;
    height: 40px;
    border-radius: 3px;
    border: 2px solid #01A39D;
    box-shadow: 0 0 10px #01A39D;
    color: #fff;
    font-size: 25px;
    padding-left: 10px;
    padding-top: 10px;
    cursor: text;
    margin: 0 auto;

    @media (max-width: 650px) {
       width: 90%;
       font-size: 20px;
    }
}
.close {
    font-family: 'Open Sans', sans-serif;
    position: absolute;
    right: 2px;
    top: 4px;
    background: none;
    border: none;
    font-size: 30px;
    color: lightgrey;
    cursor: pointer;

    @media (max-width: 650px) {
        right: 14px;
        top: 2px;
    }
}

.placeholder {
    font-family: 'Open Sans', sans-serif;
    position: absolute;
    top: 11px;
    left: 11px;
    font-size: 25px;
    color: #d0d0d0;
    pointer-events: none;

    @media (max-width: 650px) {
       font-size: 15px;
       top: 15px;
       padding-left: 15px;
    }
}
.popover {
    width: 101.3%;
    min-height: 50px;
    border: 2px solid lightgray;
    position: absolute;
    top: 55px;
    left: 0;
    right: 0;
    background: transparent;
    border-radius: 3px;
    text-align: center;

    @media (max-width: 650px) {
       width: 94%;
       margin: 0 auto;
    }
}
.popover input {
    font-family: 'Open Sans', sans-serif;
    width: 95%;
    margin-top: 15px;
    height: auto;
    font-size: 16px;
    color: #fff;
    border-radius: 3px;
    background: transparent;
    border: none;
    padding-left: 8px;
    outline: none;
}
.options {
    max-height: 150px;
    overflow-y: scroll;
    margin-top: 5px;
}
.options ul {
    list-style-type: none;
    text-align: left;
    padding-left: 0;
}
.options ul li {
    font-family: 'Open Sans', sans-serif; 
    border-bottom: 1px solid lightgray;
    padding: 10px;
    cursor: pointer;
    background: transparent;
    color: #fff;
}
.options ul li:first-child {
    border-top: 2px solid #d6d6d6;
}

.options ul li:not(.selected):hover {
    background: #8c8c8c;
    color: #fff;
}
.options ul li.selected {
    background: #01A39D;
    color: #fff;
    font-weight: 600;
}
</style>

