<script setup>
import {
  faMinus,
  faPlus,
  faCopy,
  faCubes,
  faSquareFull,
  faCircleArrowLeft,
  faArrowRotateLeft,
} from "@fortawesome/free-solid-svg-icons";
import { ref, reactive, onMounted, watch, watchEffect, computed } from "vue";

let containerProperties = [
  {
    model: ref(""),
    name: "Flex-direction",
    options: [
      "flex-column",
      "flex-column-reverse",
      "flex-row",
      "flex-row-reverse",
    ],
    isSelected: ref(false),
  },
  {
    model: ref(""),
    name: "Align-items",
    options: [
      "align-items-start",
      "align-items-end",
      "align-items-center",
      "align-items-baseline",
      "align-items-stretch",
    ],
    isSelected: ref(false),
  },
  {
    model: ref(""),
    name: "Justify-content",
    options: [
      "justify-content-start",
      "justify-content-end",
      "justify-content-center",
      "justify-content-between",
      "justify-content-around",
      "justify-content-evenly",
    ],
    isSelected: ref(false),
  },
  {
    model: ref(""),
    name: "Gap",
    options: ["gap-1", "gap-2", "gap-3", "gap-4", "gap-5"],
    isSelected: ref(false),
  },
  {
    model: ref(""),
    name: "Flex-wrap",
    options: ["flex-wrap", "flex-nowrap", "flex-wrap-reverse"],
    isSelected: ref(false),
  },
];

let itemProperties = [
  {
    model: ref(""),
    name: "Width",
    options: ["w-auto", "w-25", "w-50", "w-75", "w-100"],
    isSelected: ref(false),
  },
  {
    model: ref(""),
    name: "Height",
    options: ["h-auto", "h-25", "h-50", "h-75", "h-100"],
    isSelected: ref(false),
  },
  {
    model: ref(""),
    name: "Align-self",
    options: [
      "align-self-start",
      "align-self-end",
      "align-self-center",
      "align-self-baseline",
      "align-self-stretch",
    ],
    isSelected: ref(false),
  },
  {
    model: ref(""),
    name: "Align-items",
    options: [
      "align-items-start",
      "align-items-end",
      "align-items-center",
      "align-items-baseline",
      "align-items-stretch",
    ],
    isSelected: ref(false),
  },
  {
    model: ref(""),
    name: "Justify-content",
    options: [
      "justify-content-start",
      "justify-content-end",
      "justify-content-center",
      "justify-content-between",
      "justify-content-around",
      "justify-content-evenly",
    ],
    isSelected: ref(false),
  },
  {
    model: ref(""),
    name: "Flex-grow",
    options: ["flex-grow-0", "flex-grow-1"],
    isSelected: ref(false),
  },
  {
    model: ref(""),
    name: "Flex-shrink",
    options: ["flex-shrink-0", "flex-shrink-1"],
    isSelected: ref(false),
  },
  {
    model: ref(""),
    name: "Order",
    options: [
      "order-first",
      "order-last",
      "order-0",
      "order-1",
      "order-2",
      "order-3",
      "order-4",
      "order-5",
    ],
    isSelected: ref(false),
  },
];

let itemArr = ref([]);
let itemNumber = ref(1);

let containerCss = reactive({});
let itemCss = reactive({});

let containerClasses = computed(() => getClasses(containerProperties));
let itemClasses = computed(() => getClasses(itemProperties));

const convertedContainerCSS = computed(() =>
  convertToCssFormat("container", containerCss, containerClasses)
);

const convertedItemCSS = computed(() =>
  convertToCssFormat("item", itemCss, itemClasses)
);

const getClasses = (elements) => {
  elements
    .filter((e) => e.model.value == "")
    .map((e) => (e.isSelected.value = false));

  let isSelectedItems = elements.filter((e) => e.model.value !== "");

  isSelectedItems.map((e) => (e.isSelected.value = true));

  return isSelectedItems.map((e) => e.model.value).join(" ");
};

// update the value of each container model/properties when changed
const updateContainerClasses = (index, value) => {
  containerProperties[index].model.value = value;
};

// update the value of each item model/properties when changed
const updateItemClasses = (index, value) => {
  itemProperties[index].model.value = value;
};

const displayItems = () => {
  // remove all the items
  itemArr.value.splice(0, itemArr.value.length);

  // add new number of items
  for (let i = 0; i < itemNumber.value; i++) {
    itemArr.value.push(`Item ${i + 1}`);
  }
};

const convertClassesToObject = (cssStyles, classes) => {
  if (typeof classes !== "string") {
    return;
  }

  classes.split(" ").forEach((className) => {
    let classProperty = className.split("-");
    let key = classProperty.slice(0, classProperty.length - 1).join("-");
    let value = classProperty[classProperty.length - 1];

    if (value == "") return;

    switch (key) {
      case "h":
        key = "height";
        value = convertCSSLength(value);
        break;
      case "w":
        key = "width";
        value = convertCSSLength(value);
        break;
      case "gap":
        value = checkGapValue(value);
        break;
      case "flex":
        key = "flex-wrap";
        break;
      default:
        break;
    }

    if (
      value == "column" ||
      value == "column-reverse" ||
      value == "row" ||
      value == "row-reverse"
    ) {
      key = "flex-direction";
    }

    cssStyles[key] = value;
  });

  return cssStyles;
};

const checkGapValue = (value) => {
  if (value == 1) return "0.25rem";
  if (value == 2) return "0.5rem";
  if (value == 3) return "1rem";
  if (value == 4) return "1.5rem";
  if (value == 5) return "3rem";
};

const convertCSSLength = (value) => {
  if (value == 25) return "25%";
  if (value == 50) return "50%";
  if (value == 75) return "75%";
  if (value == 100) return "100%";
};

const convertToCssFormat = (name, target, classes) => {
  let obj = convertClassesToObject(target, classes.value);

  let cssString = `.${name} {\n`;
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      cssString += `\t${key}: ${obj[key]};\n`;
    }
  }
  cssString += "}";
  return cssString;
};

const minusItem = () => {
  if (itemNumber.value === 1) return;
  itemNumber.value--;
  displayItems();
};

const addItem = () => {
  if (itemNumber.value === 5) return;
  itemNumber.value++;
  displayItems();
};

const copyClasses = (textToCopy) => {
  navigator.clipboard.writeText(textToCopy);
};

const resetFlexboxClasses = () => {
  for (let i = 0; i < containerProperties.length; i++) {
    containerProperties[i].model.value = "";
  }
  for (let i = 0; i < itemProperties.length; i++) {
    itemProperties[i].model.value = "";
  }
  itemNumber.value = 1;
  displayItems();
};

// initiate initial number of items
displayItems();
</script>

<template>
  <section
    class="tab-pane fade"
    id="flexbox-visualizer"
    role="tabpanel"
    aria-labelledby="flexbox-visualizer-link"
    tabindex="0"
  >
    <div
      class="container-fluid px-4 d-flex align-items-center justify-content-center"
    >
      <div
        class="page-content p-4 shadow-sm gap-3 card flex-row d-flex justify-content-center align-items-center"
      >
        <div class="properties p-4 d-flex flex-column align-self-stretch card">
          <h2 class="fs-6 fw-semibold border-bottom pb-2">
            <font-awesome-icon :icon="faCubes" />
            Flexbox bootstrap
          </h2>
          <div class="d-flex flex-column gap-2 w-100 h-100">
            <!-- CONTAINER PROPERTIES -->
            <div class="container-properties w-100">
              <div
                class="mb-3 py-2 border-bottom d-flex align-items-center justify-content-center"
              >
                <h2
                  class="fs-8 m-0 flex-grow-1 d-flex gap-2 align-items-center"
                >
                  <font-awesome-icon :icon="faSquareFull" />
                  Container
                </h2>
                <button
                  class="reset-flexbox btn rounded-1 py-1 px-3 fs-8 d-flex align-items-center gap-2"
                  @click="resetFlexboxClasses()"
                >
                  <font-awesome-icon :icon="faArrowRotateLeft" class="fs-9" />
                  Reset
                </button>
              </div>
              <div class="d-flex gap-2 flex-wrap">
                <select
                  v-for="(select, index) in containerProperties"
                  class="form-select mb-1 fs-8"
                  :class="{
                    'itemSelected shadow-sm': select.isSelected.value == true,
                  }"
                  :key="index"
                  v-model="select.model.value"
                  @change="updateContainerClasses(index, $event.target.value)"
                >
                  <option value="">{{ select.name }}</option>
                  <option
                    v-for="(item, index) in select.options"
                    :value="item"
                    :key="index"
                  >
                    {{ item }}
                  </option>
                </select>
              </div>
            </div>

            <!-- ITEM PROPERTIES -->
            <div class="item-properties w-100">
              <div
                class="mb-3 py-2 gap-2 border-bottom d-flex align-items-center justify-content-center"
              >
                <h2
                  v-if="itemNumber == 1"
                  class="fs-8 m-0 d-flex gap-2 align-items-center flex-grow-1"
                >
                  <font-awesome-icon :icon="faSquareFull" />
                  Item
                </h2>
                <h2
                  v-else
                  class="fs-8 m-0 d-flex gap-2 align-items-center flex-grow-1"
                >
                  <font-awesome-icon :icon="faSquareFull" />
                  Items
                </h2>
                <div
                  class="item-number position-relative rounded-1 gap-3 d-flex align-items-center justify-content-center"
                >
                  <button class="btn py-1 fs-8" @click="minusItem()">
                    <font-awesome-icon :icon="faMinus" />
                  </button>
                  <span class="fw-semibold position-absolute">{{
                    itemNumber
                  }}</span>
                  <button class="btn py-1 fs-8" @click="addItem()">
                    <font-awesome-icon :icon="faPlus" />
                  </button>
                </div>
              </div>
              <div class="d-flex flex-wrap gap-2">
                <select
                  v-for="(select, index) in itemProperties"
                  class="form-select fs-8 mb-1"
                  :class="{
                    'itemSelected shadow-sm': select.isSelected.value == true,
                  }"
                  :key="index"
                  v-model="select.model.value"
                  @change="updateItemClasses(index, $event.target.value)"
                >
                  <option value="">{{ select.name }}</option>
                  <option
                    v-for="(item, index) in select.options"
                    :value="item"
                    :key="index"
                  >
                    {{ item }}
                  </option>
                </select>
              </div>
            </div>
          </div>
        </div>
        <div
          class="container-preview p-4 gap-2 d-flex flex-column justify-content-center align-self-stretch"
        >
          <div class="container-header mb-1 d-flex justify-content-end">
            <ul class="nav nav-pills" id="pills-tab" role="tablist">
              <li class="nav-item" role="presentation">
                <button
                  class="nav-link rounded-start-pill active"
                  id="visual"
                  data-bs-toggle="pill"
                  data-bs-target="#visual-tab"
                  type="button"
                  role="tab"
                  aria-controls="visual-tab"
                  aria-selected="true"
                >
                  Visual
                </button>
              </li>
              <li class="nav-item" role="presentation">
                <button
                  class="nav-link rounded-end-pill"
                  id="code"
                  data-bs-toggle="pill"
                  data-bs-target="#code-tab"
                  type="button"
                  role="tab"
                  aria-controls="code-tab"
                  aria-selected="false"
                >
                  Code
                </button>
              </li>
            </ul>
          </div>
          <div
            class="tab-content flex-grow-1 d-flex justify-content-center align-items-start"
            id="pills-tabContent"
          >
            <div
              class="tab-pane fade show active"
              id="visual-tab"
              role="tabpanel"
              aria-labelledby="visual"
              tabindex="0"
            >
              <div
                class="d-flex flex-column align-items-center justify-content-center"
              >
                <h2 class="container-name fs-6">Container</h2>
                <div class="container-board d-flex" :class="containerClasses">
                  <div
                    v-for="item in itemArr"
                    class="item-card d-flex fw-semibold fs-7 text-light"
                    :class="itemClasses"
                  >
                    {{ item }}
                  </div>
                </div>
              </div>
            </div>
            <div
              class="tab-pane h-100 fade"
              id="code-tab"
              role="tabpanel"
              aria-labelledby="code"
              tabindex="0"
            >
              <h2 class="container-name fs-6">Code</h2>
              <div
                class="container-board scroll-on-hover generated-code d-flex gap-2 card p-3 px-4"
              >
                <span class="fs-7 fw-semibold pb-2 border-bottom"
                  >Bootstap Class</span
                >
                <div class="container-classes card p-2">
                  <div
                    class="container-classes-header d-flex align-items-center justify-content-between"
                  >
                    <p class="px-1 fs-8 mb-1">Container</p>
                    <button
                      class="btn fs-9 mb-1 py-1 px-3 bg-secondary-emphasis d-flex align-items-center gap-1"
                      @click="copyClasses(containerClasses)"
                    >
                      <p class="m-0">Copy</p>
                      <font-awesome-icon :icon="faCopy" class="text-center" />
                    </button>
                  </div>
                  <span class="fs-8 p-1 px-2">{{ containerClasses }}</span>
                </div>
                <div class="item-classes card p-2">
                  <div
                    class="container-classes-header d-flex align-items-center justify-content-between"
                  >
                    <p v-if="itemNumber == 1" class="px-1 fs-8 mb-1">
                      {{ `Item — ${itemNumber} element` }}
                    </p>
                    <p v-else class="px-1 fs-8 mb-1">
                      {{ `Item — ${itemNumber} elements` }}
                    </p>
                    <button
                      class="btn fs-9 mb-1 py-1 px-3 bg-secondary-emphasis d-flex align-items-center gap-1"
                      @click="copyClasses(itemClasses)"
                    >
                      <p class="m-0">Copy</p>
                      <font-awesome-icon :icon="faCopy" class="text-center" />
                    </button>
                  </div>
                  <span class="fs-8 p-1 px-2">{{ itemClasses }}</span>
                </div>

                <!--  -->
                <span class="fs-7 mt-3 fw-semibold pb-2 border-bottom"
                  >CSS Styles</span
                >
                <div class="container-classes card p-2">
                  <div
                    class="container-classes-header d-flex align-items-center justify-content-between"
                  >
                    <p class="px-1 fs-8 mb-1">Container</p>
                    <button
                      class="btn fs-9 mb-1 py-1 px-3 bg-secondary-emphasis d-flex align-items-center gap-1"
                      @click="copyClasses(convertedContainerCSS)"
                    >
                      <p class="m-0">Copy</p>
                      <font-awesome-icon :icon="faCopy" class="text-center" />
                    </button>
                  </div>
                  <span class="fs-8 p-1 px-2">{{ convertedContainerCSS }}</span>
                </div>
                <div class="container-classes card p-2">
                  <div
                    class="container-classes-header d-flex align-items-center justify-content-between"
                  >
                    <p class="px-1 fs-8 mb-1">Item</p>
                    <button
                      class="btn fs-9 mb-1 py-1 px-3 bg-secondary-emphasis d-flex align-items-center gap-1"
                      @click="copyClasses(convertedItemCSS)"
                    >
                      <p class="m-0">Copy</p>
                      <font-awesome-icon :icon="faCopy" class="text-center" />
                    </button>
                  </div>
                  <span class="fs-8 p-1 px-2">
                    {{ convertedItemCSS }}
                  </span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style setup>
#interactive-visualizer .container-fluid {
  min-height: 100vh !important;
}
#interactive-visualizer .page-content {
  min-height: 95vh !important;
}
.properties {
  flex: 1;
  border-radius: 0;
  border-color: var(--tertiary-text-color);
}
.container-header {
  height: 2rem !important;
}
.container-header .nav-link {
  font-size: 0.8rem !important;
  background-color: transparent;
  color: var(--sidebar-background-color) !important;
  border: 1px solid var(--sidebar-background-color) !important;
  font-weight: 400 !important;
}
#visual {
  padding: 0.3rem 1rem 0.3rem 1.2rem !important;
}
#code {
  padding: 0.3rem 1.2rem 0.3rem 1rem !important;
  border-left-color: transparent !important;
}
.container-header .nav-link.active {
  background-color: var(--sidebar-background-color) !important;
  color: var(--primary-text-color) !important;
}
.container-preview {
  width: 450px;
  border: 1px solid var(--tertiary-text-color);
}
.container-header,
.container-name,
.container-board {
  width: 400px !important;
}
.container-board {
  height: 400px !important;
  border: 2px solid var(--tertiary-text-color);
  border-radius: 0;
  transition: var(--transition-275s);
}
.container-properties h2,
.item-properties h2 {
  height: 2rem;
}
.container-properties h2 > svg {
  color: var(--tertiary-text-color);
}
.item-properties h2 > svg {
  color: var(--primary-color);
}
.reset-flexbox {
  color: var(--secondary-text-color);
  transition: var(--transition-175s);
}
.item-number,
.reset-flexbox {
  border: 1px solid var(--secondary-text-color);
}
.reset-flexbox:hover,
.item-number:hover {
  border-color: var(--tertiary-text-color);
}
.item-number button {
  color: var(--secondary-text-color);
}
.item-number button:active {
  border-color: transparent !important;
}
.item-number button:active svg {
  transform: scale(1.1) !important;
}
.item-number-icon {
  right: 1rem;
}
.container-properties .form-select,
.item-properties .form-select {
  width: 48.8% !important;
  color: var(--tertiary-text-color);
}
.itemSelected {
  border-color: var(--tertiary-text-color) !important;
}
.item-card {
  border-radius: 0;
  border: 2px solid var(--secondary-text-color);
  background-color: var(--primary-color);
  transition: var(--transition-275s);
}
.container-classes span,
.item-classes span {
  background-color: var(--background-color);
}
.container-classes-header button {
  transition: var(--transition-175s);
  border-radius: 3px;
}
.container-classes-header button svg {
  color: var(--secondary-text-color) !important;
}
.container-classes-header button:active {
  border-color: transparent !important;
  background-color: var(--background-color) !important;
}
.container-classes-header button:hover p,
.container-classes-header button:hover svg {
  color: var(--tertiary-text-color) !important;
}
.generated-code {
  overflow-y: scroll;
}
</style>
