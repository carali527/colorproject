<template>
  <div class="outer-wrap">
    <div class="side-bar">
      <p>Color Scheme</p>
      <div
        class="show-color"
        :style="{
          'background-color': `rgb(${this.red}, ${this.green}, ${this.blue})`,
        }"
      ></div>
      <create-color
        @create-color="showColor"
        v-model:edited-category="category"
        v-model:edited-name="name"
        v-model:edited-red="red"
        v-model:edited-green="green"
        v-model:edited-blue="blue"
      ></create-color>
    </div>
    <div class="main-body">
      <nav>
        <div class="nav nav-tabs all-categories" id="nav-tab" role="tablist">
          <template v-for="categories in colorList" :key="categories.id">
            <a
              class="nav-link categories"
              :class="{ active: tabCategory === categories.category }"
              @click="tabCategory = categories.category"
              >{{ categories.category }}</a
            >
          </template>
        </div>
      </nav>
      <div class="tab-content" id="nav-tabContent">
        <div class="tab-pane" :class="{ active: true }">
          <div class="all-the-color">
            <template v-for="categorylist in colorList">
              <div
                class="card"
                v-for="(item, index) in categorylist.list"
                :key="index"
                v-show="tabCategory == categorylist.category"
              >
                <template v-if="editing == item">
                  <edited-color
                    @complete-color-item="completeColor(item)"
                    @editing-cancel="cancel()"
                    :edited-name="editingName"
                    :edited-red="editingColorRed"
                    :edited-green="editingColorGreen"
                    :edited-blue="editingColorBlue"
                    v-model:edited-name="editingName"
                    v-model:edited-red="editingColorRed"
                    v-model:edited-green="editingColorGreen"
                    v-model:edited-blue="editingColorBlue"
                  >
                    <div
                      class="color-box-editing"
                      :style="{
                        'background-color': `rgb(${this.editingColorRed}, ${this.editingColorGreen}, ${this.editingColorBlue})`,
                      }"
                    ></div>
                  </edited-color>
                </template>
                <template v-else>
                  <color-component
                    :item="item"
                    @edited-the-colors="editColor(item)"
                    @delete-the-color="deleteColor(item)"
                  >
                    <div
                      class="color-box"
                      :style="{
                        'background-color': `rgb(${item.red}, ${item.green}, ${item.blue})`,
                      }"
                    ></div>
                  </color-component>
                </template>
              </div>
            </template>
          </div>
        </div>
      </div>
    </div>
  </div>
  <footer>
    <p>{{ footer.year }} Cara. All rights reserved.</p>
  </footer>
</template>

<script>
import CreateColor from "./components/CreateColor.vue";
import EditedColor from "./components/EditedColor.vue";
import ColorComponent from "./components/ColorComponent.vue";
import axios from "axios";

const axiosClient = axios.create({ baseURL: "" });

export default {
  name: "App",
  components: {
    CreateColor,
    EditedColor,
    ColorComponent,
  },
  data() {
    return {
      red: 0,
      green: 0,
      blue: 0,
      id: 1,
      name: "",
      category: "",
      editing: null,
      editingColorRed: 0,
      editingColorGreen: 0,
      editingColorBlue: 0,
      editingName: "",
      editingCategory: "",
      tabCategory: true,
      colorList: [],
      footer: {
        copyright: "copyright",
        year: "@" + new Date().getFullYear(),
      },
    };
  },
  methods: {
    refresh() {
      axiosClient
        .get("/colorList/")
        .then((res) => {
          this.colorList = res.data;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    updateData(item) {
      // find the order of category
      let findCategory = this.colorList.findIndex((element) => {
        return element.category === item.category;
      });
      // find the id of it
      let findID = this.colorList[findCategory].id;
      let a = this.colorList[findCategory];
      axiosClient.put(`/colorList/${findID}`, a).finally(() => {
        this.refresh();
      });
    },
    deleteColor(item) {
      let findCategory = this.colorList.findIndex((element) => {
        return element.category === item.category;
      });
      let findID = this.colorList[findCategory].id;
      let obj = this.colorList[findCategory].list.indexOf(item);
      let colorListLength = this.colorList[findCategory].list.length;
      if (colorListLength === 1) {
        // delete the color including the category
        axiosClient.delete(`/colorList/${findID}`).finally(() => {
          this.refresh();
        });
      } else {
        //delete the color in the category only
        this.colorList[findCategory].list.splice(obj, 1);
        this.updateData(item);
      }
    },
    editColor(item) {
      this.editing = item;
      this.editingColorRed = item.red;
      this.editingColorGreen = item.green;
      this.editingColorBlue = item.blue;
      this.editingName = item.name;
    },
    completeColor(item) {
      if(this.editingColorRed === "" || this.editingColorGreen === "" || this.editingColorBlue === ""){
        alert("Please don't leave any blank.");
      }
      else if(this.editingColorRed < 0 || this.editingColorGreen < 0 || this.editingColorBlue < 0){
        alert("The number range will be 0 to 255.");
      }
      else if(this.editingColorRed >255 || this.editingColorGreen >255 || this.editingColorBlue >255){
        alert("Max with three numbers and don't let the number over 255.");
      }else{
        item.red = this.editingColorRed;
        item.green = this.editingColorGreen;
        item.blue = this.editingColorBlue;
        item.name = this.editingName;
        this.updateData(item);
        this.cancel();
      }
    },
    cancel() {
      this.editing = null;
      this.editingColorRed = "";
      this.editingColorGreen = "";
      this.editingColorBlue = "";
      this.editingName = "";
    },
    showColor(editedColors) {
      let R = editedColors.editedRed;
      let G = editedColors.editedGreen;
      let B = editedColors.editedBlue;
      let category = editedColors.editedCategory;
      let name = editedColors.editedName;
      let colorList = this.colorList;
      let findCategory = colorList.findIndex((element) => {
        return element.category === category;
      });
      let a = this.colorList[findCategory];
      // check the data validation first
      R = (!R)? 0 : R;
      G = (!G)? 0 : G;
      B = (!B)? 0 : B;
      if(R < 0 || G < 0 || B < 0){
        alert("Note:The number range will be 0 to 255.");
      }
      else if (R > 255 || G > 255 || B > 255) {
        alert("Please don't let the number over 255, it will be assumed to be 255.");
      } else if (findCategory == -1) {
        // push the data to database
        axiosClient
          .post(`/colorList`, {
            category: category,
            list: [
              { category: category, name: name, red: R, green: G, blue: B },
            ],
          })
          .finally(() => {
            this.refresh();
          });
      } else {
        this.colorList[findCategory].list.push({
          category: category,
          name: name,
          red: R,
          green: G,
          blue: B,
        });
        let findID = this.colorList[findCategory].id;
        axiosClient.put(`/colorList/${findID}`, a).finally(() => {
          this.refresh();
        });
      }
    },
  },
  mounted: function() {
    this.refresh();
  },
};

</script>

<style>
@import "./assets/scss/style.css";
</style>
