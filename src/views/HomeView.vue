<template>
  <head>
    <link
      href="https://fonts.googleapis.com/css?family=Inter"
      rel="stylesheet"
    />
  </head>
  <div id="app" class="container">
    <div class="container__drag">
      <p>Elements</p>
      <div>
        <draggable
          :list="icons"
          item-key="id"
          :group="{
            name: 'myGroup',
            pull: 'clone',
            put: false,
          }"
        >
          ><template #item="{ element }">
            <div class="container__drag--iconbox">
              <div class="container__drag--icons">
                <img :src="element.img" />{{ element.type }}
              </div>
            </div></template
          ></draggable
        >
      </div>
    </div>
    <div class="container__drop">
      <div class="drop__box">
        <p>Header</p>
        <div :class="{ 'container__drop--text': !imageDropped.length }">
          <a v-if="!imageDropped.length"
            >Drag and drop an element within this area.</a
          >
          <draggable
            :list="imageDropped"
            item-key="id"
            :clone="insertImage(imageDropped)"
            :group="{ name: 'myGroup', pull: true, put: true }"
            :emptyInsertThreshold="150"
          >
            <template #item="{ element }">
              <div class="container__drop--iconbox">
                <div class="container__drop--icontext">
                  <img :src="element.img" /> <a>{{ element.type }}</a>
                </div>
              </div>
            </template>
          </draggable>
        </div>
      </div>
      <div class="drop__box">
        <p>Body</p>
        <div :class="{ 'container__drop--text': !anyDropped.length }">
          <a v-if="!anyDropped.length"
            >Drag and drop an element within this area.</a
          >
          <draggable
            :list="anyDropped"
            item-key="id"
            :clone="insertAny(anyDropped)"
            :group="{ name: 'myGroup', pull: true, put: true }"
            :emptyInsertThreshold="150"
          >
            <template #item="{ element }">
              <div class="container__drop--iconbox">
                <div class="container__drop--icontext">
                  <img :src="element.img" /> <a>{{ element.type }}</a>
                </div>
              </div></template
            >
          </draggable>
        </div>
      </div>
      <div class="drop__box">
        <p>Footer</p>
        <div :class="{ 'container__drop--text': !textDropped.length }">
          <a v-if="!textDropped.length"
            >Drag and drop an element within this area.</a
          >
          <draggable
            :list="textDropped"
            item-key="id"
            :clone="insertText(textDropped)"
            :group="{ name: 'myGroup', pull: true, put: true }"
            :emptyInsertThreshold="150"
          >
            <template #item="{ element }">
              <div class="container__drop--iconbox">
                <div class="container__drop--icontext">
                  <img :src="element.img" /> <a>{{ element.type }}</a>
                </div>
              </div></template
            >
          </draggable>
        </div>
      </div>
    </div>
  </div>
  <button v-on:click="resetAll">RESET</button>
</template>

<script>
import { defineComponent } from "vue";
import draggable from "vuedraggable";
import textIcon from "../assets/icon_text.png";
import imageIcon from "../assets/icon_image.png";
import tableIcon from "../assets/icon_table.png";

export default defineComponent({
  name: "App",
  components: {
    draggable,
  },
  data: function () {
    return {
      icons: [
        { type: "Text", img: textIcon, originBox: "", id: 1 },
        { type: "Image", img: imageIcon, originBox: "", id: 2 },
        { type: "Table", img: tableIcon, originBox: "", id: 3 },
      ],
      textDropped: [],
      imageDropped: [],
      anyDropped: [],
    };
  },
  methods: {
    insertImage(iconElements) {
      iconElements.forEach((element, index) => {
        if (element.id !== 2) {
          iconElements.splice(index, 1);
          switch (element.originBox) {
            case "textbox":
              this.textDropped.push(element);
              break;
            case "anybox":
              this.anyDropped.push(element);
              break;
            default:
              break;
          }
        } else element.originBox = "imagebox";
      });
    },

    insertAny(iconElements) {
      iconElements.forEach((element) => {
        element.originBox = "anybox";
      });
    },

    insertText(iconElements) {
      iconElements.forEach((element, index) => {
        if (element.id !== 1) {
          iconElements.splice(index, 1);
          switch (element.originBox) {
            case "imagebox":
              this.imageDropped.push(element);
              break;
            case "anybox":
              this.anyDropped.push(element);
              break;
            default:
              break;
          }
        } else element.originBox = "textbox";
      });
    },

    resetAll() {
      this.textDropped = [];
      this.imageDropped = [];
      this.anyDropped = [];
    },
  },
});
</script>

<style lang="scss">
html,
body,
div {
  font-family: "Inter";
  font-weight: bold;
  font-size: 14px;
  color: #3a6b88;
}

.container {
  display: flex;
  flex-direction: row-reverse;

  .container__drop {
    display: flex;
    flex-direction: column;
    flex-basis: auto;
    padding: 60px;
    width: 70%;
    height: 75vh;
    border-radius: 4px;
    gap: 24px;
    text-align: left;
    overflow: auto;
    scrollbar-width: thin;

    .drop__box {
      display: flex;
      flex-direction: column;
      padding: 0px 8px 16px 8px;
      background-color: #f6f6f6;

      .container__drop--text {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: center;
        align-items: center;
        color: #3f3f3f;
        font-weight: lighter;
        background-color: #ffffff;
        border: 1px dashed #3a6b88;
        min-height: 13vh;
      }

      .container__drop--iconbox {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        background-color: #ffffff;
        height: 13vh;
        margin: 5px;
        .container__drop--icontext {
          display: flex;
          flex-direction: column;
          text-align: center;
          font-size: 12px;
        }
      }
    }
  }

  .container__drag {
    display: flex;
    flex-direction: column;
    width: 30%;
    width: 30%;
    text-align: left;
    padding-left: 5px;
    background-color: #f6f6f6;
    height: 75vh;

    .container__drag--iconbox {
      width: 80px;
      height: 80px;
      background-color: #ffffff;
      display: inline-flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      margin: 10px 10px 0 10px;
      transition: all 0.5s;

      .container__drag--icons {
        display: flex;
        flex-direction: column;
        align-items: center;
        font-size: 12px;
      }
    }
  }
}
</style>
